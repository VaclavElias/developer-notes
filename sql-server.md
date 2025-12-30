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
-- reorganise row_count <= 10,000 AND fragmentation > 10%
-- rebuild row_count > 10,000 AND fragmentation > 10%

SELECT 
    dbschemas.[name] AS [Schema],
    dbtables.[name] AS [Table],
    dbindexes.[name] AS [Index],
    ROUND(indexstats.avg_fragmentation_in_percent, 0) [Fragm %],
    indexstats.page_count [PageCount],
    s.row_count,
    CAST(s.used_page_count * 8.0 / 1024 AS DECIMAL(10,2)) AS IndexSizeMB,
    CASE 
        WHEN s.row_count <= 10000 THEN 'REORGANIZE'
        ELSE 'REBUILD'
    END AS [Recomm.Action],
    CASE 
        WHEN s.row_count <= 10000 THEN 
            'ALTER INDEX [' + dbindexes.[name] + '] ON [' + dbschemas.[name] + '].[' + dbtables.[name] + '] REORGANIZE;'
        ELSE 
            'ALTER INDEX [' + dbindexes.[name] + '] ON [' + dbschemas.[name] + '].[' + dbtables.[name] + '] REBUILD;' --  WITH (ONLINE = OFF)
    END AS MaintenanceCommand
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL, NULL, 'LIMITED') AS indexstats
INNER JOIN sys.tables AS dbtables 
    ON dbtables.[object_id] = indexstats.[object_id]
INNER JOIN sys.schemas AS dbschemas 
    ON dbtables.[schema_id] = dbschemas.[schema_id]
INNER JOIN sys.indexes AS dbindexes 
    ON dbindexes.[object_id] = indexstats.[object_id]
   AND indexstats.index_id = dbindexes.index_id
INNER JOIN sys.dm_db_partition_stats AS s 
    ON dbtables.[object_id] = s.[object_id]
   AND s.index_id = dbindexes.index_id
WHERE indexstats.database_id = DB_ID()
  AND dbindexes.[name] IS NOT NULL
  AND dbtables.[type_desc] = 'USER_TABLE'
  AND dbtables.[name] NOT LIKE '%dss%'
  AND indexstats.avg_fragmentation_in_percent > 10
  AND indexstats.page_count > 1000  -- Ignore very small indexes
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
-- Re-run periodically (monthly/quarterly) to catch new patterns
SELECT
    CONVERT(varchar, GETDATE(), 126) AS runtime,
    SCHEMA_NAME(o.schema_id) AS [Schema],
    OBJECT_NAME(mid.[object_id], mid.database_id) AS TableName,
    CAST(SUM(ps.used_page_count) * 8.0 / 1024 AS DECIMAL(10,2)) AS TableSizeMB,
    CONVERT(DECIMAL(28,1), migs.avg_total_user_cost * migs.avg_user_impact *
        (migs.user_seeks + migs.user_scans)) AS improvement_measure,
    migs.user_seeks,
    migs.user_scans,
    migs.avg_total_user_cost,
    migs.avg_user_impact,
    'CREATE INDEX [IX_' + OBJECT_NAME(mid.[object_id], mid.database_id) + '_' + 
        CONVERT(varchar, mig.index_group_handle) + '_' +
        CONVERT(varchar, mid.index_handle) + '] ON ' + mid.statement + '
        (' + ISNULL(mid.equality_columns, '')
        + CASE WHEN mid.equality_columns IS NOT NULL
        AND mid.inequality_columns IS NOT NULL
        THEN ', ' ELSE '' END + ISNULL(mid.inequality_columns, '') + ')'
        + ISNULL(' INCLUDE (' + mid.included_columns + ')', '') + ';' AS create_index_statement,
    mid.equality_columns,
    mid.inequality_columns,
    mid.included_columns,
    migs.unique_compiles,
    migs.last_user_seek,
    migs.last_user_scan,
    mig.index_group_handle,
    mid.index_handle
FROM sys.dm_db_missing_index_groups AS mig
INNER JOIN sys.dm_db_missing_index_group_stats AS migs
    ON migs.group_handle = mig.index_group_handle
INNER JOIN sys.dm_db_missing_index_details AS mid
    ON mig.index_handle = mid.index_handle
INNER JOIN sys.objects AS o
    ON mid.[object_id] = o.[object_id]
INNER JOIN sys.dm_db_partition_stats AS ps
    ON mid.[object_id] = ps.[object_id]
WHERE mid.database_id = DB_ID()
GROUP BY 
    o.schema_id,
    mid.[object_id],
    mid.database_id,
    migs.avg_total_user_cost,
    migs.avg_user_impact,
    migs.user_seeks,
    migs.user_scans,
    mid.statement,
    mid.equality_columns,
    mid.inequality_columns,
    mid.included_columns,
    migs.unique_compiles,
    migs.last_user_seek,
    migs.last_user_scan,
    mig.index_group_handle,
    mid.index_handle
ORDER BY improvement_measure DESC;
```
