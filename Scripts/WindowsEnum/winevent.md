```
# Set the log name and user name you want to investigate
$logName = "Security"
$userName = "USERNAME"  # Replace with the target username

# Query the Security log for Event ID 4688 related to the specified user
$events = Get-WinEvent -LogName $logName -FilterXPath "*[System[EventID=4688] and EventData[Data[@Name='NewProcessName'] and (Data='"*\\$userName\\*'")]]" -ErrorAction SilentlyContinue

# Process and display information about process creation events
foreach ($event in $events) {
    $eventXml = [xml]$event.ToXml()
    $eventData = $eventXml.Event.EventData.Data

    $processName = $eventData | Where-Object { $_.Name -eq 'NewProcessName' } | Select-Object -ExpandProperty '#text'
    $processCommandLine = $eventData | Where-Object { $_.Name -eq 'CommandLine' } | Select-Object -ExpandProperty '#text'

    Write-Host "Process Name: $processName"
    Write-Host "Command Line: $processCommandLine"
    Write-Host "--------------------------------------------------"
}

```