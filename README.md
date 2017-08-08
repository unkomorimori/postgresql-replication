# postgresql-replication

## PostgreSQLのインストール 

### リポジトリ
```
# wget https://yum.postgresql.org/9.3/redhat/rhel-7.3-x86_64/pgdg-redhat93-9.3-3.noarch.rpm
```

### 本体のダウンロード 
```
# yum install --downloadonly --downloaddir=./ postgresql93.x86_64 \
  postgresql93-server.x86_64 \
  postgresql93-libs.x86_64 \
  postgresql93-devel.x86_64 \
  postgresql93-contrib.x86_64
```

### alternatives
```
# alternatives --install /usr/bin/pg_ctl pgsql-pg_ctl /usr/pgsql-9.3/bin/pg_ctl 220
# alternatives --install /usr/bin/initdb pgsql-initdb /usr/pgsql-9.3/bin/initdb 220
# alternatives --install /usr/bin/pg_xlogdump pgsql-pg_xlogdump /usr/pgsql-9.3/bin/pg_xlogdump 220
```

## 初期化

### initdb
```
# initdb --encoding=UTF8 --no-locale
```


## 設定変更(Master) 

### postgresql.conf 
```
$ diff postgresql.conf postgresql.conf.org
< listen_addresses = '*'
< wal_level = hot_standby
< max_wal_senders = 5
< synchronous_standby_names = '*'
< hot_standby = on
```

### recovery.done 
```
standby_mode = 'on'
primary_conninfo = 'host=192.168.11.213 port=5432 user=postgres application_name=postgres_a'
```


## スレーブ構築

### recovery.conf 
```
standby_mode = 'on'
primary_conninfo = 'host=192.168.11.212 port=5432 user=postgres application_name=postgres_b'

```


### pg_basebackup 
```
#!/bin/bash

rm -rf ./data
mkdir ./data
chmod 700 ./data
pg_basebackup -h 192.168.11.212 --progress -D ./data

rm ./data/recovery.done
cp recovery.conf ./data/

rm -rf ~/9.3/data/
mv ./data ~/9.3/
```
