# Merge pyAccess and frbcatdb and set nulls

We have merged the 2 repos pyAccess (voevent <-> DB) witht the frbcatdb (DB) into a single repo (frbcatdb)

We have modified the DB schema to make most of values default nulls

the ER for DB is now done with mysql-workbench and the SQL to create the DB can be directly exported

Now this repo (AA-ALERT-LOG) also contains a dump of the DB
