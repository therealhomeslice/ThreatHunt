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

-------------------------------------------------------------------
Metasponse Start 
-------------------------------------------------------------------
runas /netonly /user:[useraccount/Target Admin]@[FQDN/Target Domain] powershell cmd /c 'C:\Program Files\Metasponse\Core\metasponse.bat' 

####### In new window ####### 

powershell.exe /ExecutionPolicy Bypass “. 'C:\Program Files\Metasponse\Metasponse.Net\psprovider.ps1'” Note: this is the provider terminal - leave it alone until we pull down files or what data from a collector. 

Metasponse Actions 

use collectors/<collector> 
use transports/smb 
use transports/mswmi 
set job.rhost <remote IP> 
set -c job.rhost - clear rhost 
use -d <collector/transports> - clear collector or transport 
job status <job name> 
Initial Metasponse Pull 

Note: we can deploy all the tools are once and as they complete they will pop up in the web ui 

collectors/survey 
collectors/logins 
collectors/accounts 
collectors/autoruns 
collectors/applications 
collectors/mru 
collectors/prefetch 
collectors/netstat 
collectors/cmdhistory - may not work if cmd history isnt enabled on the box 
collectors/shimcache 
collectors/usb drives 
collectors/dnscache

transports/smb 
transports/mswmi 
analyzers/splunk 
set job.rhost 132.58.128.0/17, 132.58.88.0/23, 132.58.92.0/23 
job rename <job name> 
schedule now 

Pull File 

In Metasponse: 

Collectors/Pullfile 
find file 
transports/smb 
transports/mswmi 
set job.rhost <remote IP> 
job rename <name> 

Schedule now 

Wait a bit for completion 

In Provider window 

ls <job name> (confirm the bin file is in there) 

gci .\bin\ | Copy-BinFile -Destination <path where you want to save file> 

example: PS metasponse:\> gci metasponse:\test\prefetch\ | export-csv C:\temp\prefetch.csv 

example: PS metasponse:\> gci metasponse\test\autoruns | export-csv c:\temp\autoruns.csv 

In normal PS window 

CD to location\<remote IP> 

Files are in the folder 

SysInternals Actions 

Copy-Item [C:\Path\To\item\to\move] -Destination [C:\Where\you\want\to\send\item] -ToSession $session 

[Desired SysInternal Action] > nameOfOutput.exe/csv/txt 

Copy-Item [C:\Path\to\Output\Item] -Destination [C:\Where\you\want\to\place\on\your\system] -FromSession $session 

psexec \\Remote-IP -c [Executable] -accepteula 

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
