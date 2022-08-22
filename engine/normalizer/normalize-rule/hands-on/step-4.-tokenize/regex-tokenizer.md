# Regex Tokenizer

### Unformatted text 를 파싱 하기 위한 정규식 기반의 토큰나이저

regex 토큰나이저는 정규식을 기반으로 동작 합니다. capture한 텍스트와 정규식이 완전 매칭 될 경우 파싱에 성공합니다.  정규식 기반의 토큰나이저는 캡처링 순서에 의존 적이며, 캡처 순서를 필드의 Key로 사용 합니다.

{% hint style="danger" %}
Regex 토큰나이저는 정규식 구문이 전체 성능에 큰 영향을 미칩니다. 정규식 작성시 아래 가이드를 준수 하십시오.

1. 백트레이스를 최소화 시키십시오 과도한 백트레이스는 성능에 치명적인 영향을 미칩니다.
2. 로그 유형이 다양할 경우 정규식은 Fail-fast 할 수 있게 작성 하십시오. 과도한 백트레이스를 발생 시키는 1개의 정규식 보다 Fail-fast한 여래개의 정규식으로 나누는 것이 더 유리 합니다.
3. 가급적이면 ^ \~ $ 를 이용하여 완전 매칭 구문을 작성 하십시오.
4. 필요한 값은 캡처 그룹으로 지정 하십시오
{% endhint %}

{% hint style="info" %}
정규식 작성 후 아래 URL을 참고 하여 검증 하십시오.

[https://regex101.com/](https://regex101.com/)
{% endhint %}

### \[ Rule syntax ]&#x20;

* PCRE2: [Perl Compatible Regular Expressions](https://www.pcre.org/)

"tokenizer":"reg"

"tokenizerRule":\[ "Regex"]



기본 표현 방법은 아래 예제를 참고 하십시오.

{% code overflow="wrap" %}
```
[ Example log ]
SFIMS: Corr: TCP, SrcIP: 172.21.80.234, DstIP: 172.21.68.78, Message: "UDR_ATTACK_MYSQL", Classification: Login

Regex: ^SFIMS: Corr: ([^,]*), SrcIP: ([^,]*), DstIP: ([^,]*), Message: "([^,]*)".*

> Group 1 : TCP
> Group 2 : 172.21.80.234
> Group 3 : 172.21.68.78
> Group 4 : UDR_ATTACK_MYSQL
```
{% endcode %}

{% hint style="warning" %}
tokenizeRule 입력 시: Escaped json 문자열로 입력 하십시오. 아래 URL을 참고 하십시오.

[https://www.freeformatter.com/json-escape.html](https://www.freeformatter.com/json-escape.html)
{% endhint %}

```
[ Rule example ]
{
  "id": 10011,
  "tag": "example",
  "capture": "(.*)",
  "key": "*\"SFIMS\"*",
  "tokenizer": "reg",
  "tokenizeRule": [
    "(?:SymantecServer:)[^,]*,IP 주소:\\s([^,]*),(?:[^,]*,)+?위험 요소 이름:\\s([^,]*),(?:발생:\\s[^,]*),([^,]*),(?:[^,]*,)*?이벤트 시간:\\s([^,]*),(?:[^,]*,)"
  ]
}
```

####
