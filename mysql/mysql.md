# MySQL

## How to run a live migration


### Tasks on the original server

1. First we need to create the `replica` user on the original database.

```sql
CREATE user 'repl'@'172.31.92.86' IDENTIFIED BY 'Replica2026.'
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'repl'@'172.31.92.86'
```

2. Create the dump

```bash
USER="USER"
PASSWORD="SOME_PASSWORD"

# Export only the databases that are not system ones
sudo mysql -u$USER -p$PASSWORD -N -e "SHOW DATABASES" | grep -Ev '^(information_schema|performance_schema|mysql|sys)$' > databases.txt

# Dump the databases to a compressed file
sudo mysqldump -u$USER -p$PASSWORD \
  --single-transaction \
  --routines \
  --triggers \
  --events \
  --source-data=2 \
  --databases $(cat ./databases.txt) \
  | gzip > mysql-migration.sql.gz
```

## Tasks on the new server

Suggested docker compose

```yaml
services:
  mysql:
    image: mysql:8.0.46
    container_name: mysql
    restart: unless-stopped
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - ./data:/var/lib/mysql
      - ./conf:/etc/mysql/conf.d
    command:
      - --server-id=2
      - --gtid-mode=ON
      - --enforce-gtid-consistency=ON
      - --log-bin=mysql-bin
      - --read-only=ON
      - --relay-log=mysql-relay-bin
      - --binlog-format=ROW
```



Check that the following variables are set correctly:

```sql
SHOW VARIABLES WHERE Variable_name IN
('server_id','gtid_mode','enforce_gtid_consistency','log_bin','read_only');
```

```sql
STOP REPLICA;

CHANGE REPLICATION SOURCE TO
  SOURCE_HOST = '172.31.89.33',
  SOURCE_USER = 'repl',
  SOURCE_PASSWORD = 'Replica2026.',
  SOURCE_AUTO_POSITION = 1;

START REPLICA;
-- debug
SHOW REPLICA STATUS\G
```
