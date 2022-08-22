---
description: Collector로부터 데이터를 받는 Log Collector의 Aggregator
---

# Aggregator

## Fluent-D

Log Collector에서 aggregator로 사용하는 Fluent-D의 configuration을 정의한다.

### Configuration

필수 입력 사항이 아닌 경우 수정이 불가능하며 필수 입력 사항은 아래와 같다

* Name
* IP
* Port
* Log level
* Log rotate size
* Log rotate age

### Input / Output

Fluent-D의 Input Type은 Forward로 고정되어 있고, Output은 Kafka2로 고정되어 있다.&#x20;



{% hint style="info" %}
자세한 설정 사항은 [Aggregator - Parameter Tuning](../../../engine/log-collector/parameter-tuning/) 참조
{% endhint %}

