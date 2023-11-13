**9/28/2022 Pastables [WICNET-U Wiki!] 

![](https://lh7-us.googleusercontent.com/uRXlm3Cgz0sTSHI8BE6OPL8Gor5D2de5Q6VsN2NmaRa6DTcwPAei_P_Fo2xR2AyGR4LbnF3Hd7b8oaRP4yjAETmo_3FtEDMALlncBTmD6xYLpdMNSmkZ8KBFMS6nF6uqH0bsybFS2buApxDcxeF_1NA)WICNET-U Wiki! 

Pastables 
-------------------------------------------------------------------
url of wiki: wiki.wicnet.af.mil 

shared drive = \\192.168.0.100\common_share shared drive = \\172.168.0.100\common_share Add target and/or * to TrustedHosts on Defender 

Set-Item WSMan:\localhost\Client\TrustedHosts -value * 

export VOLATILITY_PROFILE=<windows profile> 

export VOLATILITY_LOCATION=file:///path/to/mem.img 
-------------------------------------------------------------------
Misc. 
-------------------------------------------------------------------
#Do these things in the beginning 

Set-Item WSMan:\localhost\Client\TrustedHosts -value * 

WinRM quickconfig 

Terminal Screen Setup 

Open one terminal for copy items 

Open one terminal for interactive session (leave open) 

Get Help on finding command 

Get-Command -verb [ ] -noun [ ] 





System Internals and Metasponse

|   |   |   |   |
|---|---|---|---|
|SysInternals|Usage|Notes|Metasponse|
|streams|streams -s <dir> -accepteula|Alternate Data Streams|N/A|
|pipelist|pipelist -accepteula|Show named pipes|collectors/nulllpipes|
|psfile|psfile -accepteula|Shows remotely opened files|N/A|
|autorunsc|autorunsc <-a *> -c -m -h -accepteula|Autoruns (for user it's run as), -a * for everything, -c for csv format, -m to hide M$ things, -h to get file hashes|collectors/autoruns * to look at enabled autoruns, query for isenabled and search for enabled|
|handle|handle <-a> <-c> -u <-p [str]> -accepteula|Get file handles, -a for all, -c to close a handle, -p for all handles from a specified process. * search for “pid:” to scroll thru the file|N/A|
|tcpvcon|tcpvcon <-a> -accepteula|Advanced netstat, -a for all protocols|collectors/netstat|
|pslist/pssuspend/pskill|pslist <-t> <pid\|name> -accepteula|List processes, Suspend process, or Kill process, can be remote, -t for children processes as well|collectors/survey|
|psservice|psservice -accepteula|Get info on services, can be remote|Collectors/Survey|
|logonsessions|logonsessions -p -accepteula|Get users logged on, -p to list processes in each context|collectors/accounts|
|psloggedon|psloggedon <user> -accepteula|Search for a specific user across a domain|collectors/logins|
|psloglist|psloglist -accepteula|Look at windows event logs for different accounts, remotely|Collectors/Eventlogs|
|rootkitrevealer|rootkitrevealer -a -accepteula|Search for common rootkits|N/A|
|psinfo|psinfo -accepteula|Quick look at system info|Collectors/sysinfo|
|regjump|regjump <key> -accepteula|Regedit at specified key|collectors/regkeys - requires specific regkey inputs|

  
  

http://wiki.wicnet.af.mil/doku.php/students:21bnotes 2/7 

9/28/2022 Pastables [WICNET-U Wiki!] 

|   |   |   |   |
|---|---|---|---|
|SysInternals|Usage|Notes|Metasponse|
|strings|strings -o <-s> <file\|dir> -accepteula|Run strings on file or directory, -s for recursive|N/A|
|psexec|psexec \\remote -c <exe -args> -accepteula|Run command on remote system|N/A|
|memory|RAIR|Search for your PID if you already have that info|Rair - search for ishidden, HookMethod, IsRWX and PackedCodeBlocks to help determine if malicious|

  
  

EventID 

Powershell = 200-500 4100-4104 

Login ons = 4624 

Process CMD Line = 4688 

New Process = 106 

New Service = 7045 

Process call SeTCBPriviliege (mimikatz) = 4673 

Audit log clearing 1102 

Use .\psloglist.exe security -i <eventid>,<eventid> 

Reg Key locations 
	
	HKLM\software\microsoft\windows\currentversion\run 
	HKLM\software\microsoft\windows\currentversion\runonce 
	HKCU\software\microsoft\windows\currentversion\run 
	HKCU\software\microsoft\windows\currentversion\runonce 
	HKLM\system\currentcontrolset\services 
	HKCU\software\microsoft\windows\currentversion\policies\explorer\run 
	HKLM\software\microsoft\windows\currentversion\policies\explorer\run 

use .\autorunsc.exe -a * * > <file.txt> - the second * is to do all user contexts 

Schedule Tasks Search 

dir c:\windows\system32\Tasks\ -Recurse | sort -property CreationTime | ft CreationTime, Fullname 
	invoke-command -scriptblock {dir c:\windows\system32\Tasks\ -Recurse | sort -property CreationTime | ft CreationTime, Fullname} -session $session Command Line Options 

invoke-command -scriptblock {get-content '<last updated filepaths>'} -session $session 

Network Investigation 
	
	revolving netstat to find syn sent 
	
		netstat -anob |findstr /i syn* 
		netstat -anop 5 | select-string <PID> or <IP> 
User Investigation 
	
	To get user info on remote system 
	Psexec cmd to a remote host 
	
		.\psexec \\<ip> -u <domain>\<domain self> cmd 
		input password 
		then run net user 
		net user <target user> /domain 
		
	To get session information 
			use logonsessions in SI to pull the logged on sessions from target host 
		then look for the logon time to see if it is before the attack or during 

Network 


	


List of scripted codeblocks 

See O:\21B\Scripted Codeblocks for the codeblocks. 
	Survey Scripts + 
	Persistence + (what is listed in study material) 
	Network connections 
	netstat initial pull + 
	netstat monitor for changes 
	files that have changed on the computer + 
	Reg Key changes + 
	PSinfo + 
	strings 
	tcpvcon + 
	Clean up + 

To be completed 

Users 

	users logged in 
	user session times 
	Admins 
	User_specific query 

Event Logs 

Security 

last log time / first log time 
Search for specific eventIDs 


Linux Hunt 

show who is logged on and what they are doing 

w 

show system information 

uname -a 

show current time and date 

date 

copy a file to a remote computer 

scp /what/youwant/to/copy [username]@[ip]:/place/to/copy/filename 

copy file from a remote computer 

scp [username]@[ip]:/place/to/get/file.txt /place/to/put/file 

Invoke Commands 

	Invoke-Command -computername <ips> -Credential $cred -ScriptBlock {gci c:\ -recurse | where{$_.LastWriteTime -ge “09/12/2021”}} | export-csv file_change.csv 
	
	Invoke-Command -computername <ips> -Credential $cred -ScriptBlock {type C:\Users\<user>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt} | out file powershell_history.txt 

Fluffe++ 

	Note: when running you need to authenicate with sudo. it wont show in prompt but after running and getting error. Paste password into terminal and hit enter. Success will be had. This is a huge file. Use search for the information you want. The list of sections are listed below. 
	
	hostname 
	System_Information 
	System_uptime 
	System_Date_Time 
	Mount_Info 
	IP_config 
	ARP 
	Routes 
	who_is_logged_on 
	Exported_Shares 
	Net_Status 
	Routing_info 
	Cron_jobs 
	Active_sys_processses 
	Open_file_handles 
	Kernel_Messages 
	List_critical_directories 
	MD5SUM_binaries 
	Password_file - Users 
	user_folders 
	Sudoers_file 
	Group_File 
	File_system 
	
	ssh -t <user>@<ip> "sudo echo ----------hostname----------;hostname;echo ----------System_Information----------;uname -a;echo ----------System_uptime----------;uptime;echo ----- /opt/bitnami/apps/dokuwiki/htdocs/data/pages/students/21bnotes.txt
---------
Hunt Methodology
- Intel Driven
--> Look for specific activity of IOCs

- Behavioral Hunt
-> Looking for unusual 
---> Baseline Accounts
---> Ports/Protocols
---> Schtasks 
---> Services


Host Tasks:
	PPS
	Registry
	Autoruns
	Users/Accounts
	Schtasks
	USB
	Handles & DLLs
	

- IOC Hunt

So you found a beacon...
> Where is beacon calling from?
< IP Address or Hostname
> What process is the beacon calling from?
< PID
> What spawned the process?
< PPID
> What does the process have opened?
< handles
> What user is running the process?
< Users
>>> Where else has this user logged into?
> What other actions has the user performed?
< Prefetched
> What else is talking on this computer?
< Netstat, Mapped Shares 
> What else is running on this computer?
< Process List
> Initial Access?
< PPID 
< Email?
< Persistence?
< Psexec? 
******************************
> you found a computer with IOCs
< IP Address or Host Name
> Persistence?
< Schedule Tasks
< Services
< Autoruns 
< RegKeys
******************************
> So you're getting Desparate
< Least Frequency Analysis

Least Frequency Analysis is the practice of finding anomalies across computers 
> LFA Scripts 

LFA Via Kibana
Filter for Process/Services/Schtasks/Account - Sort by Count 
Then Filter for where that input value is at




psexec \\Remote-IP -c [Executable] -accepteula



#Schtasks:
dir c:\windows\system32\Tasks\ -Recurse | sort -property CreationTime | ft CreationTime, Fullname
invoke-command -scriptblock {dir c:\windows\system32\Tasks\ -Recurse | sort -property CreationTime | ft CreationTime, Fullname} -session $session

Windows MIP Set up

rename-computer “new-hostname”
restart-computer

Start-Service “Windows Defender Firewall”

Set-NetFirewallProfile -Profile * -Enabled True -DefaultInboundAction Block -DefaultOutboundAction Allow -AllowInboundRules False

Remove-NetFirewallRule
Remove-NetFirewallRule -Displayname “name”


New-NetFirewallRule -DisplayName “Name” -Direction <Inbound/Outbound> -LocalPort <port, port, etc> -RemoteAddress <ip, ip, ip, etc> -Protocol <tcp, udp, icmpv4, icmpv6, any> -Action <Allow/Block>

Set-NetFirewallProfile -AllowInboundRules True


Accountability:

sudo tcpdump -nn -i ens192 -w <name.pcap>


SCP Pull: scp <Username>@<IPorHost>:<PathToFile>   <LocalFileLocation>
SCP Push: scp /home/linuxhint/file2 kali@192.168.1.107:dir/


Elevated Execution Permissions / Data staging

	Setuid: find / -perm -4000 -type f -exec ls -ld {} \;
	
	Setgid: find / -user root -2000 -exec ls -ldb {} \;
	
	Setuid and Setgid: find / -user root -perm -6000 -exec ls -ldb {} \;
	
	Data staging in Linux images: find ./ -type f -name ".jpg" -o -name ".png" -o -name "*.gif"

ADS Detection

	To discover files using Alternate Data Streams (ADS), you can use the following command in Windows command prompt:
		dir /r
	This command will display all files and their associated ADS on the file system.
		dir /r /s | findstr “:$DATA”
	If you know the name of a file you would like to check for an ADS via powershell, you can use the following syntax:
		PS> Get-Item -Path .\Testing.txt -Stream 

Malicious Hidden FIles
	Use either the attrib or dir commands.
	Viewing hidden files with dir and attrib command:
	dir /ah
	attrib -s -h -r /s /d *. *

Malicious WMI Event Subscription:
	Get-WmiObject -Namespace root\subscription -Class __FilterToConsumerBinding
	
	EventID 19 User created Wmi filter
	EventID 20 User created Wmi consumer
	EventID 21 User created Wmi subscription
	
	__EventFilter is a WQL query that outlines the trigger event of interest.
	__EventConsumer is an action to perform upon triggering an event.
	__FilterToConsumerBinding is the registration mechanism that binds a filter to a consumer.