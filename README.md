# Developer Notes
Notes for day to day work.

1. [Installation](#installation)
1. [Azure](#azure)
1. [Git](#git)
1. [SQL Server](sql-server.md)
1. [Bullhorn](#bullhorn)
1. [WordPress](#wordpress)
1. [Linux](linux.md)
2. [Hyper-V](#hyper-v)
1. [CSS](#css)

## Installation
You need to install winget e.g. through Microsof Store - App Installer.

- winget install -e --id Microsoft.WindowsTerminal
- winget install -e --id Google.Chrome
- winget install -e --id Git.Git --source winget
- winget install -e --id GitHub.GitHubDesktop
- winget install -e --id Ghisler.TotalCommander
- winget install -e --id ScooterSoftware.BeyondCompare4
- winget install -e --id IrfanSkiljan.IrfanView
  - webp plug-in not included
- winget install -e --id Adobe.Acrobat.Reader.64-bit
- winget install -e --id OpenJS.NodeJS
- winget install -e --id Insomnia.Insomnia
- winget install -e --id Postman.Postman
- winget install -e --id Notepad++.Notepad++
- winget install -e --id voidtools.Everything
- winget install -e --id Microsoft.SQLServerManagementStudio
- winget install -e --id Anthropic.ClaudeCode

### Other  
- winget install -e --id Meltytech.Shotcut
- winget install -e --id OBSProject.OBSStudio
- winget install -e --id Telerik.Fiddler.Classic
- winget install -e --id Microsoft.VisualStudioCode

### Setup
- [How to configure Visual Studio to use Beyond Compare](https://stackoverflow.com/questions/4466238/how-to-configure-visual-studio-to-use-beyond-compare)
- [Options to optimize gaming performance in Windows 11](https://support.microsoft.com/en-us/windows/options-to-optimize-gaming-performance-in-windows-11-a255f612-2949-4373-a566-ff6f3f474613)
- [Possible exclusions](https://gist.github.com/Braytiner/be2497d1a06f5a9d943dc7760693d460)

## Azure
### App Service
- London - WEBSITE_TIME_ZONE:GMT Standard Time
- Linux VM - [Add Swap](https://support.microsoft.com/en-gb/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines)
- Check Application Insight association - Logs -> customMetrics | where name == "HeartbeatState" | take 1

## Git
- git fetch origin
- git merge origin/develop
- git tag -a v1.1.0.4rtm -m "Release version"
- git push origin master
- git push --tags
- git fetch --prune
- git checkout develop *..switching*
- git config --global core.autocrlf true *..on Windows*

## Hyper-v 

### Linux

```
Get-VMSwitch | Select-Object Name, SwitchType    # find your switch name first

$vm     = 'Ubuntu 26.04'
$root   = 'D:\Data\ubuntu'
$iso    = 'D:\Data\iso\ubuntu-26.04.1-desktop-amd64.iso'
$switch = 'Live Connection'

New-VM -Name $vm -Generation 2 -MemoryStartupBytes 8GB -Path $root `
       -NewVHDPath "$root\$vm\Virtual Hard Disks\$vm.vhdx" -NewVHDSizeBytes 120GB `
       -SwitchName $switch
Set-VM -Name $vm -ProcessorCount 6 -AutomaticCheckpointsEnabled $false
Set-VMMemory -VMName $vm -DynamicMemoryEnabled $false
Set-VMFirmware -VMName $vm -SecureBootTemplate MicrosoftUEFICertificateAuthority
Add-VMDvdDrive -VMName $vm -Path $iso
Set-VMFirmware -VMName $vm -FirstBootDevice (Get-VMDvdDrive -VMName $vm)
Start-VM -Name $vm
```

Windows

```
$vm     = 'Windows 11'
$root   = 'D:\Data\windows11'
$iso    = 'D:\Data\iso\Win11_25H2_EnglishInternational_x64_v2.iso'
$switch = 'Live Connection'

New-VM -Name $vm -Generation 2 -MemoryStartupBytes 8GB -Path $root `
       -NewVHDPath "$root\$vm\Virtual Hard Disks\$vm.vhdx" -NewVHDSizeBytes 120GB `
       -SwitchName $switch

Set-VM -Name $vm -ProcessorCount 4 -AutomaticCheckpointsEnabled $false
Set-VMMemory -VMName $vm -DynamicMemoryEnabled $false

# Windows 11 needs TPM 2.0. The key protector must exist before the TPM can be turned on.
Set-VMKeyProtector -VMName $vm -NewLocalKeyProtector
Enable-VMTPM -VMName $vm

Set-VMFirmware -VMName $vm -SecureBootTemplate MicrosoftWindows -EnableSecureBoot On
Add-VMDvdDrive -VMName $vm -Path $iso
Set-VMFirmware -VMName $vm -FirstBootDevice (Get-VMDvdDrive -VMName $vm)
Enable-VMIntegrationService -VMName $vm -Name 'Guest Service Interface'

vmconnect.exe localhost $vm
Start-VM -Name $vm
```

## Bullhorn
- when running DELETE via API, it will only hard-delete entities that are hard-deletable such as Placements and Sendouts, and most entities that do not use an isDeleted field. In order to prevent important data being lost, many entities such as candidate data can only be soft-deleted via API.

## WordPress

[Ubuntu Installation](https://websiteforstudents.com/install-wordpress-on-ubuntu-16-04-17-10-18-04-with-apache2-mariadb-php-7-2-and-lets-encrypt-ssl-tls/)

Must plugins.

Plugin Name | Author | Note
---|---|---
My Private Site | David Gewirtz | to make it private, enable manually
WP Smush | WPMU DEV | to optimise images
Revision Control | Dion Hulse | to have only a few revisions
BackUpWordPress | Human Made Limited | WordPress Database Backup (Email, scheduled)
~~WP-DB-Backup~~ | Austin Matzko | WordPress Database Backup  (Email, scheduled)
Transient Cleaner | Code Art | Clean expired transients from your options table
Delete Expired Transients || crap stored as posts in db backup
Antivirus | Sergej Muller |
Check Email | |

## CSS
```css
.table-index tbody tr {
  counter-increment: rowNumber; }

.table-index tbody td:first-child {
  text-align: center; }
  .table-index tbody td:first-child::before {
    content: counter(rowNumber);
    font-size: 12px; }
```
