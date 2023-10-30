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
