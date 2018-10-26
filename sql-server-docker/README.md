# MSSQL Server for Docker

Quickstart setup: https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-2017

## Backing up and Restoring Databases
Further information can be found [here](https://docs.microsoft.com/en-us/sql/linux/tutorial-restore-backup-in-sql-server-container?view=sql-server-2017) about restoring databases on Docker

### Backing up
```SQL
BACKUP DATABASE [<name_of_database>] 
TO DISK = N'/var/opt/mssql/backup/<name_of_file></name_of_file>.bak' WITH NOFORMAT, NOINIT, NAME = '<name_of_file>', SKIP, NOREWIND, NOUNLOAD, STATS = 10
```

### Restoring

1. Find the file names and paths which are inside the backup
	```SQL
	RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/<name_of_file>.bak'
	```
2. Use these file names to specify the new paths for each of the files listed in the previous step
	```SQL
	RESTORE DATABASE WideWorldImporters FROM DISK = '/var/opt/mssql/backup/<name_of_file>.bak' 
	WITH 
		MOVE '<mdf_logical_name>' TO '<mdf_physical_name_file_path>', 
		MOVE '<ndf_logical_name>' TO '<ndf_physical_name_file_path>', 
		MOVE '<ldf_logical_name>' TO '<ldf_physical_name_file_path>', 
		MOVE '<mdf_logical_name>' TO '<mdf_physical_name_file_path>',
		MOVE 'InMemory_Data_1' TO '<InMemory_Data_1_file_path>'
	```
	(NB:: If a file isn't included within the 1st step's results, you don't have to include it)
