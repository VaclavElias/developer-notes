# Developer Notes
Notes for day to day work.

1. [Asp.Net Core 1.0] (asp.netcore1.0)
2. [Git] (git)
3. [SQL Server] (#sqlserver)
4. [WordPress] (#wordpress)
 

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

## SQL Serveer
- EXEC sp_change_users_login 'UPDATE_ONE','something','something'
- EXEC sp_change_users_login 'REPORT'

- CREATE CREDENTIAL *YourNameCredential* WITH IDENTITY='*YourIdentity*', SECRET='*YourSecretKey*';
- RESTORE DATABASE *DatabaseName* FROM URL ='*AzureUrl*' WITH CREDENTIAL='*YourNameCredential*',STATS= 5



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


