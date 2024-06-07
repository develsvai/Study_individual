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


### Mysql user 관련설정 

 ```markdown
## MySQL 유저 권한 설정

### 1. 특정 데이터베이스에만 접근 허용
```sql
-- 새로운 사용자 생성 및 비밀번호 설정
CREATE USER 'username'@'host' IDENTIFIED BY 'password';

-- 특정 데이터베이스에만 접근 허용
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'host';
```

### 2. 데이터베이스 목록 보기 금지
```sql
-- 사용자에게 SHOW DATABASES 금지
REVOKE SHOW DATABASES ON *.* FROM 'username'@'host';

-- 다른 데이터베이스에 대한 접근 권한 제한
REVOKE ALL PRIVILEGES ON *.* FROM 'username'@'host';
```

### 3. 권한 적용
```sql
-- 권한 적용
FLUSH PRIVILEGES;
```

## 예시: `example_user`가 `example_db`에만 접근할 수 있도록 설정
```sh
# MySQL 접속
mysql -u root -p
```

```sql
-- example_user 생성 및 비밀번호 설정
CREATE USER 'example_user'@'%' IDENTIFIED BY 'example_password';

-- example_db에 대한 모든 권한 부여
GRANT ALL PRIVILEGES ON example_db.* TO 'example_user'@'%';

-- SHOW DATABASES 권한 금지
REVOKE SHOW DATABASES ON *.* FROM 'example_user'@'%';

-- 다른 데이터베이스에 대한 접근 권한 제한
REVOKE ALL PRIVILEGES ON *.* FROM 'example_user'@'%';

-- 권한 적용
FLUSH PRIVILEGES;
```

### 4. 참고: 권한 확인
```sql
-- 특정 유저의 권한 확인
SHOW GRANTS FOR 'username'@'host';
```
```
