---
description: 다양한 센서와 보안장비로부터 이벤트를 수집하여  엔진의 정규화, 탐지 등을 위해 Kafka로 전송합니다.
---

# Log collector

### 로그 수집

SUMiTS On-prem 엔진의 정규화 과정을 거치기 위해 보내지는 이벤트는 다양한 센서와 보안장비로부터 들어오는 이벤트입니다. 다양한 형식의 이벤트들을 수집하고 유실없이 전달하기 위해서는 Log Collector가 필요합니다.&#x20;

### Log Collector 구성

![](<../../.gitbook/assets/image (1).png>)

Log Collector는 Collect Agent 와 Aggregator로 구성됩니다.

Collect Agent와 Aggregator는 모두 Opensource를 사용합니다.&#x20;

**Collect Agent**는 각 장비별로 설치하기에 용이하기 위해서 경량화되어 있어야하고, 다양한 기능이 아니라 이벤트들을 수집하는 기능만 필요 조건으로 가지고 있기 때문에 Fluent-bit를 사용합니다. 현재 SUMiTS On-prem 에서 지원하는 이벤트 프로토콜은 `File`, `TCP/UDP`, `Http`, `Mqtt` 총 5종입니다.&#x20;

**Aggregator**는 이벤트 전달자 역할로 다양한 이벤트를 집계/처리하는 성능이 우선시되고 대용량의 이벤트를 다른 출력으로의 라우팅 기능이 중요하기 때문에 Fluentd 를 사용합니다. SUMiTS On-prem 에서는 집계/처리된 이벤트들을 Kafka에 저장하여 Normalizer가 정규화를 할 수 있도록 합니다.

