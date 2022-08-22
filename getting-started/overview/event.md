---
description: 이 장에서는 Core engine에서 처리하는 Event의 형식과 단계별 처리 과정을 설명하고 이벤트의 생명주기에 대해서 다룹니다.
layout: landing
---

# Event

## 수집 이벤트 \[ Collect event ]

Event의 기본 형식은 Time, Event ID, Sender info, Raw event로 구성 됩니다. 이벤트는 표준 수집 Collector를 통해 Kafka에 저장되거나 특별한 Agent를 통해 저장 할 수 있습니다. Event의 수집 Flow는 아래 Collector 페이지를 참고 하십시오.

* \<collecotr 링크 event flow 다이어그램 >

### 수집 이벤트 전송 형식

Device에서 발생한 로그를 수집하여 Kafka에 저장하는 로그를 _**Collect**_ _**event**_라고 하며 전송 포멧은 기본포멧 과  Sys-log 포멧을 지원 합니다.

![](../../.gitbook/assets/collect\_event.png)

#### 기본 포멧

![](../../.gitbook/assets/collect\_event\_default.png)

* **Time:** 13자리 숫자 Epoch mills로 _**로그가 수집된 시간**_을 표기한다. 이 시간은 이후 엔진에서 이벤트를 처리하는 기본 시간으로 사용된다. _로그의 발생 시간을 기준으로 이벤트 처리가 필요할 경우에는 로그의 원문 ( Orig device log )에 포함된 시간 정보를 정규화 하여 이용 하여야 합니다._
* **ID:** 128Bit의 UUID 형식이며 이벤트 개별 이벤트의 구분과 검색을 위해 사용 됩니다.
* **Sender:** 로그를 수집한 _**로그 수집기의 정보**_  Collect Agent의 정보이며 Device가 직접 전송 할 경우에는 해당 Device의 정보가 될 수 있다. 구성은 Sender의 IP, Port, Host name 또는 특별한 ID 등 별칭을 담을 수 있습니다.
* **Orig log:** Device가 생성한 실제 원본 로그이며 JSON Escape 규칙을 따르는 Text 입니다.&#x20;

{% hint style="info" %}
\[ Json Escape: [https://www.freeformatter.com/json-escape.html](https://www.freeformatter.com/json-escape.html) ]
{% endhint %}



&#x20;각 항목간의 구분은 ","로 구분 됩니다, Sender와 Orig device log는 별도의 포멧이 필요 합니다.

* **Sender Format:** 중괄호로 쌓여있는 Key, value 형식으로 Value는 "|" 구분자를 사용한다.\
  ****Format => **\[ "sender", "ip|port|host|" ]** \
  ****\
  ****<mark style="color:orange;background-color:yellow;">Ex) \["sender", "10.10.10.10|443|Local host|"]</mark>\ <mark style="color:orange;background-color:yellow;"></mark>
* **Orig log Format:** 중괄호로 쌓여있는 Key, value 형식으로 Value는 Json escape 규칙을 따릅니다.\
  Format => \["raw","Original text"]\
  \
  <mark style="color:orange;background-color:yellow;">Ex) \["raw", "SFIMS: Corr: TCP, SRC IP 10.20.203.40 DStIP:0.0.0.0"]</mark>\ <mark style="color:orange;background-color:yellow;"></mark>
*   **Default format Sample**\
    ****

    ```
    1657069151518,fbd92c06-b182-43a1-93d9-c15ed44ecfbb,[["sender", "10.10.10.10|4443|ubuntu"], ["raw", "SFIMS: Corr: TCP, SrcIP: 172.21.80.234, DstIP: 172.21.68.78, Message: "UDR_ATTACK_MYSQL", Classification: Login"]]
    ```



#### **Syslog format**

log를 수집하기 위해 별도의 Agent와 Collector를 사용 할 수 없는 환경을 위해 RFC3164, RFC5424의 Syslog 포멧을 지원 합니다.  sys-log의 기본 포멧은 RFC 공식 문서를 참고 하십시오.&#x20;

* Sys-log 로그 포멧의 경우 Sender IP = hostname, Port = 0, Host = hostname을 사용 합니다. Time의 경우 정규화 엔진에서 소비한 현재 시간을 Collect time 시간으로 사용 합니다.



\<Colector 설정 링크>



## 정규화 이벤트

이기종 장비로 부터 발생하는 다양한 데이터 형식을 일관된 형식을 표준화 하는 작업을 정규화 라고 합니다. 정규화 는 로그를 파싱하고 파싱된 각 항목을 의미있는 Fields 로 생성 합니다.  정규화 이벤트는 Code table에 명시된 Field의 Name과 Type에 따라 Json 형태로 생성 됩니다. Code table과 Field의 설명은 정규화 엔진을 참고 하십시오&#x20;

<정규화 엔진 링크>

### 로그 파서와 지원 형식

* Json, Regex, Key Value pair, Single separator를 기본으로 제공 합니다 로그 파싱 방법은 정규화 엔진을 참고 하십시오.

## 탐지 이벤트

탐지 이벤트는 정규화 이벤트와 탐지 룰에 기입된 추가 정보와 함께 Json 형태로 저장 됩니다.

{% hint style="warning" %}
_**<탐지이벤트 처리의  문제점>**_

Legacy 시스템인 Secudium-IoT의 Field와 이벤트 처리 구조를 유지 하기 때문에 아래와 같은 문제점이 발생하며 향후 버전에서는 개선이 필요함. \[ Alarm기능과 UI의 변경이 필요 ]

1. Field의 값을 변경 하거나 중복 데이터를 생성하는 이벤트 처리에서 원칙적으로 하지 말아야 할 행위를 함
2. Multiple Event를 핸들링 할 수 있는 데이터구조가 없음 때문에 임계탐지나 상관 탐지의 경우 근거 이벤트를 담을 수 있는 방법이 없어 버려지게 됩니다.
{% endhint %}

* Live detect: 정규화 이벤트 결과에 8개의 필드가 추가 되고 2개의 필드 값이 변경 됩니다.

<details>

<summary>Fields</summary>

**\[ Added Fields ]**

* destinationCountryCode
* detectedTime
* eventRiskLevel
* eventType
* risklevelscoreOverrideBit
* ruleId
* ruleName
* sourceCountryCode

**\[ Replace Fields]**

* eventCategory
* eventRiskScore

</details>

* Threshold event: Live detect 결과에서 5개의 필드 값을 변경 합니다.

<details>

<summary>Fields</summary>

**\[ Replace Fields]**

* eventCount
* eventType
* ruleId        &#x20;
* ruleName

</details>

* Correlated event: 정규화 이벤트 결과에 6개의 필드를 추가 합니다.

<details>

<summary>Fields</summary>

**\[ Added Fields]**

* destinationCountryCode
* detectedTime
* eventType
* ruleId
* ruleName
* sourceCountryCode

</details>
