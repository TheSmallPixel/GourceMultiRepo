# GourceMultiRepo
Gource shell script to combine all repository changes in one file
```
# Define the base path
$basePath = "D:\Projects" 

# Get all directories in the base path
$directories = Get-ChildItem -Path $basePath -Directory

# Initialize a counter for naming log files
$logCounter = 1

# Iterate through each directory and run gource command
foreach ($dir in $directories) {
    # Change to the directory
    Set-Location $dir.FullName
    # Run the gource command
	$logFilePath = "$basePath\gource$logCounter.txt"
    gource   --output-custom-log $logFilePath $basePath\$dir
    $logCounter++
}

# Change back to the base path
Set-Location $basePath

# Combine and sort all log files
Get-ChildItem -Filter "gource*.txt" | 
    Foreach-Object { Get-Content $_.FullName } | 
    Sort-Object { $_.Split("|")[0] -as [int] } | 
    Set-Content "combined.txt"

# Run gource with the combined log file
gource combined.txt --seconds-per-day 0.01

```
