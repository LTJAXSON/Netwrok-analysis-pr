

In this writeup, im gonna do the Network Traffic Analysis Exercise in https://www.malware-traffic-analysis.net/2020/03/14/index.html which give us the network lan segment as 10.3.11.0/24 (255).

## EXECUTIVE SUMMARY:


On the date of 2020-03-12 00:24:36, a user called otis.witherspoon with a hostname LAPTOP-7XMV2SN was infected with TrickBot malware.


## DETAILS OF OUR INFECTED HOST:

- IP Address : 10.3.11.194
- Mac Address : b8:ca:3a:ec:3b:8f
- UserName : otis.witherspoon
- HostName : LAPTOP-7XMV2SN

## IOC's:

##### URL's:

|IP|Domain Name|URL|Method|Port
-----|---------|--------|--------|-----
50[.]87[.]248[.]17|bolton-tech[.]com|hxxp[://]bolton-tech[.]com/YAS20[.]exe|GET|80
64[.]44[.]133[.]131|NONE|hxxp[://]64[.]44[.]133[.]131/images/imgpaper[.]png|GET|80
64[.]44[.]133[.]131|NONE|hxxp[://]64[.]44[.]133[.]131/images/cursor[.]png|GET|80

##### Malware's:

|Name|MD5 Hash
-------|--------
|YAS20.exe|c74656862b16f81d02c109030260245b
|imgpaper.png|179345004c39336eaba39863b95f268b
|cursor.png|9dfb3952172deeddb3a5e3cafed933bc

## Analysis

First of all, we need to identify which ip belongs to whom in the network  subnet 10.3.11.0/24, as we don't have the network policy. Using the adtrace command we can pull all the ip address on the network capture pcap file(Please Note this is bad practice on large network segment).

```bash
┌──(jax㉿kali)-[~/war]
└─$ adtrace 2020-03-14-traffic-analysis-exercise.pcap|awk '{print $3}' | grep 10.3.11.* | sort -n | uniq                           
10.3.11.1
10.3.11.119
10.3.11.194
10.3.11.218
10.3.11.3
```

Then, we will use kerberos Cname String (as this is an active directory) to find the machine name of each device (Select AS-REQ **message sent to the authentication service**).
```wireshark
ip.addr == <IP> && kerberos.CNameString
```

|IP Address|Machine Name|Username
------------|---------------|-----------
10.3.11.1|LAN segment gateway|LAN segment gateway
10.3.11.119|LAPTOP-SQSA42O|dorian.neff
10.3.11.194|LAPTOP-7XMV2SN|otis.witherspoon
10.3.11.218|LAPTOP-I9ZPI04|dominga.mcdowell
10.3.11.3|Mondogreek-DC|DC

We came with 3 users in the Active directory network. After that, we will use zeek-cut to get every dns that query that happened on the network and remove our domain name as it not needed.

```bash
zeek -C -r 2020-03-14-traffic-analysis-exercise.pcap && cat dns.log| zeek-cut query | uniq | grep -v mondogreek > ../dns.txt
```

A lot of DNS names returns, so i will create a python script to upload the dns file to virustotal for analysis.

```python
import requests

import json

  

url = 'https://www.virustotal.com/vtapi/v2/url/report'

params = {'apikey': '<Your-API-Key>'}

with open('dns.txt', 'r') as f:

for line in f:

params['resource'] = line.strip()

response = requests.get(url, params=params)

try:

json_response = response.json()

except json.JSONDecodeError:

continue

if json_response['response_code'] == 1:

if json_response['positives'] > 0:

print('Malicious domain:', json_response['url'])

else:

print('Not malicious domain:', json_response['url'])

else:

print('Error:', json_response['verbose_msg'])
```

after running the script (With Some False Positive domains like www.google.com) we hit a  malicious Domains : 

|Domain Name|IP Address|port
-------|-------|-------
| bolton-tech[.]com|50[.]87[.]248[.]17|80

we immediately jump to Wireshark to investigate the traffic on http protocol.

```wireshark
ip.addr == 50.87.248.17 && http
```

And there is just one packet with the user otis.witherspoon downloading an EXE file called YAS20.exe. Before we analyze the binary we will get the time frame of when the first SYN ACK was created for later use : 

```wireshark
ip.addr == 50.87.248.17
```

The Time Frame : 2020-03-12 00:24:36


### Malware

First we will download the binary from wireshark and gets it's md5 hash.

```bash
┌──(jax㉿kali)-[~/war]
└─$ md5sum YAS20.exe 
c74656862b16f81d02c109030260245b  YAS20.exe
```

Indeed 58 security vendors and 3 sandboxes flagged this file as malicious. on looking at relations in virustotal, we spot 3 new ip address to check if there also been requested.

```wireshark
ip.addr == 170.84.78.224 or ip.addr == 181.113.28.146 or ip.addr == 91.235.129.144
```

but we are clean as wireshark return nothing. now we will upload the malware to any.run to see what IOC's we can get more from it. we see the malware drops some none printable file name : (VMMM;;;;영상ثافوزبت.exe) which drops another file called DllHost.exe for privilege escalation attack. After some research, the malware name turned to be trickbot https://www.virustotal.com/gui/collection/84d1c210b384c7c3e6bf1e02cf93fd9f8e718bd052ad86771eec4316eececbc1 .


As we have now know the  IP address of infected machine, we gonna analyze more in wireshark to see if any other request has been made.

```wireshark
ip.addr == 10.3.11.194 && http
```

We immediately spot a http request that contain a another exe files that impersonate of an png image file.

|IP Address|File Name
---------|--------
|64.44.133.131|imgpaper.png
|64.44.133.131|cursor.png

Which is another trickbot malware's that do the same as the previous malware.At the End, checking the time frame of the 2020-03-12 00:24:36 with those new connections show's that, indeed the request from the ip 50.87.248.17 was the first infection.


# Reference

- https://www.malware-traffic-analysis.net/2020/03/14/index.html
