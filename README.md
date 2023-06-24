## ToDo
- cc update
- ng build --watch

# Developer Notes
Notes for day to day work.

1. [C#](#c)
1. [Installation](installation)
1. [Asp.Net Core](#aspnet-core)
1. [Azure](#azure)
1. [Git](#git)
1. [SQL Server](sql-server.md)
1. [Bullhorn](#bullhorn)
1. [WordPress](#wordpress)
1. [Linux](linux.md)
1. [Knockout.js](#knockoutjs)
1. [Other Handy Tools](#other-handy-tools)
1. [CSS](#css)
1. [Other](#other)

## C# #
- Delegate: NotifyMoving?.Invoke(Id);

## Installation
- winget install --id Git.Git -e --source winget
- winget install -e --id GitHub.GitHubDesktop
- winget install -e --id Ghisler.TotalCommander
- winget install -e --id ScooterSoftware.BeyondCompare4
 
## Asp.Net Core
- _Layout.Mobile.cshtml
- [Asp.NET Core on Ubuntu - Multihosting](https://developingsoftware.com/aspnetcore-ubuntu#configure-nginx-as-a-reverse-proxy-to-asp.net-core) 
- [ASP.NET Core Multi-tenancy](http://benfoster.io/blog/aspnet-core-multi-tenancy-data-isolation-with-entity-framework)
- [Custom View Engine](http://weblogs.asp.net/imranbaloch/custom-viewengine-aspnet5-mvc6), [Custom Views](http://www.davepaquette.com/archive/2015/05/04/displaying-custom-asp-net-mvc-views-per-deployment.aspx)
- [Data Encryption](https://docs.asp.net/en/latest/security/data-protection/using-data-protection.html)
- [HangFire](http://hangfire.io/) - an easy way to perform background processing in .NET and .NET Core applications 
- [Mailkit](https://github.com/jstedfast/MailKit), [mail](http://stevejgordon.co.uk/how-to-send-emails-in-asp-net-core-1-0), [MailGun](http://benjii.me/2017/02/send-email-using-asp-net-core/)
- [Middleware - Sitemap](http://dotnetthoughts.net/generate-dynamic-xml-sitemaps-in-aspnet5)
- [Minify Html](https://github.com/deanhume/html-minifier)

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

## Knockout.js
- debug: ko.dataFor($0)

## Other Handy Tools
- http://hangfire.io/overview.html create, process and manage your background jobs
- http://fontello.com/ font extract/combine
- http://www.glyphrstudio.com/ edit svg font

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
