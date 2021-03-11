# ISCSalesOps

### How to get a list of columns in views
```sql
select t.table_schema as schema_name,
       t.table_name as view_name,
       c.column_name,
       c.data_type,
       case when c.character_maximum_length is not null
            then c.character_maximum_length
            else c.numeric_precision end as max_length,
       is_nullable
from information_schema.tables t
join information_schema.columns c 
              on t.table_schema = c.table_schema 
              and t.table_name = c.table_name
where table_type = 'VIEW' 
      and t.table_schema not in ('information_schema', 'pg_catalog')
      and t.table_name in ('sfdc-case-w0001-t0003-final-view', 'sfdc-w003-t006-splits-final-table') --Change the View Names here
      
order by schema_name,
         view_name;
         
```
### How to drop multiple tables at once
```sql
SELECT 'DROP TABLE "' + schemaname + '"."' + tablename + '";' FROM pg_tables WHERE schemaname = 'public' and tablename like ' insert name type %'

-- Generate the list and execute the command (copy the results)

```
