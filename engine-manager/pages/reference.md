---
description: Reference Table의 Database 및 Zookeeper 설정 정보 구성 및  znode에 배포하기 위한 page
---

# Reference

## ~~Assets~~ ( -> References)

Zookeeper에 배포된 Reference table 목록

## Options

Navigation bar 우측 상단의 triple-dot을 클릭하여 Reference page의 option을 설정

#### Zookeeper options

* 접속 정보 (Connection URL): Zookeeper 접속 URL
* 최상위 경로 (Root path): Rule 단위 배포 경로
* 백업 경로 (Backup path): 백업 경로
* 적재 제한 크기 (Max node size): znode 사이즈
* 구분자 (Separator): Table column 구분자

#### Database options

* 서버 IP (Server IP): 데이터베이스 IP
* 포트 (Port): 데이터베이스 Port number
* DB 명 (Database name): 데이터베이스 이름
* 드라이버 (Driver): 데이터베이스 JDBC Driver

## Execute

SQL 쿼리를 직접 입력하여 **Options**에 지정된 Database로부터 정보를 가져와 Reference table을 생성

## Deploy

페이지 우측 상단의 Deploy 버튼을 클릭하여 데이터베이스로부터 가져온 테이블 정보를 **Options**에 지정된 Zookeeper 노드에 배포

## Index

빠른 검색을 위해 테이블 특정 Column을 index로 지정하여 별도로 Zookeeper에 저장
