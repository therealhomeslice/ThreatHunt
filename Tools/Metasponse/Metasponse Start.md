
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