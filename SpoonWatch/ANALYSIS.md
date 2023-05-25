
## LAN SEGMENT :

- LAN segment range:  192.168.1.0/24 (192.168.1.0 through 192.168.1.255)
- Domain:  spoonwatch.net
- Domain controller:  192.168.1.9 - SPOONWATCH-DC
- LAN segment gateway:  192.168.1.1
- LAN segment broadcast address:  192.168.1.255

-------------------

**Executive Summary:**

*On 2022-01-07 19:07:32, a workstation with a host-name "DESKTOP-GXMYNO2" was infected with a credintials stealer malware known as 'OskiStealer'. Which after the user "steve.smith" requsted the C2 "2[.]56[.]57[.]108/1-7.jpg (which is a dll malware executable files)", Got his information stealed like cookie's & password etc and got send to the C2 server as a zip file :  "_5861695120.zip".

---------------------------------------


## Infected Host Details:

- Time-of-Infection : 2022-01-07 19:07:32
- IP Address : 192.168.1.216
- Mac Address : 9c:5c:8e:32:58:f9
- Host-Name : DESKTOP-GXMYNO2
- User-Name : steve.smith


--------------------------------

## Indicators of Compromise (IOCs):

- C2 : 2[.]56[.]57[.]108
- 1.jpg : e477a96c8f2b18d6b5c27bde49c990bf : sqlite3.dll
- 2.jpg : ef2834ac4ee7d6724f255beaf527e635 : freebl3.dll
- 3.jpg : 8f73c08a9660691143661bf7332c3c27 : mozglue.dll
- 4.jpg : 109f0f02fd37c84bfc7508d4227d7ed5 : msvcp140.dll
- 5.jpg : bfac4e3c5908856ba17d41edcd455a51 : nss3.dll
- 6.jpg : a2ee53de9167bf0d6c019303b7ca84e5 : softokn3.dll
- 7.jpg : 7587bf9cb4147022cd5681b015183046 : vcruntime140.dll



-------------------

## reference:

- https://www.malware-traffic-analysis.net/2022/01/07/index.html
- https://bazaar.abuse.ch/sample/c30ce79d7b5b0708dc03f1532fa89afd4efd732531cb557dc31fe63acd5bc1ce/