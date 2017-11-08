# ToDo
npm i -g npm

# Developer Notes
Notes for day to day work.

1. [C#](#c)
2. [Asp.Net Core 1.0](#aspnet-core-10)
3. [Git](#git)
4. [SQL Server](#sql-server)
5. [WordPress](#wordpress)
6. [Let's Encrypt](#lets-encrypt)
7. [Azure](#azure)
8. [Linux](#linux)
9. [Knockout.js](#knockoutjs)
10. [Other Handy Tools](#other-handy-tools)
11. [Visual Studio 15](#visual-studio-15)
12. [Other](#other)

## C# #
- Delegate: NotifyMoving?.Invoke(Id);
 
## Asp.Net Core 1.0
- _Layout.Mobile.cshtml
- [Asp.NET Core on Ubuntu - Multihosting](https://developingsoftware.com/aspnetcore-ubuntu#configure-nginx-as-a-reverse-proxy-to-asp.net-core) 
- [ASP.NET Core Multi-tenancy](http://benfoster.io/blog/aspnet-core-multi-tenancy-data-isolation-with-entity-framework)
- [Bundling](https://github.com/Shazwazza/Smidge), [bundling from Visual Studuio](https://visualstudiogallery.msdn.microsoft.com/9ec27da7-e24b-4d56-8064-fd7e88ac1c40)
- [Custom View Engine](http://weblogs.asp.net/imranbaloch/custom-viewengine-aspnet5-mvc6), [Custom Views](http://www.davepaquette.com/archive/2015/05/04/displaying-custom-asp-net-mvc-views-per-deployment.aspx)
- [Data Encryption](https://docs.asp.net/en/latest/security/data-protection/using-data-protection.html)
- [Dependecy Injection](http://odetocode.com/blogs/scott/archive/2016/02/18/avoiding-the-service-locator-pattern-in-asp-net-core.aspx)
- [Google Capcha from CodeProject](http://www.codeproject.com/Articles/1018668/FETCH-Retrieve-data-from-a-remote-web-server-with), [Google Capcha example on YouTube](https://www.youtube.com/watch?v=HcSEU_BZwDw)
- [HangFire](http://hangfire.io/) - an easy way to perform background processing in .NET and .NET Core applications 
- [Let's Encrypt in IIS](https://weblog.west-wind.com/posts/2016/Feb/22/Using-Lets-Encrypt-with-IIS-on-Windows)
- [Login/Logout through REST](https://www.illucit.com/blog/2016/04/asp-net-5-mvc-6-identity-authentication/)
- [Mailkit](https://github.com/jstedfast/MailKit), [mail](http://stevejgordon.co.uk/how-to-send-emails-in-asp-net-core-1-0), [MailGun](http://benjii.me/2017/02/send-email-using-asp-net-core/)
- [Middleware - Sitemap](http://dotnetthoughts.net/generate-dynamic-xml-sitemaps-in-aspnet5)
- [Minify Html](https://github.com/deanhume/html-minifier)
- [MySQL + EF6](http://dan.cx/2015/08/entity-framework-6-mysql-aspnet)
- [Strongly Typed Configuration Settings](https://weblog.west-wind.com/posts/2016/May/23/Strongly-Typed-Configuration-Settings-in-ASPNET-Core)
- [Secret Manager Tool](http://www.fiyazhasan.me/dont-share-your-secrets-asp-net-core-secret-manager-tool)
- [SQL Lite + EF7](http://damienbod.com/2015/08/30/asp-net-5-with-sqlite-and-entity-framework-7/)


## Git
- git fetch origin
- git merge origin/develop
- git tag -a v1.1.0.4rtm -m "Release version"
- git push origin master
- git push --tags
- git checkout develop *..switching*
- git config --global core.autocrlf true *..on Windows*

## SQL Server
- STRING_SPLIT - returns table with a column named value (from SQL Server 2016)
```plsql
SELECT * FROM STRING_SPLIT(‘Lorem ipsum dolor sit amet.’, ”)
```
- String split - temp alternative for small strings
```plsql
SELECT Charindex(','+cast(id as varchar(8000))+',', @Ids)
```
- other handy bits
```plsql
EXEC sp_change_users_login 'UPDATE_ONE','something','something'
EXEC sp_change_users_login 'REPORT'
CREATE CREDENTIAL YourNameCredential WITH IDENTITY='YourIdentity', SECRET='YourSecretKey'
BACKUP DATABASE DatabaseName TO URL = 'AzureUrl' WITH CREDENTIAL = 'YourNameCredential', FORMAT
RESTORE DATABASE DatabaseName FROM URL ='AzureUrl' WITH CREDENTIAL = 'YourNameCredential', STATS= 5
[, MOVE 'DatabaseName' to 'x:\data\DatabaseName.mdf' ,MOVE 'DatabaseNamelog' to 'x:\data\DatabaseName.ldf'] 
```

## WordPress
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

## Let's Encrypt
- letsencrypt.exe --test
- [Installation on .Net Core](http://www.softfluent.com/blog/dev/2016/11/09/Using-Let-s-encrypt-with-ASP-NET-Core)

## Azure 
### VM
1. Create Virtual Network *..in desired location* 
2. Create Storage *..same location, no need to create a container*
3. Linux or Windows *..create D1, to get the Cloud Service and then stop it, remove or downgrade it*
4. Linux create A0 *..no need standard to get HDD 70 as image is 30GB ??*
5. Install Dynamic Compression for IIS
 
### VM Recovery
There will be a new IP Address added to the server after recovery.

1. Make sure the name remains the same
2. If the website was linked through CNAME this should not be a problem
3. IP Address needs to be updated
4. Endpoints might be different, double check them

#### Assign IP

1. Add-AzureAccount *..authenticate*
2. Get-AzureReservedIP *..list reserved ips*
3. Set-AzureReservedIPAssociation -ReservedIPName "*ReservedName*" -ServiceName "*YourServiceName*" *..assing reserved IP to the server*


### Reserve IP 
1. http://clemmblog.azurewebsites.net/convert-existing-dynamic-vip-reserved-ip-addresses-azure/
2. New-AzureReservedIP -ReservedIPName "vip01" -Location "West Europe" -ServiceName "viptest01"


## Linux
- sudo command
- df [-h] *..disk information*
- yum install htop, check-update | apt-get update, upgrade, dist-upgrade
- apt-get autoremove (removes unused packages, cleans boot partition)
- reboot -h now
- systemctl start/restart/status/stop/enable mariadb/waagent
- tab+tab auto completition
- midnight commander - Norton like Commander
- ls -la list details

### Vi Editor
 - sudo vi /var/log/mariadb/mariadb.log 
 - G - end of file
 - :q! - type to exit type
 - ZZ - exit & save
 
### cron
```30 1 * * * root /bin/systemctl stop httpd.service && (/opt/letsencrypt/letsencrypt-auto renew | tee -a /var/log/letsencrypt-renew.log) && /bin/systemctl start httpd.service```

### Knockout.js
- debug: ko.dataFor($0)

### Other Handy Tools
- http://hangfire.io/overview.html create, process and manage your background jobs
- http://fontello.com/ font extract/combine
- http://www.glyphrstudio.com/ edit svg font

### Visual Studio 15
- .editorconfig - overrides personal preferences, use in teams

### Other
- Print Spooler - restart this service
- Windows Audio
- Feedback Polls - [HotJjar] (https://www.hotjar.com/polls)
