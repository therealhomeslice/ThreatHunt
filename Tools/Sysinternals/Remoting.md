Remoting for SysInterals 
-------------------------------------------------------------------
$cred = get-credential 
$remote_ip = '\\IP' 
$user = '<user>' 
$passwrd = '<password>' 
Enter-psSession -computername <IP> -credential $cred 
new-pssession -computername <IP> -credential $cred 
$session = get-pssession <session #> 
Enter-psSession -Session $session 


Sysinternal Actions
-------------------------------------------------------------------

Copy-Item [C:\Path\To\item\to\move] -Destination [C:\Where\you\want\to\send\item] -ToSession $session 

[Desired SysInternal Action] > nameOfOutput.exe/csv/txt 

Copy-Item [C:\Path\to\Output\Item] -Destination [C:\Where\you\want\to\place\on\your\system] -FromSession $session 

psexec \\Remote-IP -c [Executable] -accepteula

**

