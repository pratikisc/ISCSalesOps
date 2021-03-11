### How to drop multiple tables at once
```sql
SELECT 'DROP TABLE "' + schemaname + '"."' + tablename + '";' FROM pg_tables WHERE schemaname = 'public' and tablename like ' insert name type %'

-- Generate the list and execute the command (copy the results)

```
