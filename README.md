# Overview

SUMiTS On-prem 엔진은 각종 센서나 보안장비로 부터 이벤트를 수집합니다.&#x20;

다양한 이기종 시스템의 이벤트를 정규화 하고, 위협/재난/재해등 이벤트 탐지를 위한 룰 기반의 실시간 데이터 처리 엔진 입니다. 전용의 룰 DSL을 제공 하며 Kafka + Spark 조합의 분산 프레임워크 환경에서 동작함으로 매우 빠른 성능과 안정성, 유연한 시스템 확장 환을 제공 합니다.

![](.gitbook/assets/Architecture\_overview.drawio.png)

* Collector: 로그 수집 전송을 위한 경량 Agent와  Aggregator로 구성 됩니다. 표준 경량 Agent는 File, DB, TCP/UDP, HTTP, MQTT 프로토코롤을 지원합니다. 수집된 모든 이벤트는 Kafka에 저장됩니다 따라서 표준 Agent에서 지원하지 않는 프로토콜은 Kafka에 직접 저장하거나 Aggregate server로 직접 전송하는 커스텀 Agent를 개발하여야 합니다. Collector의 설정과 상세 Use Case는 아래를 참고 하십시오.\
  \<Collector 링크>\

* Engine: 실시간으로 유입되는 로그를 정규화 하고 이벤트를 분석하여 위협등을 탐지 합니다. 데이터 표준화 작업을 거치는 정규화 엔진/실시간 위협 탐지 엔진/임계치 기반의 탐지엔진/상관분석을 위한 그래프 탐지 엔진으로 구성되어 있습니다. 각 엔진간의 이벤트 공유와 결과의 저장은 Kafka에 저장됩니다. 엔진별 상세 동작과 설정 Use Case는 아래를 참고 하십시오\
  \<Engine 링크>\

* Config: 정규화 엔진과 탐지엔진의 동작을 위해서는 정규화 참조 정보, 정규화 룰, 탐지 룰, 설정 데이터가 필요 합니다. 데이터의 관리는 Admin UI를 통해 관리되며 Zookeeper를 이용하여 엔진과 공유 합니다. 관리 UI의 상세 동작과 설정 Use Case는 아래를 참고 하십시오\
  <관리 UI 링크>\

* Base component: 로그 수집 컴포넌트는 FluentBit, Fluentd를 사용 합니다. 엔진은 Spark kafka stream을 기반으로 동작합니다 모든 데이터의 기본 저장 큐는 Kafka를 사용하며 엔진의 동작 제어를 위해 Zookeeper를 사용 합니다.  설치 및 설정의 상세 정보는 아래를 참고 하십시오.\
  <설치 및 설정 링크>

## Step 1: Installing Engine & Collector

Bigda

## Step 2: Use Cases

TBD

## Step 3: Learn More
