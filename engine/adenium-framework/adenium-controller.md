---
description: Adenium controller는 Zookeeper와의 상호작용을 통해 Engine의 동작 제어와 참조 데이터를 관리 합니다.
---

# Adenium controller

![](../../.gitbook/assets/adenium\_controller.png)

### Adenium controller

Controller는 사용자의 Command를 수신하기 위해 Engine마다 지정된 특정 Node의 변경을 모니터링를 합니다(1). 사용자 또는 다른 시스템에서 해당 노드에 Command 메세지를 Write하면 Controller는 노드의 변경을 즉시 감지하여 명령을 파싱하고 알맞은 동작을 수행 합니다.(2) 코어 엔진별 사용하는 명령어와 Watch node는 각 엔진의 설명 페이지를 참고 하십시오.&#x20;

Controller는 효율적인 Broad cast를 위해 내부에 Reference queue를 가지고 있습니다. 이 큐의 용도는 룰 또는 참조 정보의 데이터 업데이트가 발생 했을 때 동작 중인 Application의 중단 없이 데이터를 로드하고(3) 브로드 캐스팅시 변경된 데이터만 업데이트 하는 부분 업데이트(partial update)를 지원하기 위해 고안 되었습니다.(5)

### Controller의 동작과정

1. 엔진 기동시 Main Thread와 Command listening을 위한 Monitoring Thread가 시작 됩니다.
2. Monitoring Thread는 Watch 노드에 Command 메시지가 있을 경우 메세지를 파싱하고 필요한 데이터를 로딩하여 Queue에 push 합니다.
3. Main Thread는 매 micro-batch 실행시 Queue에 새로 도작한 Message(참조 정보/룰)가 있는 지 확인하고, Message가 있을 경우, Work에 Broadcast 합니다.
4. Worker는 Broadcast된 참조 정보/룰을 이용하여 작업을 수행 합니다.

### Zookeeper Tree

Adenium은 엔진에서 사용하는 룰, 참조 데이터, 제어 명령을 zookeeper로 관리 합니다.&#x20;

최상위 노드는 "sumits"로 시작되며 znode 변경이 발생 할 경우 VersionRoot 노드로 관리 됩니다.

* sumits: 최상위 노드
* version: 버전에 따라 하위 Znode를 관리 합니다.
* watch: 엔진별 제어 명령어를 수신하기위해 모니터링 하는 노드 입니다.
* offsets: 엔진별 처리한 kafka topic의 offsets를 저장 합니다. Restore 모드시 이 오프셋을 참조 합니다.
* codes: sumits에서 사용하는 fields 테이블을 저장 합니다.
* enrich: 정규화에서 사용하는 enrich 룰
* geo\_ip: 국가코드 테이블 ( Secudium IoT 레거시 테이블 )
* nor\_rules: 정규화 룰
* ref\_tables: 레퍼런스 테이블 명으로 하위 노드를 생성 하며 레퍼런스 테이블은 각각 data 노드와 index 노드를 가지고 있습니다.
* detect\_rule: Live detect 룰
* threshold\_rule: 임계치 기반 탐지 룰
* graph\_rule: 상관 탐지&#x20;
* <mark style="color:orange;">exclude\_rule: Live detect후 제외 규칙 ( Secudium IoT 레거시 룰, 사용하지 않을 예정, 하위 호환을 위해 존재)</mark>
* <mark style="color:orange;">filter\_rule: Live detect 전 제외 규칙 ( Secudium IoT 레거시 룰, 사용하지 않을 예정, 하위 호환을 위해 존재)</mark>

<details>

<summary>Znode Tree</summary>

```
/**
   * Zookeeper Properties structure
   *
   *
   *|-- sumits
   *|   |-- VersionRoot //1.0
   *|   |   |-- app
   *|   |   |   |-- watch				
   *|   |   |   |   |-- Normalizer
   *|   |   |   |   |-- Detector
   *|   |   |   |   |-- Threshold
   *|   |   |   |-- offsets		
   *|   |   |   |   |-- Normalizer
   *|   |   |   |   |-- Detector
   *|   |   |   |   |-- Threshold
   *|   |   |   |-- common
   *|   |   |   |   |-- codes
   *|   |   |   |-- normalizer
   *|   |   |   |   |-- enriches
   *|   |   |   |   |-- geo_ip
   *|   |   |   |   |   |-- blocks
   *|   |   |   |   |   |-- country
   *|   |   |   |   |-- nor_rules
   *|   |   |   |   |-- ref_tables
   *|   |   |   |   |   |-- [Table name]
   *|   |   |   |   |   |   |-- data
   *|   |   |   |   |   |   |-- index
   *|   |   |   |-- analyzer
   *|   |   |   |   |-- detect_rule
   *|   |   |   |   |-- exclude_rule
   *|   |   |   |   |-- filter_rule
   *|	|   |   |   |-- threshold_rule
   *|	|   |	|   |-- graph_rule
   */      	        	    
```

</details>

