Computers-Versions-Address
	Filter: event.module : *
	Lens: Table
	Rows
		host.hostname
		host.os.name
		host.ip
	Columns:
		N/A
	Metrics
		count of records	

User Account Creations
	filter: 
		event.code : 4720 
	Lens: Table
	Rows
		user.name.keyword
		winlog.event_data.TargetUserName
		winlog.keywords
	Columns:
		N/A
	Metrics
		count of records	

net.exe and net1.exe (original PE Filename) :
	filter:  
		event.code : 1 and (process.pe.original_file_name : net.exe or process.pe.original_file_name : net1.exe)
	Lens: Table
	Rows:
		process.pe.original_file_name : net1.exe)
		user.name.keyword
	Columns:
		N/A
	Metrics
		count of records	

Failed Logins
	filter: 
		event.code : 4625
	Bar Vertical
	Horizontal Axis
		@timestamp
	Vertical Axis
		Count of @timestamp
	Breakdown By
		winlog.event_data.TargetUserName

Domain Characterization
	filter:
		N/A
	Lens: 
		Table
	Rows:
		host.name
		host.os.name
		host.ip
	Metrics
		count of records
		
Created Schedule Tasks
	filter: 
		event.code : 1 and process.name *schtasks.exe* and process.command_line : *create*
	Lens: Bar Vertical Stacked
	Horizontal Axis
		@timestamp
	Vertical Axis
		Count of Records
	Break Down By
		process.command_line

Dangerous Powershell
	filter: 
		event.code : "1" and process.executable : (*powershell* or *cmd*) AND process.command_line : (*http* OR *https* OR *invoke-webrequest* OR *expand-archive* OR *webclient* OR *ExecutionPolicy* OR *Bypass* OR *-Exec* OR *Bypass* OR *-ep* OR *-EncodedCommand* OR  *-enc* OR *-NonInteractive* OR *-NonI* OR *-NoLogo* OR *-NoProfile* OR *-NoP* OR *-WindowStyle Hidden* OR *-w Hidden* OR *decode* OR *compress-archive* OR *bitstransfer* OR *Net.WebClient* OR *DownloadFile* OR *-iex* OR *Invoke-Shellcode* OR *mpcmdrun.exe*)
	Lens: Donut
	Slice By
		process.command_line
	Size by
		count of records

Files Downloaded From Internet:
	filter:
		event.code : 15 AND event.action : "File stream created (rule: FileCreateStreamHash)"  AND *ZoneId=3*
	Rows:
		agent.name
		process.executable
		file.directory
		file.name
	Metrics
		count of records

Alternate Data Streams:
	filter:
		event.code : 15 AND event.action : "File stream created (rule: FileCreateStreamHash)"
	Rows:
		agent.name
		process.exectuable
		file.directory
		file.name
		winlog.event_data.Contents.keyword
	Metrics
		count of records

CMD/Powershell DLL Activity
	filter:
		event.code : 1 AND process.executable: (*powershell.exe* OR *cmd.exe*) AND process.command_line : *dll*
	Lens:
		table
	Rows:
		agent.name
		process.command_line
	Metrics
		count of records

Attrib Command Usage
	filter: 
		event.code : 1 AND process.command_line : *attrib*
	Rows:
		agent.name
		process.command_line
	Metrics
		count of records

*Network* Internal-External
	filter: 
		source.ip: 192.168.1.0/16 and NOT destination.ip: 192.168.0.0/16
	Rows
		source.ip
		destination.ip

Process Network Communications
	Filter:
		event.code : ("3") AND (source.ip : (192.168.1.0/24) AND NOT destination.ip : (192.168.1.0/24)) AND NOT process.executable : *.exe*
	Lens:
		Table
	Rows:
		process.executable
		destination.ip
		host.hostname
	metrics
		count of records
Event Actions LFA
	filter
		n/a
	lens:
		table
	rows
		event.action
	metrics
		count of records

Account Manipulation:
	filter: 
		event.code : ("4728" OR "4732" OR "4738" OR "5136" OR "4720")
	lens:
		table
	rows:
		event.code
		message.keyword
		winlog.event_data.TargetUserName
		host.hostname
	metrics
		count of records

Run Key Modification
	filter:
		event.code : ("12" OR "13" or "15") AND message : (run*)
	lens:
		table
	rows:
		event.code
		message.keyword
		host.hostname
	metrics
		count of records

Reg Key Mods by Users:
	filter:
		event.code : ("12" OR "13" OR "15") AND NOT user.name : *system*
	lens:
		table
	rows:
		process.executable
		winlog.event_data.TargetObject.keyword
		winlog.event_data.Details.keyword
		user.name.keyword
	metrics
		count of records

SUDO Use:
	filter:
		*.CommandLine : sudo*
	lens:
		table
	rows:
		sysmon.event_data.CommandLine.keyword
		sysmon.event_data.ParentCommandLine.keyword
		sysmon.event_data.CurrentDirectory.keyword
	metrics
		count of records

Linux Startup:
	filter:
		*.CommandLine : (*.bash OR *_log* OR *profile*)
	rows:
		sysmon.event_data.CommandLine.keyword
	metrics
		count of records	

Bitsadmin Usage:
	filter:
		event.code : ("59" OR "60" OR "1" OR "4688") AND process.executable : *bitsadmin.exe* AND *.command_line : (*create* OR *addfile* OR *setnotifyflags* OR *setnotifycmdline* OR *setminretrydelay* OR *setcustomheaders* OR *resume* OR *transfer*)
	rows:
		process.command_line
		event.code
	metrics
		count of records	

USB Usage:
	filter:
		event.code : 6416
	Lens:
		table
	rows:
		event.code
	metrics
		count of records	

Process Creates & Modified Services

New Processes and Modified Services

Cron Monitor:
	filter:
		message : (*crontab* AND *replace*)
	lens:
		table
	rows:
		host.hostname
		message.keyword
	metrics
		count of records	

DLL Search Order Hijacking:
	filter:
		event.code : ("11" OR "7" OR "1") AND process.executable : (*powershell.exe* OR *cmd.exe*) AND process.command_line : (*dll*) 
	rows:
		event.code
		process.command_line
	metrics
		count of records	

Shortcut Modifications:
	filter:
		event.code : "11" AND NOT winlog.user.domain : "NT AUTHORITY" AND file.target : (*StartUp* OR *Startup*) AND process.executable : ("cmd.exe" OR "powershell.exe" OR "wmic.exe" OR "mshta.exe" OR "pwsh.exe" OR "cscript.exe" OR "wscript.exe" OR "regsvr32.exe" OR "RegAsm.exe" OR "rundll32.exe" OR "EQNEDT32.EXE" OR "WINWORD.EXE" OR "EXCEL.EXE" OR "POWERPNT.EXE" OR "MSPUB.EXE" OR "MSACCESS.EXE" OR "iexplore.exe" OR "InstallUtil.exe") 
	rows:
		host.hostname
		process.executable
		file.target.keyword
	metrics
		count of records	
