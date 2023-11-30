```Powershell
# Define an array of trusted processes
$trustedProcesses = @(
    @{ ProcessName = "csrss.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "wininit.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "services.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "lsass.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "svchost.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "spoolsv.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "explorer.exe"; ExecutionLocation = "C:\Windows" },
    @{ ProcessName = "winlogon.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "taskeng.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "dwm.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "ctfmon.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "alg.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "taskhost.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "services.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "conhost.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "cmd.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "wmiprvse.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "ctfmon.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "RuntimeBroker.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "SystemSettings.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "MicrosoftEdge.exe"; ExecutionLocation = "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" },
    @{ ProcessName = "MicrosoftEdgeCP.exe"; ExecutionLocation = "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" },
    @{ ProcessName = "MicrosoftEdgeSH.exe"; ExecutionLocation = "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" },
    @{ ProcessName = "RuntimeBroker.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "SystemSettings.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "SearchUI.exe"; ExecutionLocation = "C:\Windows\SystemApps\Microsoft.Windows.Cortana_cw5n1h2txyewy" },
    @{ ProcessName = "SearchIndexer.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "svchost.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "SecurityHealthService.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "SecurityHealthSystray.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "TextInputHost.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "sihost.exe"; ExecutionLocation = "C:\Windows\System32" },
    @{ ProcessName = "Microsoft.Photos.exe"; ExecutionLocation = "C:\Program Files\WindowsApps\Microsoft.Windows.Photos_2021.21050.10022.0_x64__8wekyb3d8bbwe" }
    # Add other trusted processes here
)
# Get the list of running processes
$runningProcesses = Get-Process

# Create an array to store information about untrusted processes
$untrustedProcesses = @()

# Check each running process against the trusted processes array
foreach ($process in $runningProcesses) {
    $processName = $process.ProcessName
    $executionLocation = $process.Path

    # Check if the process is in the trusted processes array
    $trustedProcess = $trustedProcesses | Where-Object { $_.ProcessName -eq $processName }

    if (-not $trustedProcess) {
        # Process is not in the trusted list, add to the untrustedProcesses array
        $untrustedProcesses += [PSCustomObject]@{
            ProcessName = $processName
            ExecutionLocation = $executionLocation
        }
    }
}

# Display information about untrusted processes
if ($untrustedProcesses.Count -gt 0) {
    Write-Host "Untrusted Processes:"
    $untrustedProcesses | Format-Table -AutoSize
} else {
    Write-Host "No untrusted processes found."
}
```


Get GPO
``` powershell
# Get all GPOs
$gpos = Get-GPO -All

# Create an array to store GPO reports
$gpoReports = @()

# Generate a report for each GPO
foreach ($gpo in $gpos) {
    $gpoReport = Get-GPOReport -Name $gpo.DisplayName -ReportType Html
    $gpoReports += $gpoReport
}

# Combine individual reports into a single HTML file
$htmlContent = $gpoReports -join "`n`n"

# Save the HTML content to a file
$htmlContent | Out-File -FilePath "GPO_Report.html"

Write-Host "GPO information has been exported to GPO_Report.html"

```

find a file
```powershell
# Define the file name you're searching for
$fileName = "example.txt"

# List of remote computer names
$computers = @("Computer1", "Computer2", "Computer3")

foreach ($computer in $computers) {
    try {
        # Use Invoke-Command to run a script block on the remote computer
        $result = Invoke-Command -ComputerName $computer -ScriptBlock {
            param($fileName)

            # Specify the path where you want to search for the file
            $searchPath = "C:\"

            # Use Test-Path to check if the file exists
            $fileExists = Test-Path (Join-Path $searchPath $fileName) -PathType Leaf

            # Output the result
            [PSCustomObject]@{
                ComputerName = $env:COMPUTERNAME
                FileName = $fileName
                Exists = $fileExists
            }
        } -ArgumentList $fileName -ErrorAction Stop

        # Display the result
        Write-Output $result
    }
    catch {
        Write-Host "Error on $computer: $_"
    }
}

```