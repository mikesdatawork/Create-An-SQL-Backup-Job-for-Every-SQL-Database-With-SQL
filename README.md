![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Create An SQL Backup Job for Every SQL Database With SQL
**Post Date: June 9, 2006**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Create a backup job for every database. these are native backups to e:\backup\ drive, using a .txt ouput file to d:\backupreport
create a backup job for every database (using cursors of course)
these are native backups to e:\backup\ drive, using a .txt ouput file to d:\backupreport</p>      


## SQL-Logic
```SQL
declare @mydb varchar(60)
declare GetDBname cursor read_only
for
select name from sysdatabases where name not in ('tempdb')
open GetDBname
fetch next from GetDBname
into @mydb
declare @desc varchar(50)
while @@fetch_status = 0
begin
set @desc = 'FULL BACKUP ' + @MYDB
exec msdb.dbo.sp_add_job @job_name= @desc,
@enabled=1,
@notify_level_eventlog=0,
@notify_level_email=0,
@notify_level_netsend=0,
@notify_level_page=0,
@delete_level=0,
@description=@desc,
@category_name='[uncategorized (local)]',
@owner_login_name='sa'
fetch next from GetDBname
into @mydb
end
close GetDBname
deallocate GetDBname
go
/**************************************************/
declare @mydb varchar(60)
declare GetDBname cursor read_only
for
select name from sysdatabases
open GetDBname
fetch next from GetDBname
into @mydb
declare @desc varchar(50)
declare @command varchar (200)
declare @outputfile varchar (50)
while @@fetch_status = 0
begin
set @desc = 'FULL BACKUP ' + @MYDB
set @command = 'BACKUP DATABASE [' + @mydb + '] TO DISK = N"E:\Backup\' + @mydb + '.bak" WITH INIT , NOUNLOAD , NAME = N"' + @mydb + ' Backup", NOSKIP , STATS = 10, NOFORMAT'
set @outputfile = 'D:\BackupReport\' + @mydb + '.txt'
EXEC msdb.dbo.sp_add_jobstep @job_name = @desc, @step_name=N'Full Backup',
@step_id=1,
@cmdexec_success_code=0,
@on_success_action=1,
@on_success_step_id=0,
@on_fail_action=2,
@on_fail_step_id=0,
@retry_attempts=0,
@retry_interval=0,
@os_run_priority=0,
@subsystem=N'TSQL',
@command=@command,
@database_name=@mydb,
@output_file_name=@outputfile,
@flags=2
fetch next from GetDBname
into @mydb
end
close GetDBname
deallocate GetDBname
go
/**************************************************/
declare @mydb varchar (60)
declare GetDBname cursor read_only
for
select name from sysdatabases
open GetDBname
fetch next from GetDBname
into @mydb
declare @desc varchar(50)
while @@fetch_status = 0
begin
set @desc = 'FULL BACKUP ' + @mydb
EXEC msdb.dbo.sp_add_jobschedule @job_name = @desc, @name=N'Backup',
@enabled=1,
@freq_type=4,
@freq_interval=1,
@freq_subday_type=1,
@freq_subday_interval=0,
@freq_relative_interval=0,
@freq_recurrence_factor=0,
@active_start_date=20050419,
@active_end_date=99991231,
@active_start_time=0,
@active_end_time=235959
EXEC msdb.dbo.sp_add_jobserver @job_name = @desc, @server_name = N'(local)'
fetch next from GetDBname
into @mydb
end
close GetDBname
deallocate GetDBname
go
```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

