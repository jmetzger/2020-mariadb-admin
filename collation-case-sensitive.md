# Search senstive 

  * Searching sensitive is a matter of the collation 
  * for utf8 it would be ut8_bin 
  * only works if your charset is utf8 for the table/field 
  
```
select * from page where pageTitle="Something" collate utf8_bin;
alter table table_name modify field_name varchar(255) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT ''

show character set
show collation
#And all valid collations for utf8 can be found with:
show collation where charset = 'utf8'


```

# Refs 

https://stackoverflow.com/questions/901066/mysql-case-sensitive-search-for-utf8-bin-field
