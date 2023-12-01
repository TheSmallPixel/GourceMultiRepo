# Gource Multi Repo
Enjoy seeing the progress you've made over all your years of development in just one short animation!
Install Gource:
```
https://gource.io/
```
Create a new shellscript *.ps1:
```
# Define the base path
$basePath = "D:\Projects" # Replace with your actual path

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
    gource --output-custom-log $logFilePath $basePath\$dir
	# Print log file creation
	Write-Host "Gource output for $dir to: $basePath\$dir"
    # Increase the counter for the next iteration
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
Run the script
```
.\\GourceCombine.ps1
```

# Enjoy 🚀
![image](https://github.com/TheSmallPixel/GourceMultiRepo/assets/25280244/f18a7e22-0cf1-44da-b49a-3fac7ccf3459)
