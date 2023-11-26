IF RSAT IS MISSING
```

enter-pssession-computer 131.7.14.2.2 -credentials mystery\Administrators
Install-windowsfeature RSAT-AD-Tools
Install-windowsfeature RSAT-AD-Powershell
Restart-computer

```
The following command will interrogate all the users of the muggle domain and allow you to look oddities with excel. 

Invoke-Command -ComputerName 131.9.2.3 -Credential $creds -ScriptBlock {get-aduser -filter * -property *} | ConvertTo-Csv | out-file muggle_users.csv


https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps
