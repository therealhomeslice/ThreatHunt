# Establish a variable with the desired CIDR range
$cidrRange = "192.168.0.0/24"  

# Get IP addresses from the CIDR range and adds it to an array 
$ipAddresses = Get-IPAddressFromCIDR $cidrRange 
 
# For each IP in the array
foreach ($ipAddress in $ipAddresses) { 
# Write to the screen for the current IP “Checking patch level for $ipAddress..."
Write-Host "Checking patch level for $ipAddress..." 
#Ping the current IP address to check if it's reachable; if it is, try below.
$pingReply = Test-Connection -ComputerName $ipAddress -Count 1 -Quiet 
if ($pingReply) { 
 
try { 
 
#Retrieve patch information using WMI 
$os = Get-WmiObject -Class Win32_OperatingSystem -ComputerName $ipAddress -ErrorAction Stop 
$patchLevel = $os.CSDVersion 
$servicePack = $os.ServicePackMajorVersion 
 
Write-Host "Patch level for $ipAddress: $patchLevel" 
Write-Host "Service Pack for $ipAddress: $servicePack" 
} 
catch { 
Write-Host "Error retrieving patch information for $ipAddress: $_" 
} 
} 
else { 
Write-Host "$ipAddress is not reachable." 
} 
 
Write-Host 
} 
 
# Function to get IP addresses from CIDR range 
function Get-IPAddressFromCIDR { 
param ( 
[Parameter(Mandatory = $true)] 
[string]$cidrRange 
) 
 
$cidr = $cidrRange -split '/' 
$ipAddress = $cidr[0] 
$subnetMask = 0xFFFFFFFF -shl (32 - $cidr[1]) 
 
$ip = [System.Net.IPAddress]::Parse($ipAddress).GetAddressBytes() -bor $subnetMask 
 
$ipAddresses = @() 
for ($i = $ip[0]; $i -le $ip[0] + $subnetMask - 1; $i++) { 
$ipAddresses += [System.Net.IPAddress]::Parse("$([BitConverter]::ToString([BitConverter]::GetBytes($i)) -join '.')") 
} 
 
return $ipAddresses 
}


DNS Records
```powershell
function Get-ComputerNameByIP {
param(
$IPAddress = $null
)
BEGIN {
}
PROCESS {
if ($IPAddress -and $_) {
throw ‘Please use either pipeline or input parameter’
break
} elseif ($IPAddress) {
([System.Net.Dns]::GetHostbyAddress($IPAddress))
} elseif ($_) {
trap [Exception] {
write-warning $_.Exception.Message
continue;
}
[System.Net.Dns]::GetHostbyAddress($_)
} else {
$IPAddress = Read-Host “Please supply the IP Address”
[System.Net.Dns]::GetHostbyAddress($IPAddress)
}
}
END {
}
}

#Use any range you want here
1..255 | ForEach-Object {”10.20.100.$_”} | Get-ComputerNameByIP
```

Primitive PSTree

```powershell
function Print-ProcessTree() {

    function Get-ProcessAndChildProcesses($Level, $Process) {
        "{0}[{1,-5}] [{2}]" -f ("  " * $Level), $Process.ProcessId, $Process.Name
        $Children = $AllProcesses | where-object {$_.ParentProcessId -eq $Process.ProcessId -and $_.CreationDate -ge $Process.CreationDate}
        if ($null -ne $Children) {
            foreach ($Child in $Children) {
                Get-ProcessAndChildProcesses ($Level + 1) $Child
            }
        }
    }

    $AllProcesses = Get-CimInstance -ClassName "win32_process"
    $RootProcesses = @()
    # Process "System Idle Process" is processed differently, as ProcessId and ParentProcessId are 0
    # $AllProcesses is sliced from index 1 to the end of the array
    foreach ($Process in $AllProcesses[1..($AllProcesses.length-1)]) {
        $Parent = $AllProcesses | where-object {$_.ProcessId -eq $Process.ParentProcessId -and $_.CreationDate -lt $Process.CreationDate}
        if ($null -eq $Parent) {
            $RootProcesses += $Process
        }
    }
    # Process the "System Idle process" separately
    "[{0,-5}] [{1}]" -f $AllProcesses[0].ProcessId, $AllProcesses[0].Name
    foreach ($Process in $RootProcesses) {
        Get-ProcessAndChildProcesses 0 $Process
    }
}
```

TCP Connections w/Processes

```Powershell

Get-NetTCPConnection | Select-Object LocalAddress,LocalPort,RemoteAddress,RemotePort,State,OwningProcess, @{n="ProcessName"; e={( Get-Process -Id $_.OwningProcess).ProcessName}} | Format-Table


```


