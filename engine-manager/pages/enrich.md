---
description: Enrich Rule의 Database 및 Zookeeper 설정 정보 구성 및  znode에 배포하기 위한 page.
---

# Enrich

## Options

Navigation bar 우측 상단의 triple-dot을 클릭하여 Enrich rule page의 option을 설정 할 수 있다.

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

## Table

Enrich table은 Options에 지정된 Database에서 목록을 가져와 추가, 제거, 수정 등 관리 기능 제

## Deploy

Navigation bar 우측 상단의 triple-dot을 클릭하여 Options에서 지정된 Zookeeper에 Enrich table 정보를을 배포

