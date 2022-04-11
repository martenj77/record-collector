---
title: "Powershell Photo Organizer"
date: 2021-12-27
description: ""
categories:
  - Code
  - Photo
images:
  - /files/git01.jpg
authorname: "Mårten Johannesson"
authorimage: "/files/mj.jpg"
robots: "noindex"
---
I've started to organize all my photos from the last 20 or so years. 

<!--more-->
I found a powershell script to organize them into neat folders. However there were some problems with Swedish date format - so I made a minor edit and [posted it to a github gist](https://gist.github.com/martenj77/f410d8b71fd4b1eec728f7d600f07fbe).

The [original can be found here](https://gist.github.com/jongio/a40ea198ca5ebd85d711a7779289cc89) - many thanks [jongio](https://gist.github.com/jongio)!


### photoorganizer.ps1
```powershell
Param(
    [string]$source, 
    [string]$dest, 
    [string]$format = "yyyy/yyyy_MM/yyyy_MM_dd"
)

$shell = New-Object -ComObject Shell.Application

function Get-File-Date {
    [CmdletBinding()]
    Param (
        $object
    )

    $dir = $shell.NameSpace( $object.Directory.FullName )
    $file = $dir.ParseName( $object.Name )

    # First see if we have Date Taken, which is at index 12
    $date = Get-Date-Property-Value $dir $file 12

    if ($null -eq $date) {
        # If we don't have Date Taken, then find the oldest date from all date properties
        0..287 | ForEach-Object {
            $name = $dir.GetDetailsof($dir.items, $_)

            if ( $name -match '(date)|(created)') {
            
                # Only get value if date field because the GetDetailsOf call is expensive
                $tmp = Get-Date-Property-Value $dir $file $_
                if ( ($null -ne $tmp) -and (($null -eq $date) -or ($tmp -lt $date))) {
                    $date = $tmp
                }
            }
        }
    }
    return $date
}

function Get-Date-Property-Value {
    [CmdletBinding()]

    Param (
        $dir,
        $file,
        $index
    )

    $value = ($dir.GetDetailsof($file, $index) -replace "\u200E") -replace "\u200F" 
	
    if ($value -and $value -ne '') {
        return [DateTime]::ParseExact($value, 'yyyy/MM/dd HH:mm', $null)
    }
    return $null
}

Get-ChildItem -Attributes !Directory $source -Recurse | 
Foreach-Object {
    Write-Host "Processing $_"

    $date = Get-File-Date $_

    if ($date) {
    
        $destinationFolder = Get-Date -Date $date -Format $format
        $destinationPath = Join-Path -Path $dest -ChildPath $destinationFolder   

        # See if the destination file exists and rename until we get a unique name
        $newFullName = Join-Path -Path $destinationPath -ChildPath $_.Name
        if ($_.FullName -eq $newFullName) {
            Write-Host "Skipping: Source file and destination files are at the same location. $_"    
            return
        }

        $newNameIndex = 1
        $newName = $_.Name

        while (Test-Path -Path $newFullName) {
            $newName = ($_.BaseName + "_$newNameIndex" + $_.Extension) 
            $newFullName = Join-Path -Path $destinationPath -ChildPath $newName  
            $newNameIndex += 1   
        }

        # If we have a new name, then we need to rename in current location before moving it.
        if ($newNameIndex -gt 1) {
            Rename-Item -Path $_.FullName -NewName $newName
        }

        Write-Host "Moving $_ to $newFullName"

        # Create the destination directory if it doesn't exist
        if (!(Test-Path $destinationPath)) {
            New-Item -ItemType Directory -Force -Path $destinationPath
        }

        robocopy $_.DirectoryName $destinationPath $newName /mov
    }
}


```