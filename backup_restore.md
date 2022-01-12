#Restore process

#Install and configure infrastructure with Ansible:
ansible-playbook infra.yaml

#Restore Influxdb data from the backup:
Command to download the backup:
!!!Must be run on managed host (Nordo80-3)!!!
1. Login as backup:
    sudo su backup
2. Run command:
    rm -rf /home/backup/influxdb/*; influxd backup -portable -database telegraf /home/backup/influxdb/
3. Download backup:
    duplicity --no-encryption incremental /home/backup/influxdb/ rsync://Nordo80@backup.efiffel.ttu//home/Nordo80/influxdb/

Command to restore from backup:
!!!Must be run on managed host (Nordo80-3)!!!
1. Login as root:
    sudo -i
2. Stop telegraf service:
    service telegraf stop
3. Delete 'telegraf' database:
    influx -execute 'DROP DATABASE telegraf'
4. Restore telegraf:
    influxd restore -portable -database telegraf /home/backup/restore/influxdb/
5. Start telegraf service:
    systemctl start telegraf

Checking restore results:
1. Login as a root:
    sudo -i
2. Login to influxdb:
    influx
3. Use telegraf database:
    use telegraf;
4. Check telegraf measurements:
    show measurements;

#Restore MySQL agama data from the backup:
Command to download the backup:
!!!Must be run on managed host (Nordo80-1)!!!
1. Login as backup:
    su backup
2. Run command:
    sudo -u backup mysqldump agama > /home/backup/mysql/agama.sql
3. Download backup:
    duplicity --no-encryption incremental /home/backup/mysql/ rsync://Nordo80@backup.efiffel.ttu//home/Nordo80/mysql/

Command to restore from backup:
!!!Must be run on managed host (Nordo80-1)!!!
1. Login as root:
    sudo -i
2. Run command:
    rm -rf /home/backup/restore/mysql/*
3. Restore from backup:
    1. sudo -u backup duplicity --no-encryption restore rsync://Nordo80@backup.efiffel.ttu//home/Nordo80/mysql /home/backup/restore/mysql/
    2. mysql agama < /home/backup/restore/mysql/agama.sql

Checking restore results:
1. Login as a root:
    sudo -i
2. Login to mysql:
    mysql
3. Use agama database:
    use agama;
4. Check agama database items:
    select * from item;

# Verify that backup was successful and run ansible playbook again:
ansible-playbook infra.yaml



