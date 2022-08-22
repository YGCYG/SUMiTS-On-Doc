# Json Tokenizer

### Json 형식 로그를 파싱 하기 위한 토큰나이저

json 토큰나이저 룰은 XPath와 유사한 문법을 사용하고 있습니다.

{% hint style="info" %}
Json 토큰나이저는 다음과 같은 특성을 가지고 있습니다.

* 반환 결과가 배열 일 경우 csv 형태의 문자열을 반환 합니다.&#x20;
* null 값을 가진 key는 반환 하지 않습니다.
{% endhint %}

### \[ Rule Syntax ]

디렉터리 패스 구조로 node를 탐색 할 수 있습니다: expression: \[ root ] / \[ child ] / \[ child ] \~

배열 값은 중 괄호와 배열 인덱스를 사용 하여 탐색 할 수 있습니다: expression: \[ root ] / \[ child(index) ]

"tokenizer": "json"

"tokenizerRule": \[ "path1", "path2", "path3" ]



기본 표현 방법은 아래 예제를 참고 하십시오.

```
[ Example json log ]
{
  "person": {
    "id": 1,
    "firstName": "Tom",
    "lastName": "Cruise",
    "hobby": ["ski", "fishing" ]
  }
}

> person/id => "1"
> person/hobby(1) => "fishing"
> person/hobby => "ski,fishing"
```

```json
[ Rule example ]
{
  "id": 10011,
  "tag": "example",
  "capture": "(.*)",
  "key": "*\"person\"*",
  "tokenizer": "json",
  "tokenizeRule": [
    "person/id",
    "person/hobby(1)"
  ]
}
```
