## 下载ci构建

```txt
https://github.com/reigadegr/postgre_action/actions
```

## 解压到自定义目录

- 本例以`E:\Program Files\postgre`为例，文件解压到这里

- 设置环境变量`PGSQL_HOME`，值为`E:\Program Files\postgre`

- 设置环境变量`%PGSQL_HOME%\bin`到`PATH`

- 保存以下内容为bat，并以管理员权限执行。

  ```bat
  rmdir /s /q "E:\Program Files\postgre\data"
  
  Remove-Item -Recurse -Force "E:\Program Files\postgre\data"
  
  initdb.exe -D "E:\Program Files\postgre\data" -U root -E UTF8 --locale=en_US.UTF-8 --auth-host=md5
  
  cmd.exe
  ```
  此时，修改"E:\Program Files\postgre\data\pg_hba.conf":
  ```txt
  host    all             all             127.0.0.1/32            md5
  ```
  
  改为：
  ```txt
   host    all             all             127.0.0.1/32            trust
  ```
  继续执行以下内容：
  ```bat
  sc stop pgsql
  sc delete pgsql
  
  pg_ctl.exe register -N pgsql -D "E:\Program Files\postgre\data" -l "E:\Program Files\postgre\log.txt"
  
  
  sc start pgsql
  sc config pgsql start= auto
  
  cmd.exe
  ```
  继续登录，设置密码为1234，一条条执行：
  ```bat
  psql -U root -d postgres -h 127.0.0.1
  
  ALTER USER root PASSWORD '1234';\q
  ```
  验证密码生效：
  ```bat
  psql -U root -d postgres -W
  ```

