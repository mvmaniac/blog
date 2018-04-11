---
title: MariaDB zip 파일 설치 및 실행 (Windows 10 Pro x64)
date: 2018-04-03 23:55:49
thumbnail: /blog/images/mariadb_t.jpg
categories: 
- DB
- MariaDB
tags:
- DB
- MariaDB
- zip
- 포터블
- 설치
---
---
##### *설치 경로는 Y:\MariaDB\10.2.14 아래에 한다고 가정함*
---

1. **해당 링크 클릭 [MariaDB Download](https://downloads.mariadb.org/ "MariaDB Download")**
    * 최신 버전으로 다운로드 버튼 클릭 <small>(현재 10.2.14)</small>
    * PackageType 은 **ZIP file** 로 받을 것
    
    <!--more-->
    
2. **압축을 풀고자 하는 디렉터리로 이동 후 압축 풀기**

3. **환경변수 설정**
    * 추가
        * 변수 명 : MARIADB_HOME
        * 변수 값 : `Y:\MariaDB\10.2.14`
        
    * 편집
        * 변수 명 : Path
        * 변수 값 : `%MARIADB_HOME%\bin`
        
4. **윈도우 서비스 등록 및 DB 데이터 초기화**
    * 메모장에 아래 명령어를 넣은 후 확장자를 .bat 으로 한 배치 파일 생성
        ```bat <small>명령어</small>
        %MARIADB_HOME%\bin\mysql_install_db.exe --datadir=[DB 데이터 폴더 경로] --service=[윈도우 서비스 명] --port=[포트번호 (기본은 3306)] --password=[root 계정 비밀번호]    
        ```
        ```bat <small>예시 (공백이 있는 경우 ""로 묶어줌)</small>
        %MARIADB_HOME%\bin\mysql_install_db.exe --datadir=%MARIADB_HOME%\data_user --service="MariaDB 10.2.14" --port=5306 --password=1234
        ```
    * 생성한 배치 파일을 관리자 권한으로 실행
        
5. **ini 파일 설정**
    * 기본적으로 4번에서 --datadir 옵션 값으로 설정한 경로에 my.ini 파일이 생성되어 있음
    * my-huge.ini, my-large.ini 등등 중 하나를 선택하여 my.ini 파일에 기존 내용은 유지하면서 붙여넣기 하면 됨
 
6. MariaDB 실행
    * 윈도우 실행 창에서 services.msc 입력
    * 4번에서 --service 옵션 값으로 설정한 서비스 명을 찾아서 서비스 시작
 
---
##### [참고]
1. **윈도우 서비스 제거**
   * 관리자 권한으로 cmd 창 실행 후 아래 명령어 입력
    ```bat <small>명령어</small>
    sc delete [윈도우 서비스 명]    
    ```
    ```bat <small>예시</small>
    sc delete "MariaDB 10.2.14"  
    ```
