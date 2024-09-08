# Hardenkitty
This is the stable version of HardeningKitty from the Windows Hardening Project by Michael Schneider. The stable version of HardeningKitty is signed with the code signing certificate of scip AG. Since this is the stable version, we do not accept pull requests in this repo, please send them to the development repo.


__________************************ Kindly Click on the Updated README.md  for step to step installation and harden *******************__________
Step to step installation and hardenOS For Click here:- https://acesse.dev/NinjaHardenKittyREADME

**##How to Run**

Run the script with administrative privileges to access machine settings. For the user settings it is better to execute them with a normal user account. Ideally, the user account is used for daily work.

Download HardeningKitty and copy it to the target system (script and lists). Then HardeningKitty can be imported and executed:

Important Step -> if any case (C:\tmp) is not exist then you can create new tmp folder.

#1  `PS C:\tmp> Import-Module .\HardeningKitty.psm1`
#2 ` PS C:\tmp> Invoke-HardeningKitty -EmojiSupport`

**##How to install
First create the directory HardeningKitty and for every version a sub directory like 0.9.2 in a path listed in the PSModulePath environment variable.

Copy the module HardeningKitty.psm1, HardeningKitty.psd1, and the lists directory to this new directory.

```
PS C:\tmp> $Version = "0.9.2"
PS C:\tmp> New-Item -Path $Env:ProgramFiles\WindowsPowerShell\Modules\HardeningKitty\$Version -ItemType Directory
PS C:\tmp> Copy-Item -Path .\HardeningKitty.psd1,.\HardeningKitty.psm1,.\lists\ -Destination $Env:ProgramFiles\WindowsPowerShell\Modules\HardeningKitty\$Version\ -Recurse
```



**### Audit**
The default mode is audit. HardeningKitty performs an audit, saves the results to a CSV file and creates a log file. The files are automatically named and receive a timestamp. Using the parameters ReportFile or LogFile, it is also possible to assign your own name and path.

The Filter parameter can be used to filter the hardening list. For this purpose the PowerShell ScriptBlock syntax must be used, for example { $_.ID -eq 4505 }. The following elements are useful for filtering: ID, Category, Name, Method, and Severity.

`Invoke-HardeningKitty -Mode Audit -Log -Report`

HardeningKitty can be executed with a specific list defined by the parameter FileFindingList. If HardeningKitty is run several times on the same system, it may be useful to hide the machine information. The parameter SkipMachineInformation is used for this purpose.

`Invoke-HardeningKitty -FileFindingList .\lists\finding_list_0x6d69636b_user.csv -SkipMachineInformation`

HardeningKitty uses the default list, and checks only tests with the severity Medium.
_____##### Kindly use this as per your requirement #####____________ 

`Invoke-HardeningKitty -Filter { $_.Severity -eq "Low" }`
or
`Invoke-HardeningKitty -Filter { $_.Severity -eq "Medium" }`
or
`Invoke-HardeningKitty -Filter { $_.Severity -eq "High" }`
_____________________________________________________________________________________________
**### Config**
The mode config retrives all current settings of a system. If a setting has not been configured, HardeningKitty will use a default value stored in the finding list. This mode can be combined with other functions, for example to create a backup.

HardeningKitty gets the current settings and stores them in a report:

`Invoke-HardeningKitty -Mode Config -Report -ReportFile C:\tmp\my_hardeningkitty_report.csv`

**##Backup**
Backups are important. Really important. Therefore, HardeningKitty also has a function to retrieve the current configuration and save it in a form that can be partially restored.

Disclaimer: HardeningKitty tries to restore the original configuration. This works quite well with registry keys and Hardening Kitty really tries its best. But the backup function is not a snapshot and does not replace a real system backup. It is not possible to restore the system 1:1 with HardeningKitty alone after HailMary. If this is a requirement, create an image or system backup and restore it.

The Backup switch specifies that the file is written in form of a finding list and can thus be used for the HailMary mode. The name and path of the backup can be specified with the parameter BackupFile.

`Invoke-HardeningKitty -Mode Config -Backup`
Please test this function to see if it really works properly on the target system before making any serious changes. A Schrödinger's backup is dangerous.

Non-Default Finding List
Note that if -FileFindingList is not specified, the backup is referred to the default finding list. Before deploying a specific list in HailMary mode, always create a backup referred to that specific list.

`Invoke-HardeningKitty -Mode Config -Backup -BackupFile ".\myBackup.csv" -FileFindingList ".\list\{list}.csv"`
Restoring a Backup
The Backup switch creates a file in form of a finding list, to restore the backup load it in HailMary mode like any find list:

`Invoke-HardeningKitty -Mode HailMary -Log -Report -FileFindingList ".\myBackup.csv"`


### Harden Kitty Benchmark score card ( Where you fall )

The formula for the HardeningKitty Score is (Points achieved / Maximum points) * 5 + 1.
___________________________________________________________________________
Score     | Rating Casual                  |     | Rating Professional.   |
- 6       | 😹 Excellent	                 |         Excellent            |
- 5	      | 😺 Well done	                 |          Good                |
- 4	      |😼 Sufficient	                 |        Sufficient            |
- 3       |😿 You should do better         |      	Insufficient          |
- 2	      |🙀 Weak                         |	Insufficient                |
- 1	      |😾 Bogus                        |   	Insufficient              |
 ___________________________________________________________________________
