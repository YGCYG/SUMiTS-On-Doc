---
description: 이 장에서는 엔진에서 사용하는 Field에 대해서 설명 합니다.
---

# Field

### Field

필드는 위협 로그에서 추출한 의미 있는 구문을 저장하고 표현하기 위한 단위 이며 정규화/탐 이벤트를 구성하는 가장 작은 단위의 데이터 구조 입니다.

필드는 그 성격에 따라 _원본 필드, 파생 필드, 치환 필드_ 로 구분 합니다.

{% hint style="info" %}
필드를 취급 할 때는 다음 원칙을 반드시 준수 하시기 바랍니다.

1. 필드 값의 변경이 필요 할 경우 새로운 필드에 저장 하고 원문의 필드를 절대 변경 하지 않는다.
2. 하나의 필드는 반드시 하나의 의미만 지녀야 한다. 예를 들어 address라는 필드를 정의 하고 이 필드에 source ip, dest ip, agent ip등 서로 다른 의미를 갖는 값을 하나의 필드에 저장 하여서는 안됩니다. 이 경우 각각 의미에 맞는 필드를 새롭게 정의하여 저장 하는 것이 옳은 처리 방식 입니다. _**하나의 필드가 중의적 의미를 갖게 되면 후 처리 단계에서 공통의 로직을 적용 할 수 없고 중의적 의미를 해석하기 위한 특수 로직이나 처리가 필요하게 되어 정규화의 의미가 없어 집니다.**_
3. <mark style="color:red;">**Legacy field 구조 ( Secudium Iot)는 필수 필드를 제외 하고는 모두 재계 하십시오 특히customString, customNum, customLabel 등 중의적 의미를 담는 필드는 절대 사용 하여서는 안됩니다. 필요 할 경우 새로운 필드를 정의 하십시오**</mark>
{% endhint %}

### Field의 유형

* 원본 필드: 원본 이벤트에서 Tokenize된 데이터를 말하며 값이나 의미가 변경 되지 않은 원본 그대로의 값을 저장
* 파생 필드: 원본 필드를 기준으로 별도의 데이터 Set을 참조하여 생성된 새로운 값을 저장
* 치환 필드: 원본/파생 필드를 정해진 규칙에 의해 변경한 값을 저장
  * 치환 필드의 존재는 파생적인 문제를 발생 시킬 여지가 있습니다. 성격상 중복 되거나, 표현 방법에 대한 변경이 필요하여 치환을 하게 되는 경우라도 In-place update(Over write) 하지 않고 새로운 필드를 생성하여야 합니다.&#x20;

SUMiTS On-prem은 레거시 시스템 호환을 위해 Secudium IoT의 필드 체계를 그대로 유지 하고 있습니다. 따라서 기존 레거시 시스템의 약점을 그대로 이어가고 있어 개선이 필요한 부분 입니다. Field 체계를 재 설계 하기 위해서는 Web/Alarm 기능이 재 개발 되어야 합니다.

### Field의 종류

SUMiTS On-prem은 이벤트 처리 과정에서 필요한 필수 필드와 엔진에서 사용하는 필드를 사전 예약해둔 예약 필드와 자유롭게 확장 가능한 Event field로 구분 합니다.&#x20;

* Reserved field: 일부는 레거시 시스템의 호환을 위해 기존 Secudium IoT 필드 이름을 그대로 유지 하고 있습니다. Reserved 필드중 2xxxx대는 엔진에서 사용하는 System field 입니다.

<details>

<summary>Reserved fields</summary>

10001 eventId

10030 collectorReceiptTime

10023 rawEvent

10025 collectorAddress

10032 collectorId

20005 collectorPort

20007 norRuleId

20008 detectedTime

20009 syslogInfo

</details>

* Event field: Code table에 정의한 후 자유롭게 사용 할 수 있습니다.

### Code table

기존 레거시 시스템의 필드는 Name key 기반으로 구성 되어 있습니다. 따라서 Name이 변경되면 모든 데이터와 후 처리 기능들의 변경이 발생 하게 됩니다. id key 기반으로 변경이 필요 하나 기존 시스템에 미치는 영향을 최소화 하기 위한 방법으로 엔진 <-> 레거시 시스템 사이에 키 변환 과정을 수행 합니다.이 과정에 필요한 것이 Code table 입니다.&#x20;

엔진은 Code table을 이용하여 Name 기반의 Field key를 Id 기반의 Field key로 변경하고 모든 Field를 Id로 핸들링 합니다. 이후 엔진에서 처리된 정규화/탐지 이벤트는 저장 시점에 Name key 기반의 필드로 저장 하게 됩니다.&#x20;

{% hint style="info" %}
계속 언급하고 있는 내용이지만 레거시 시스템의 호환을 위해 필드는 기존 시스템과 동일 한 약점을 지고 있습니다.&#x20;
{% endhint %}

### Field type
