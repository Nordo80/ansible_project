# Backup coverage
Backup covers - **Nginx** services and **Mysql** database (update)

# RPO (recovery point objective)
Cases - hours

# Versioning and retention
We store 3 versions of backup:
**Full backup** - full backup of all data, make once a year (12.01)
Stores for 3 years
**Incremental** - Incremental backup we make once a month
Stores for year
**Differential** - Differential backup we make every week
Stores for 2 months

# Usability checks
Usability of the last created backup checks every 7 days on virtual environment

# RTO (recovery time objective)
Recovery time is around 1 hour
