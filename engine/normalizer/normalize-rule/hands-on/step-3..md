# step 3. 식별 규칙 작성

### 식별 규칙

식별 규칙은 이 룰이 적용될 Payload의 패턴입니다.&#x20;

로그가 유입 되면 식별 규칙에 따라 패턴 매칭을 하고 매칭된 패턴에 연결된 정규화 룰을 선택 합니다.

선택된 정규화 룰은 우선순위에 따라 순차적으로 파싱을 시도 합니다. 파싱에 성공하면 처음 성공한 결과를 반환 하고 이후의 룰은 시도 하지 않습니다. 따라서 룰의 우선순위는 신중히 선택 하여야 합니다. (Early termination)

식별 규칙으로 입력한 key 패턴을 통해 정규화 룰을 선택하는 과정은 아래와 같습니다.

![](../../../../.gitbook/assets/select\_nor\_rule.png)



1. Payload에서 capture 정규식으로 텍스트를 선택 합니다.
2. 텍스트에서 \*"TCP"\* 패턴으로 정규화 룰 집합을 선택 합니다. \*"TCP"\* 패턴은 문장에 "TCP"가 포함 되어 있으면 이라는 의미의 패턴 입니다.
3. 텍스트에 적용 가능한 룰 10011, 20301, 33451이 선택 되었으며 이 룰 로 정규화를 수행 합니다.

### 식별 규칙 입력 값

* capture: 전체 Payload에서 구문 분석을 수행하기 위한 텍스트 영역을 정규식으로 지정 합니다.&#x20;

{% code overflow="wrap" %}
```
[ Example ]
capture: "(.*)"
SFIMS: Corr: TCP, SrcIP: 172.21.80.234, DstIP: 172.21.68.78, Message: "UDR_ATTACK_MYSQL", Classification: Login

capture: ".* Message: (.*)"
Message: "UDR_ATTACK_MYSQL", Classification: Login
```
{% endcode %}

* key: 정규화 룰을 선택하기 위한 패턴을 등록 합니다. \
  \
  \[ 패턴 문법 ]\
  "String"   : 완전 매칭\
  "String"\*  : String 으로 시작하는 문자열\
  \*"String"  : String 으로 끝나는 문자열\
  \*"String"\* : String을 포함하는 문자

```json
[ Rule example ]
{
  "id": 10011,
  "tag": "CISCO_FW",
  "capture": "(.*)",
  "key": "*\"SFIMS\"*"
}
```
