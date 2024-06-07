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

### Mysql user 관련설정 

## 정리
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

-- 특정 유저의 권한 확인
SHOW GRANTS FOR 'username'@'host';
```

### Mysql CRUD

```markdown
## MySQL의 CRUD 명령어 사용법

### 1. CREATE (데이터 삽입)
```sql
-- 테이블 생성
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
);

-- 데이터 삽입
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

#### 예시
```sql
-- Users 테이블 생성
CREATE TABLE Users (
    ID INT AUTO_INCREMENT,
    Name VARCHAR(100),
    Email VARCHAR(100),
    PRIMARY KEY (ID)
);

-- 데이터 삽입
INSERT INTO Users (Name, Email)
VALUES ('John Doe', 'john.doe@example.com');
```

### 2. READ (데이터 조회)
```sql
-- 전체 데이터 조회
SELECT * FROM table_name;

-- 특정 조건의 데이터 조회
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

#### 예시
```sql
-- Users 테이블의 전체 데이터 조회
SELECT * FROM Users;

-- 특정 사용자의 데이터 조회
SELECT Name, Email FROM Users
WHERE ID = 1;
```

### 3. UPDATE (데이터 수정)
```sql
-- 데이터 수정
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

#### 예시
```sql
-- 사용자 데이터 수정
UPDATE Users
SET Email = 'new.email@example.com'
WHERE ID = 1;
```

### 4. DELETE (데이터 삭제)
```sql
-- 데이터 삭제
DELETE FROM table_name
WHERE condition;
```

#### 예시
```sql
-- 사용자 데이터 삭제
DELETE FROM Users
WHERE ID = 1;
```

### CRUD 예시 정리
#### 테이블 생성 및 데이터 삽입
```sql
CREATE TABLE Products (
    ProductID INT AUTO_INCREMENT,
    ProductName VARCHAR(100),
    Price DECIMAL(10, 2),
    PRIMARY KEY (ProductID)
);

INSERT INTO Products (ProductName, Price)
VALUES ('Laptop', 999.99),
       ('Smartphone', 499.99);
```

#### 데이터 조회
```sql
-- 전체 조회
SELECT * FROM Products;

-- 특정 조건 조회
SELECT ProductName, Price FROM Products
WHERE Price > 500;
```

#### 데이터 수정
```sql
-- 가격 수정
UPDATE Products
SET Price = 899.99
WHERE ProductID = 1;
```

#### 데이터 삭제
```sql
-- 특정 제품 삭제
DELETE FROM Products
WHERE ProductID = 2;
```
```


