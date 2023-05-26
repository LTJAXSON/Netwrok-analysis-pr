
## LAN segment data:

- LAN segment range:  10.0.19.0/24 (10.0.19.0 through 10.0.19.255)
- Domain:  burnincandle.com
- Domain controller:  10.0.19.9 - BURNINCANDLE-DC
- LAN segment gateway:  10.0.19.1
- LAN segment broadcast address:  10.0.19.255



## Executive Summary:

```text
On Monday 2022-03-21 at approximately 20:58 UTC, a Host name "DESKTOP-5QS3D5D" with username "patrick.zimmerman" was infected with the malware "IcedID"(https://www.cisecurity.org/insights/white-papers/security-primer-icedid) which is a malware banking trojan that targets user financial information and is capable of acting as a dropper for other malware.However, The user was infected with IcedID (Bokbot) malware that led to Cobalt Strike.
```




## Infection Host Details:

- Time-of-infection : 
- IP Address : 10.0.19.14
- Mac Address : 00:60:52:b7:33:0f
- HostName : DESKTOP-5QS3D5D
- UserName : patrick.zimmerman



## analysis-0

```text
The malicious packet was found after expanding the first http packet and found some encoded hex data: 4445534B544F502D35515333443544 & 7061747269636B2E7A696D6D65726D616E, which turnd out to be user information : DESKTOP-5QS3D5D & patrick.zimmerman which indecate that is a IcedID malware behaviour. Full static analysis can be found here : https://sysopfb.github.io/malware,/icedid/2020/04/28/IcedIDs-updated-photoloader.html.
```

```text
on opening brim, there was a lot of traffic happing on 157[.]245[.]142[.]66, which after that, our victime patrick.zimmeman was connecting to the smb server on the IPC$, which after that, the victime connect to the "SAMR" protcole and enumrate the domain : 
```
- EnumDomains - is used to obtain a listing of all domains hosted by the server-side of the protocol.
- LookupDomain - uses the established context handle to return the SID of a domain object, given the object’s name obtained from the EnumerateDomains method.
- OpenDomain - obtains a handle to a domain object, using the SID returned by the previous _LookupDomain_ method.
- LookupNames - translates a set of account names into a set of RIDs.
- OpenGroup - obtains a handle to a domain object, given a SID.
- QueryGroupInfo
- QueryGroupMember - will query all the members that are part of Domain Admin but will only display the RID.
- LookupRids - Shows the account name of each RID that is a member of Domain Admins.
- OpenUser - obtains a handle to a user, given a RID.
- QueryUserInfo - obtains attributes from a user object using the user object handle
- QuerySecurity
- GetGroupsForUser - obtains a listing of groups that a user is a member of.
- GetAliasMembership - obtains the union of all aliases that a given set of SIDs is a member of.

## **Indicators of Compromise (IOCs)**:

### Domains-IP For IcedID:

- oceriesfornot[.]top ==> 188[.]166[.]154[.]118
- antnosience[.]com, otectagain[.]top ==> 157[.]245[.]142[.]66
- suncoastpinball[.]com, dilimoretast[.]com ===> 160[.]153[.]32[.]99
- seaskysafe[.]com ===> 91[.]193[.]16[.]181

### Suspicious traffic to file sharing domains:

- filebin[.]net ==> 185[.]47[.]40[.]36
- situla[.]bitbit[.]net ==> 87[.]238[.]33[.]8 , 87[.]238[.]33[.]7


### C2

- bupdater[.]com ==> 23[.]227[.]198[.]203


## reference:

- https://www.malware-traffic-analysis.net/2022/03/21/index3.html
- https://sysopfb.github.io/malware,/icedid/2020/04/28/IcedIDs-updated-photoloader.html
