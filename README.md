# Developer Notes
Notes for day to day work.

1. [Installation](#installation)
1. [Azure](#azure)
1. [Git](#git)
1. [SQL Server](sql-server.md)
1. [Bullhorn](#bullhorn)
1. [WordPress](#wordpress)
1. [Linux](linux.md)
1. [CSS](#css)
1. [Other](#other)

## Installation
You need to install winget e.g. through Microsof Store - App Installer.

- winget install -e --id Microsoft.WindowsTerminal
- winget install -e --id Google.Chrome
- winget install -e --id Git.Git --source winget
- winget install -e --id GitHub.GitHubDesktop
- winget install -e --id Ghisler.TotalCommander
- winget install -e --id ScooterSoftware.BeyondCompare4
- winget install -e --id IrfanSkiljan.IrfanView
- winget install -e --id Adobe.Acrobat.Reader.64-bit
- winget install -e --id OpenJS.NodeJS
- winget install -e --id Insomnia.Insomnia
- winget install -e --id Postman.Postman
- winget install -e --id Notepad++.Notepad++
- winget install -e --id voidtools.Everything
- winget install -e --id Microsoft.SQLServerManagementStudio

### Other  
- winget install -e --id Meltytech.Shotcut
- winget install -e --id OBSProject.OBSStudio
- winget install -e --id Telerik.Fiddler.Classic
- winget install -e --id Microsoft.VisualStudioCode

### Setup
- [How to configure Visual Studio to use Beyond Compare](https://stackoverflow.com/questions/4466238/how-to-configure-visual-studio-to-use-beyond-compare)
- [Options to optimize gaming performance in Windows 11](https://support.microsoft.com/en-us/windows/options-to-optimize-gaming-performance-in-windows-11-a255f612-2949-4373-a566-ff6f3f474613)

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
- git checkout develop *..switching*
- git config --global core.autocrlf true *..on Windows*

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

## Other
- Feedback Polls - [HotJjar] (https://www.hotjar.com/polls)
- Uninstall Telementry - rundll32 "%PROGRAMFILES%\NVIDIA Corporation\Installer2\InstallerCore\NVI2.DLL",UninstallPackage NvTelemetryContainer
