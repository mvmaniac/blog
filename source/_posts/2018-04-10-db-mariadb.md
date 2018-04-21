---
title: MariaDB 계정 추가 및 권한설정
date: 2018-04-10 18:13:52
thumbnail: /Blog/images/mariadb_t.jpg
categories: 
- DB
- MariaDB
tags:
- DB
- MariaDB
- 계정
---
---
##### *TEST 라는 DB가 만들어져 있다고 가정함*
---

1. **root 계정으로 로그인**
    * 관리자 권한으로 cmd 창 실행 
    * 아래 명령어 입력 후 비밀번호 입력
    ```bat <small>명령어</small>
    mysql -uroot -p mysql
    ```
    <!--more-->
    
    
2. **계정 설정**
    * 조회
    ```sql <small>명령어</small>
    SELECT HOST, USER, PASSWORD FROM USER;
    ```
    * 추가
    ```sql <small>명령어</small>
    CREATE USER [계정명]@[IP] IDENTIFIED BY '[비밀번호]';
    ```
    ```sql <small>예시</small>
    -- localhost에서만 접근 가능
    CREATE USER user@localhost IDENTIFIED BY '1234';
    
    -- '%'로 하면 모든 IP에서 접근 가능
    CREATE USER user@'%' IDENTIFIED BY '1234';
    ```
    * 삭제
    ```sql <small>명령어</small>
    DROP USER [계정명]@[IP];
    ```
    ```sql <small>예시</small>
    -- localhost인 경우
    DROP USER user@localhost
    
    -- '%'인 경우
    DROP USER user@'%' 
    ```
    
3. **권한 설정**
    * 추가
    ```sql <small>명령어</small>
    GRANT [접근권한] ON [데이터베이스명].[테이블명] TO [계정명]@[IP];
    ```
    ```sql <small>예시</small>
    -- *.* 로 하면 모든 데이터베이스와 그 데이터베이스의 모든 테이블에 모든 권한 허용 
    GRANT ALL PRIVILEGES ON *.* TO user@localhost;
    
    -- TEST 데이터베이스의 모든 테이블에 모든 권한 허용
    GRANT ALL PRIVILEGES ON test.* TO user@'%';
    
    -- TEST 데이터베이스의 모든 테이블에 SELECT, INSERT, UPDATE, DELETE 권한만 허용
    GRANT SELECT, INSERT, UPDATE, DELETE ON test.* TO user@'%';
    ```
    * 조회
    ```sql <small>명령어</small>
    SHOW GRANTS FOR [계정명]@[IP];
    ```
    ```sql <small>예시</small>
    -- localhost인 경우
    SHOW GRANTS FOR user@localhost;
    
    -- '%'인 경우
    SHOW GRANTS FOR user@'%';
    ```
    * 삭제
    ```sql <small>명령어</small>
    REVOKE [접근권한] ON [데이터베이스명].[테이블명] FROM [계정명]@[IP];
    ```
    ```sql <small>예시</small>
    -- 모든 권한 삭제 
    REVOKE ALL ON *.* TO user@localhost;
    
    -- INSERT, UPDATE, DELETE 권한만 삭제
    REVOKE INSERT, UPDATE, DELETE ON *.* TO user@localhost;
    ```
    
4. **적용**
    * 아래 명령어 실행
    ```sql <small>명령어</small>
    FLUSH PRIVILEGES;
    ```
    * INSERT, DELETE, UPDATE를 통해 사용자를 추가, 삭제, 권한 변경 등을 수행하였을 때 이 변경 사항을 반영하기 위하여 사용
    
---
##### [참고]
1. **함수를 생성하려면 아래 명령어 실행**
    ```sql <small>명령어</small>
    SET GLOBAL log_bin_trust_function_creators = ON; 
    ```