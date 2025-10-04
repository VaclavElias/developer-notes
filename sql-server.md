# SQL Server
- DBCC FREEPROCCACHE, DBCC DROPCLEANBUFFERS - reset cache
- STRING_SPLIT - returns table with a column named value (from SQL Server 2016)
```sql
SELECT * FROM STRING_SPLIT(‘Lorem ipsum dolor sit amet.’, ”)
```
- String split - temp alternative for small strings
```sql
SELECT Charindex(','+cast(id as varchar(8000))+',', @Ids)
```
- Get all triggers
```sql
SELECT name,is_instead_of_trigger FROM sys.triggers WHERE type = 'TR'
```

- other handy bits
```sql
EXEC sp_change_users_login 'UPDATE_ONE','something','something'
EXEC sp_change_users_login 'REPORT'
CREATE CREDENTIAL YourNameCredential WITH IDENTITY='YourIdentity', SECRET='YourSecretKey'
BACKUP DATABASE DatabaseName TO URL = 'AzureUrl' WITH CREDENTIAL = 'YourNameCredential', FORMAT
RESTORE DATABASE DatabaseName FROM URL ='AzureUrl' WITH CREDENTIAL = 'YourNameCredential', STATS= 5
[, MOVE 'DatabaseName' to 'x:\data\DatabaseName.mdf' ,MOVE 'DatabaseNamelog' to 'x:\data\DatabaseName.ldf'] 
SELECT percent_complete, estimated_completion_time, * FROM sys.dm_exec_requests
```
- check index fragmentation and table size
```sql
-- reorganise row_count <= 10,000
-- rebuild row_count > 10,000
-- reorganise row_count <= 10,000
-- rebuild row_count > 10,000
SELECT 
    dbschemas.[name] AS [Schema],
    dbtables.[name] AS [Table],
    dbindexes.[name] AS [Index],
    indexstats.avg_fragmentation_in_percent,
    indexstats.page_count,
    s.row_count,
    CAST(SUM(ps.used_page_count) * 8.0 / 1024 AS DECIMAL(10,2)) AS TableSizeMB
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL, NULL, NULL) AS indexstats
INNER JOIN sys.tables AS dbtables 
    ON dbtables.[object_id] = indexstats.[object_id]
INNER JOIN sys.schemas AS dbschemas 
    ON dbtables.[schema_id] = dbschemas.[schema_id]
INNER JOIN sys.indexes AS dbindexes 
    ON dbindexes.[object_id] = indexstats.[object_id]
   AND indexstats.index_id = dbindexes.index_id
JOIN sys.dm_db_partition_stats AS s 
    ON dbtables.[object_id] = s.[object_id]
   AND s.index_id IN (0,1)
JOIN sys.dm_db_partition_stats AS ps 
    ON dbtables.[object_id] = ps.[object_id]
WHERE indexstats.database_id = DB_ID()
  AND dbindexes.[name] IS NOT NULL
  AND dbtables.[type_desc] = 'USER_TABLE'
  AND dbtables.[name] NOT LIKE '%dss%'
  AND indexstats.avg_fragmentation_in_percent > 10
GROUP BY 
    dbschemas.[name],
    dbtables.[name],
    dbindexes.[name],
    indexstats.avg_fragmentation_in_percent,
    indexstats.page_count,
    s.row_count
ORDER BY indexstats.avg_fragmentation_in_percent DESC;
```
- create rebuild index queries for Azure
```sql
SELECT 'ALTER INDEX ALL ON ['+ Table_schema+'].['+Table_name+'] REBUILD;' FROM  information_schema.tables 
WHERE Table_schema!='sys'
ORDER BY Table_schema, Table_name
```
- missing indexes
```sql
SELECT
   CONVERT (varchar, getdate(), 126) AS runtime
   , mig.index_group_handle
   , mid.index_handle
   , CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
        (migs.user_seeks + migs.user_scans)) AS improvement_measure
   , 'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
        CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
        (' + ISNULL (mid.equality_columns,'')
        + CASE WHEN mid.equality_columns IS NOT NULL
        AND mid.inequality_columns IS NOT NULL
        THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '') + ')'
        + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement
   , migs.*
   , mid.database_id
   , mid.[object_id]
FROM sys.dm_db_missing_index_groups AS mig
   INNER JOIN sys.dm_db_missing_index_group_stats AS migs
      ON migs.group_handle = mig.index_group_handle
   INNER JOIN sys.dm_db_missing_index_details AS mid
      ON mig.index_handle = mid.index_handle
 ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC
```