Color Management

```Powershell


function Format-Color([hashtable] $Colors = @{}, [switch] $SimpleMatch) {
    $lines = ($input | Out-String) -replace "`r", "" -split "`n"
    foreach($line in $lines) {
        $color = ''
        foreach($pattern in $Colors.Keys){
            if(!$SimpleMatch -and $line -match $pattern) { $color = $Colors[$pattern] }
            elseif ($SimpleMatch -and $line -like $pattern) { $color = $Colors[$pattern] }
        }
        if($color) {
            Write-Host -ForegroundColor $color $line
        } else {
            Write-Host $line
        }
    }
}


cat txtlog.log | Format-Color @{ 'Valid' = 'Red' }
```


```Powershell
et-CimInstance -ClassName Win32_Process -filter "ProcessID like 3052" | select-object *

```


```powershell
# Usage
# run script directly from powershell for quick standard checks
# 
# For quick standard checks directly from CMD:
# powershell -nologo -executionpolicy bypass -file WindowsEnum.ps1
#
# To run extensive file searches use extended parameter (it can take a long time, be patient!):
# PS C:\> .\WindowsEnum.ps1 extended
# From CMD:
# powershell -nologo -executionpolicy bypass -file WindowsEnum.ps1 extended


param($extended)
 
$lines="------------------------------------------"
function whost($a) {
    Write-Host
    Write-Host -ForegroundColor Green $lines
    Write-Host -ForegroundColor Green " "$a 
    Write-Host -ForegroundColor Green $lines
}


function Format-Color([hashtable] $Colors = @{}, [switch] $SimpleMatch) {
    $lines = ($input | Out-String) -replace "`r", "" -split "`n"
    foreach($line in $lines) {
        $color = ''
        foreach($pattern in $Colors.Keys){
            if(!$SimpleMatch -and $line -match $pattern) { $color = $Colors[$pattern] }
            elseif ($SimpleMatch -and $line -like $pattern) { $color = $Colors[$pattern] }
        }
        if($color) {
            Write-Host -ForegroundColor $color $line
        } else {
            Write-Host $line
        }
    }
}

whost "Windows Enumeration Script v 0.1
          by absolomb
       www.sploitspren.com"



$standard_commands = [ordered]@{

    'Basic System Information'                    = 'Start-Process "systeminfo" -NoNewWindow -Wait';
    'Environment Variables'                       = 'Get-ChildItem Env: | ft Key,Value';
    'Network Information'                         = 'Get-NetIPConfiguration | ft InterfaceAlias,InterfaceDescription,IPv4Address';
    'DNS Servers'                                 = 'Get-DnsClientServerAddress -AddressFamily IPv4 | ft';
    'ARP cache'                                   = 'Get-NetNeighbor -AddressFamily IPv4 | ft ifIndex,IPAddress,LinkLayerAddress,State';
    'Routing Table'                               = 'Get-NetRoute -AddressFamily IPv4 | ft DestinationPrefix,NextHop,RouteMetric,ifIndex';
    'Connected Drives'                            = 'Get-PSDrive | where {$_.Provider -like "Microsoft.PowerShell.Core\FileSystem"}| ft';
    'Firewall Config'                             = 'Start-Process "netsh" -ArgumentList "firewall show config" -NoNewWindow -Wait | ft';
    'Current User'                                = 'Write-Host $env:UserDomain\$env:UserName';
    'User Privileges'                             = 'start-process "whoami" -ArgumentList "/priv" -NoNewWindow -Wait | ft';
    'Local Users'                                 = 'Get-LocalUser | ft Name,Enabled,LastLogon';
    'Logged in Users'                             = 'Start-Process "qwinsta" -NoNewWindow -Wait | ft';
    'Credential Manager'                          = 'start-process "cmdkey" -ArgumentList "/list" -NoNewWindow -Wait | ft'
    'User Autologon Registry Items'               = 'Get-ItemProperty -Path "Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\WinLogon" | select "Default*" | ft';
    'Local Groups'                                = 'Get-LocalGroup | ft Name';
    'Local Administrators'                        = 'Get-LocalGroupMember Administrators | ft Name, PrincipalSource';
    'User Directories'                            = 'Get-ChildItem C:\Users | ft Name';
    'Searching for SAM backup files'              = 'Test-Path %SYSTEMROOT%\repair\SAM ; Test-Path %SYSTEMROOT%\system32\config\regback\SAM';
    'Running Processes'                           = 'gwmi -Query "Select * from Win32_Process" | where {$_.Name -notlike "svchost*"} | Select Name, Handle, @{Label="Owner";Expression={$_.GetOwner().User}} | ft -AutoSize';
    'Installed Software Directories'              = 'Get-ChildItem "C:\Program Files", "C:\Program Files (x86)" | ft Parent,Name,LastWriteTime';
    'Software in Registry'                        = 'Get-ChildItem -path Registry::HKEY_LOCAL_MACHINE\SOFTWARE | ft Name';
    'Folders with Everyone Permissions'           = 'Get-ChildItem "C:\Program Files\*", "C:\Program Files (x86)\*" | % { try { Get-Acl $_ -EA SilentlyContinue | Where {($_.Access|select -ExpandProperty IdentityReference) -match "Everyone"} } catch {}} | ft';
    'Folders with BUILTIN\User Permissions'       = 'Get-ChildItem "C:\Program Files\*", "C:\Program Files (x86)\*" | % { try { Get-Acl $_ -EA SilentlyContinue | Where {($_.Access|select -ExpandProperty IdentityReference) -match "BUILTIN\Users"} } catch {}} | ft';
    'Checking registry for AlwaysInstallElevated' = 'Test-Path -Path "Registry::HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Installer" | ft';
    'Unquoted Service Paths'                      = 'gwmi -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.StartMode -eq "Auto" -and $_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike ''"*''} | select PathName, DisplayName, Name | ft';
    'Scheduled Tasks'                             = 'Get-ScheduledTask | where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State';
    'Tasks Folder'                                = 'Get-ChildItem C:\Windows\Tasks | ft';
    'Startup Commands'                            = 'Get-CimInstance Win32_StartupCommand | select Name, command, Location, User | fl';
    'Process Connections'                         = 'Get-NetTCPConnection | Select-Object LocalAddress,LocalPort,RemoteAddress,RemotePort,State,OwningProcess, @{n="ProcessName"; e={( Get-Process -Id $_.OwningProcess).ProcessName}} | Format-Table | Format-Color @{ "Established" = "Yellow" } ';
}

$extended_commands = [ordered]@{

    'Searching for Unattend and Sysprep files' = 'Get-Childitem –Path C:\ -Include *unattend*,*sysprep* -File -Recurse -ErrorAction SilentlyContinue | where {($_.Name -like "*.xml" -or $_.Name -like "*.txt" -or $_.Name -like "*.ini")} | Out-File C:\temp\unattendfiles.txt';
    'Searching for web.config files'           = 'Get-Childitem –Path C:\ -Include web.config -File -Recurse -ErrorAction SilentlyContinue | Out-File C:\temp\webconfigfiles.txt';
    'Searching for other interesting files'    = 'Get-Childitem –Path C:\ -Include *password*,*cred*,*vnc* -File -Recurse -ErrorAction SilentlyContinue | Out-File C:\temp\otherfiles.txt';
    'Searching for various config files'       = 'Get-Childitem –Path C:\ -Include php.ini,httpd.conf,httpd-xampp.conf,my.ini,my.cnf -File -Recurse -ErrorAction SilentlyContinue | Out-File C:\temp\configfiles.txt'
    'Searching HKLM for passwords'             = 'reg query HKLM /f password /t REG_SZ /s | Out-File C:\temp\hklmpasswords.txt';
    'Searching HKCU for passwords'             = 'reg query HKCU /f password /t REG_SZ /s | Out-File C:\temp\hkcupasswords.txt';
    'Searching for files with passwords'       = 'Get-ChildItem c:\* -include *.xml,*.ini,*.txt,*.config -Recurse -ErrorAction SilentlyContinue | Where-Object {$_.PSPath -notlike "*C:\temp*" -and $_.PSParentPath -notlike "*Reference Assemblies*" -and $_.PSParentPath -notlike "*Windows Kits*"}| Select-String -Pattern "password" | Out-File C:\temp\password.txt';
    
}
function RunCommands($commands) {
    ForEach ($command in $commands.GetEnumerator()) {
        whost $command.Name
        Invoke-Expression $command.Value
    }
}


RunCommands($standard_commands)

if ($extended) {
    if ($extended.ToLower() -eq 'extended') {
        $result = Test-Path C:\temp
        if ($result -eq $False) {
            New-Item C:\temp -type directory
        }
        whost "Results writing to C:\temp\
    This may take a while..."
        RunCommands($extended_commands)
        whost "Script Finished! Check your files in C:\temp\"
    }
}
else {
    whost "Script finished!"
}








```