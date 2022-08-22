---
description: 로그 수집/연동 프로세스를 Node-Edge의 Graph 형태로 시각화하고, 수집구간 및 정규화 엔진의 룰 설정 정보를 구성하는 페이지
---

# Pipeline

## Options

### Make

구성된 Pipeline의 각 Node 별 최종 output(ex. Fluent-Bit configuration)을 다운로드 및 normalize rule 배포

### Save

pipeline 저장

### Import

JSON 형식의 파일로 Pipeline을 Import

### Export

JSON 형식의 파일로 Pipeline을 export

## Graph

### Nodes

#### Collector

데이터 수집 구간의 수집기??(ex. Fluent-Bit)에 따른 설정 정보를 구성

#### Aggregator

데이터 수집 구간의 수집기??(ex. Fluent-D)에 따른 설정 정보를 구성

#### Parser

정규화 룰 설정 정보를 구성

#### Ref

정규화 룰에 연동되는 Reference Table의 설정 정보를 구성
