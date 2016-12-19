# Trivia for SQL

### Add and remove index safely [Credit](http://stackoverflow.com/questions/30259196/add-index-to-table-if-it-does-not-exists)
```sql
/** add index index1 to my_table **/
set @x := (select count(*) from information_schema.statistics where table_name = 'my_table' and index_name = 'index1' and table_schema = database());
set @sql := if( @x > 0, 'select ''Index exists.''', 'ALTER TABLE my_table ADD INDEX index1 (index1);');
PREPARE stmt FROM @sql;
EXECUTE stmt;
/** add index index1 from my_table **/
set @x := (select count(*) from information_schema.statistics where table_name = 'my_table' and index_name = 'index1' and table_schema = database());
set @sql := if( @x > 0,'ALTER TABLE my_table DROP INDEX index1;', 'select ''Index not exists.''');
PREPARE stmt FROM @sql;
EXECUTE stmt;
```
