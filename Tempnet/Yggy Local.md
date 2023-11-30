```powershell
# Test Windows Event Viewer
Write-Host "Testing Windows Event Viewer..."
try {
    Get-EventLog -LogName Application -Newest 1 | Out-Null
    Write-Host "Windows Event Viewer: Accessible"
} catch {
    Write-Host "Windows Event Viewer: Not accessible"
}

# Test Task Scheduler
Write-Host "Testing Task Scheduler..."
try {
    Get-ScheduledTask -TaskPath "\" | Out-Null
    Write-Host "Task Scheduler: Accessible"
} catch {
    Write-Host "Task Scheduler: Not accessible"
}

# Test Regedit
Write-Host "Testing Regedit..."
try {
    reg query HKLM\SOFTWARE | Out-Null
    Write-Host "Regedit: Accessible"
} catch {
    Write-Host "Regedit: Not accessible"
}

# Test Windows Management Instrumentation (WMI)
Write-Host "Testing WMI..."
try {
    Get-WmiObject -Query "SELECT * FROM Win32_ComputerSystem" | Out-Null
    Write-Host "WMI: Accessible"
} catch {
    Write-Host "WMI: Not accessible"
}

# Test PowerShell
Write-Host "Testing PowerShell..."
try {
    Get-Process | Out-Null
    Write-Host "PowerShell: Accessible"
} catch {
    Write-Host "PowerShell: Not accessible"
}

```

```powershell
# Test Windows Event Viewer
Write-Host "Testing Windows Event Viewer..."
try {
    Get-EventLog -LogName Application -Newest 1 | Out-Null
    Write-Host "Windows Event Viewer: Accessible"
} catch {
    Write-Host "Windows Event Viewer: Not accessible"
}

# Test Task Scheduler
Write-Host "Testing Task Scheduler..."
try {
    Get-ScheduledTask -TaskPath "\" | Out-Null
    Write-Host "Task Scheduler: Accessible"
} catch {
    Write-Host "Task Scheduler: Not accessible"
}

# Test Regedit
Write-Host "Testing Regedit..."
try {
    reg query HKLM\SOFTWARE | Out-Null
    Write-Host "Regedit: Accessible"
} catch {
    Write-Host "Regedit: Not accessible"
}

# Test Windows Management Instrumentation (WMI)
Write-Host "Testing WMI..."
try {
    Get-WmiObject -Query "SELECT * FROM Win32_ComputerSystem" | Out-Null
    Write-Host "WMI: Accessible"
} catch {
    Write-Host "WMI: Not accessible"
}

# Test PowerShell
Write-Host "Testing PowerShell..."
try {
    Get-Process | Out-Null
    Write-Host "PowerShell: Accessible"
} catch {
    Write-Host "PowerShell: Not accessible"
}

```
