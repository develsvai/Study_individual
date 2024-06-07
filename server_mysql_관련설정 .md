```markdown
## MySQL Docker Container 실행 및 설정

### 1. MySQL Docker 컨테이너 실행
```sh
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=smt32f734o@ -d -p 3306:3306 mysql:latest
```

### 2. 컨테이너 bash 접속
```sh
docker exec -it mysql-container bash
```

### 3. MySQL 접속
```sh
mysql -u root -p
```

### 4. 권한 허용
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
```

### 5. 모든 IP 허용
```sh
echo "[mysqld]" >> /etc/mysql/my.cnf
echo "bind-address = 0.0.0.0" >> /etc/mysql/my.cnf
```

### 6. 계정 비밀번호 변경
```sql
ALTER

```markdown
ALTER USER 'root'@'%' IDENTIFIED BY 'your-new-password';
```

### 7. root 권한 부여
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
```

### 8. 변경 사항 적용
```sql
FLUSH PRIVILEGES;
```

## 정리된 스크립트
```sh
# MySQL Docker 컨테이너 실행
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=smt32f734o@ -d -p 3306:3306 mysql:latest

# 컨테이너 bash 접속
docker exec -it mysql-container bash

# MySQL 접속
mysql -u root -p

# 권한 허용
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

# 모든 IP 허용
echo "[mysqld]" >> /etc/mysql/my.cnf
echo "bind-address = 0.0.0.0" >> /etc/mysql/my.cnf

# MySQL 재시작 (bash 내에서 실행)
service mysql restart

# 계정 비밀번호 변경
ALTER USER 'root'@'%' IDENTIFIED BY 'your-new-password';

# root 권한 부여
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

# 변경 사항 적용
FLUSH PRIVILEGES;
```
```
