Toc: show
Parent: ../index.md
Title: Use a script to customize your build process
ShortTitle: Run scripts
ms.ContentId: 7D184F55-18BC-40E5-8BE7-283A0DB8E823

#Use a script to customize your build process

When you are ready to move beyond the basics of compiling and testing your code, use a script to add your team's business logic to your build process.

##Run a PowerShell script

You can run a PowerShell Script on a [Windows build agent](/Library/vs/alm/Build/agents/windows.md).

1.  Push your script into your repo.
2.  Add a PowerShell build step.
    ![Add step](../_shared/_img/BldStepAddBegin.png)![Add PowerShell step](_img/BldScriptPSAdd.png)
3.  Drag the build step where you want it to run.
4.  Specify the name of the script.

##Example: Version your assemblies

For example, to version to your assemblies, copy and upload this script to your team project:

```ps

##-----------------------------------------------------------------------

##<copyright file="ApplyVersionToAssemblies.ps1">(c) Microsoft Corporation. This source is subject to the Microsoft Permissive License. See http://www.microsoft.com/resources/sharedsource/licensingbasics/sharedsourcelicenses.mspx. All other rights reserved.</copyright>

##-----------------------------------------------------------------------

#Look for a 0.0.0.0 pattern in the build number.

#If found use it to version the assemblies.

#

#For example, if the 'Build number format' build process parameter

#$(BuildDefinitionName)_$(Year:yyyy).$(Month).$(DayOfMonth)$(Rev:.r)

#then your build numbers come out like this:

#"Build HelloWorld_2013.07.19.1"

#This script would then apply version 2013.07.19.1 to your assemblies.

#Enable -Verbose option

[CmdletBinding()]

#Regular expression pattern to find the version in the build number

#and then apply it to the assemblies

$VersionRegex = "\d+\.\d+\.\d+\.\d+"

#If this script is not running on a build server, remind user to

#set environment variables so that this script can be debugged

if(-not ($Env:BUILD_SOURCESDIRECTORY -and $Env:BUILD_BUILDNUMBER))
{
    Write-Error "You must set the following environment variables"
    Write-Error "to test this script interactively."
Write-Host '$Env:BUILD_SOURCESDIRECTORY - For example, enter something like:'
    Write-Host '$Env:BUILD_SOURCESDIRECTORY = "C:\code\FabrikamTFVC\HelloWorld"'
    Write-Host '$Env:BUILD_BUILDNUMBER - For example, enter something like:'
    Write-Host '$Env:BUILD_BUILDNUMBER = "Build HelloWorld_0000.00.00.0"'
    exit 1
}

#Make sure path to source code directory is available

if (-not $Env:BUILD_SOURCESDIRECTORY)
{
    Write-Error ("BUILD_SOURCESDIRECTORY environment variable is missing.")
exit 1
}
elseif (-not (Test-Path $Env:BUILD_SOURCESDIRECTORY))
{
    Write-Error "BUILD_SOURCESDIRECTORY does not exist: $Env:BUILD_SOURCESDIRECTORY"
    exit 1
}
Write-Verbose "BUILD_SOURCESDIRECTORY: $Env:BUILD_SOURCESDIRECTORY"

#Make sure there is a build number

if (-not $Env:BUILD_BUILDNUMBER)
{
    Write-Error ("BUILD_BUILDNUMBER environment variable is missing.")
exit 1
}
Write-Verbose "BUILD_BUILDNUMBER: $Env:BUILD_BUILDNUMBER"

#Get and validate the version data

$VersionData = [regex]::matches($Env:BUILD_BUILDNUMBER,$VersionRegex)
switch($VersionData.Count)
{
   0        
      { 
         Write-Error "Could not find version number data in BUILD_BUILDNUMBER."
exit 1
      }
   1 {}
   default 
      { 
         Write-Warning "Found more than instance of version data in BUILD_BUILDNUMBER."
Write-Warning "Will assume first instance is version."
}
}
$NewVersion = $VersionData[0]
Write-Verbose "Version: $NewVersion"

#Apply the version to the assembly property files

$files = gci $Env:BUILD_SOURCESDIRECTORY -recurse -include "*Properties*","My Project" | 
    ?{ $_.PSIsContainer } | 
    foreach { gci -Path $_.FullName -Recurse -include AssemblyInfo.* }
if($files)
{
    Write-Verbose "Will apply $NewVersion to $($files.count) files."

    foreach ($file in $files) {
        $filecontent = Get-Content($file)
        attrib $file -r
        $filecontent -replace $VersionRegex, $NewVersion | Out-File $file
        Write-Verbose "$file.FullName - version applied"
    }


```

Add the build step to your build definition.

![Apply version to assemblies build step](_img/BldScriptPSExmpVerAssembliesBuildStep.png)

Specify your build number with something like this:

![Build number format](_img/BldScriptPSExmpVerAssembliesBuildNumFormat.png)


```

$(BuildDefinitionName)_$(Year:yyyy).$(Month).$(DayOfMonth)$(Rev:.r)
```

##Q&A

####What variables are available for me to use in my scripts?

[Predefined variables you can use in your scripts](variables.md)

####Which branch of the script does the build run?

The build runs the script same branch of the code you are building.

####What kinds of parameters can I use?

You can use named parameters.
Other kinds of parameters, such as switch parameters, are not yet supported and will cause errors.


