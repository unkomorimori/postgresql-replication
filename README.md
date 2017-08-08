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
