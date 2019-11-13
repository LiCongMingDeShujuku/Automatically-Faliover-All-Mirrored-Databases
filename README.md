![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 自动故障转移所有镜像数据库
#### Automatically Faliover All Mirrored Databases
**发布-日期: 2015年10月28日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
以下SQL逻辑将自动故障转移所有镜像数据库。它将首先将你的所有数据库设置为完全安全，然后将它们故障转移。要记住一件事，在运行代码后，你应该转到辅助服务器并使用ALTER DATABASE [MY_DATABASE] SET SAFETY OFF关闭完全安全。这将使他们回到高性能模式。

## English
The following SQL logic will automatically failover ALL your mirrored databases. It will first set all your databases to FULL SAFETY, then fail them over. One thing to keep in mind is that after you have run the code you should go to the secondary server and turn off full safety with ALTER DATABASE [MY_DATABASE] SET SAFETY OFF. This will bring them back to High Performance Mode.

---
## Logic
```SQL
use master;
set nocount on
 
declare
@set_full_safety_on_mirror_databases    varchar(max) = ''
select
@set_full_safety_on_mirror_databases    = @set_full_safety_on_mirror_databases +
'alter database [' + cast(DB_NAME(database_id) as varchar(255)) + '] set safety full;' + char(10) from
sys.database_mirroring
where
mirroring_guid is not null
and mirroring_role_desc = 'PRINCIPAL'
 
--exec (@set_full_safety_on_mirror_databases)
select  (@set_full_safety_on_mirror_databases) for xml path(''), type
 
use master;
set nocount on
 
declare
@failover_mirror_databases  varchar(max) = ''
select
@failover_mirror_databases  = @failover_mirror_databases +
'alter database [' + cast(DB_NAME(database_id) as varchar(255)) + '] set partner failover;' + char(10) + char(10) from
sys.database_mirroring
where
mirroring_guid is not null
and mirroring_role_desc = 'PRINCIPAL'
 
--exec (@failover_mirror_databases)
select  (@failover_mirror_databases) for xml path(''), type

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

