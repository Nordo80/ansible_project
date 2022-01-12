# Backup services SLA

# Terms
**Backup coverage** - what can be backed up, and whats not
**Backup RPO (recovery point objective)** - acceptable data loss (time period)
**Versioning and retention** - how many backup versions are stored their store period
**Usability** - how is the backup recovery verified (backup usability)
**Restoration criteria** - In which cases, backup should/could be restored
**Backup RTO (recovery time objective)** - how long will it take to restore the service

# Backup coverage
Backup service covers only databases:
Database services - **InfluxDB** and **MySQL** database.
All services, except databases, dont contain any user data, and everything can be re-created from scratch using Ansible playbook.

# RPO (recovery point objective)
RPO - 28 days

# Versioning and retention
InfluxDB Telegraf: 
Influxdbdump is done automatically every day at 02.00
The previous dump is deleted before new one is created
**Full backup** is done automatically on every Saturday at 02:20. Full backup contain all data from services that are backed up and they are done on every week
**Incremental backup** is done automaticaly every day at 02:45. Incremental backup stores only the difference in the data relative to the last incremental backup.
MySQL Agama:
MySQLdbdump is done automatically every day at 03:00 UTC. 
**Incremental backups** are done automatically every day at 03:50 UTC. **Full backups** are done on every Saturday at 03:20 UTC. 

Backups are retained for 28 days, only 28 versions of backups can be stored at the same time.
The backups that are older than 29 days are deleted at 05:00 every Saturday.

# Usability checks
Usability of the last created backup checks every 7 days on virtual environment

# Restoration criteria
Backup can be restored only in that case, when you know that needed stored data has been lost.

# RTO (recovery time objective)
Recovery time should take no more than 2 hours and half.
