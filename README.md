# mysqlbackup and mysqlrotatebackup
Script for backup mysql databases

This repository contain two scripts:
* mysqlbackup - regulary used with rsnapshot backup system
* mysqlrotatebackup - when rsnapshot is not in use, make the backup and take care of the backup rotation as well
## mysqlbackup usage
`mysqlbackup [-h mysqlhost] [-e mysql-extra-config-file] [dbname [dbname ...]]`
The dumps are stored on the current directory. When used with rsnapshot it copying the changed files.
When no dbname is passed, all the databases in this host are dumped
The dumps done for each table separately, and store under directory with the database's name.
A file with all the users' grants regarding the database and its tables are stored in grants.sql file within the database's directory (not gziped). For example:
```
database1
  table1.sql.gz
  table2.sql.gz
  grants.sql
database2
  table1.sql.gz
  table2.sql.gz
  grants.sql
```
## mysqlrotatebackup usage
`mysqlbackup -l backup-level [-d backup-dir] [-h mysqlhost] [-e mysql-extra-config-file] [dbname [dbname ...]]`

The backup levels are: hourly, daily, weekly, monthly and anual
The number of backups stored for each level are hard-coded in the script and can be change. The current configuration is:
no hourly backup, 6 daily backups, 4 weekly backups, 12 monthly backups and 1 anula backup.

In order to save disk space, the content of the rotated directory is copied with hard link, and only changed files are copied. This won't work if the filesystem doesn't support hard links (like NFS filesystem).

The dumps are stored in the same structure as for mysqlbackup, preceding with the snapshot directory. for example:
```
 daily.0
   database1
    table1.sql.gz
    table2.sql.gz
    grants.sql
   database2
    table1.sql.gz
    table2.sql.gz
    grants.sql
 daily.1
   database1
    table1.sql.gz
    table2.sql.gz
    grants.sql
   database2
    table1.sql.gz
    table2.sql.gz
    grants.sql
```
