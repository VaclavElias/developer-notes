# Developer Notes
Notes for day to day work.

1. [Asp.Net Core 1.0] (aspnet-core-10)
2. [Git] (git)
3. [SQL Server] (#sql-server)
4. [WordPress] (#wordpress)
5. [Linux] (#linux)
 

## Asp.Net Core 1.0
- Mail - https://github.com/jstedfast/MailKit

## Git
- git fetch origin
- git merge origin/develop
- git tag -a v1.1.0.4rtm -m "Release version"
- git push origin master
- git push --tags
- git checkout develop *..switching*
- git config --global core.autocrlf true *..on Windows*

## SQL Server
```plsql
EXEC sp_change_users_login 'UPDATE_ONE','something','something'
EXEC sp_change_users_login 'REPORT'
CREATE CREDENTIAL YourNameCredential WITH IDENTITY='YourIdentity', SECRET='YourSecretKey'
RESTORE DATABASE DatabaseName FROM URL ='AzureUrl' WITH CREDENTIAL='YourNameCredential',STATS= 5
```

## WordPress
Must plugins.

Plugin Name | Author | Note
---|---|---
My Private Site | Jonradio Private Site (David Gewirtz) | to make it private, enable manually
WP Smush | WPMU DEV | to optimise images
Revision Control | Dion Hulse | to have only a few revisions
WP-DB-Backup |Austin Matzko | WordPress Database Backup 
Delete Expired Transients || crap stored as posts in db backup
Antivirus | Sergej Muller |
Check Email | |

## Linux
- sudo command
- df [-h] *..disk information*
- yum install htop, check-update
- reboot -h now
- systemctl start/restart/status/stop/enable mariadb/waagent
- tab+tab auto completition

### Vi Editor
 - sudo vi /var/log/mariadb/mariadb.log 
 - G - end of file
 - :q! - type to exit type
 - ZZ-exit



