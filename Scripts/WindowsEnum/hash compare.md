``` powershell
# List of known bad hashes
$knownBadHashes = @(
    "Enter known bad hash 1 here",
    "Enter known bad hash 2 here",
    "Enter known bad hash 3 here",
    # Add more known bad hashes as needed
)

# Prompt user for hash input
$userHash = Read-Host -Prompt "Enter the hash to check"

# Check if the user's hash matches any known bad hashes
if ($knownBadHashes -contains $userHash) {
    Write-Host "The entered hash is associated with a known bad file."
} else {
    Write-Host "The entered hash is not in the list of known bad hashes."
}

```


```
# Path to the text file containing known bad hashes
$hashesFilePath = "C:\Path\To\Your\Hashes.txt"

# Check if the file exists
if (Test-Path $hashesFilePath -PathType Leaf) {
    # Read the known bad hashes from the file
    $knownBadHashes = Get-Content $hashesFilePath

    # Prompt user for hash input
    $userHash = Read-Host -Prompt "Enter the hash to check"

    # Check if the user's hash matches any known bad hashes
    if ($knownBadHashes -contains $userHash) {
        Write-Host "The entered hash is associated with a known bad file."
    } else {
        Write-Host "The entered hash is not in the list of known bad hashes."
    }
} else {
    Write-Host "Error: The specified file does not exist."
}

```