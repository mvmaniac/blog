---
title: MariaDB utf8mb4 설정
date: 2018-05-07 13:20:22
thumbnail: /blog/images/mariadb_t.jpg
categories: 
- DB
- MariaDB
tags:
- DB
- MariaDB
- UTF8MB4
---

0. **utf8mb4 로 설정하는 이유?**
    * 이모티콘을 테이블에 저장하려면 기존의 utf8형식으로는 저장이 안됨  
    이모티콘은 4바이트인데 mysql 나 mariadb 에서는 utf8이 3바이트로 설계가 되었기 때문, 
    만약 지원하고 싶다면 utf8mb4로 설정 해야 함
    
    <!--more-->

1. **my.ini 파일 수정**
    * 만약 ini 파일이 없으면 만들면 됨
    * 아래 내용을 추가
        ```ini <small>utf8mb4 설정</small>
        [client]  
        default-character-set=utf8mb4
        
        [mysqld] 
        collation-server=utf8mb4_unicode_ci 
        init_connect="SET collation_connection = utf8mb4_unicode_ci" 
        init_connect="SET NAMES utf8mb4" 
        character-set-server=utf8mb4
         
        [mysqldump] 
        default-character-set=utf8mb4
         
        [mysql] 
        default-character-set=utf8mb4
        ```
    
2. **확인**
    * MariaDB 설치 후 아래 명령어로 확인 ([MariaDB 설치 참고](/blog/2018/04/03/db-mariadb "MariaDB 설치 방법")) 
        ```sql <small>명령어</small>
        show variables like 'c%';
        ```
    * 아래 처럼 나오면 됨
    
    ![utf8mb 설정](/blog/images/posts/2018-05-07-db-mariadb_01.png "utf8mb 설정")