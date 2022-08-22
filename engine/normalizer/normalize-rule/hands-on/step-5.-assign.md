# step 5. Assign

Assign rule은 Tokenize한 결과가 저장될 정규화 필드를 지정 합니다. 정규화 룰의 성공/실패 여부는 Assign 규칙에 의해 결정 됩니다. Tokenize한 결과가 모두 Assign 되어야 정규화 성공으로 간주 됩니다.

Assign 규칙은 3개의 필드로 구성 되어 있습니다.

* form: 원문 메세지에서 파싱한 필드 key 입니다. Key가 있는 Json/kv 토큰나이저는 원문의 Key를 사용 하지만  필드 Key가 없는 Regex, Sep 토큰나이저는 시퀀스한 인덱스 숫자를 키로 사용 합니다.
* to: 원문 메세지에서 파싱한 필드의 값이 저장될 정규화 필드 key 입니다.
* default(optional): json/kv 토큰나이저의 경우 Tokenize한 필드 값이 없을 경우 기본 값을 지정 할 수 있습니다. default 값을 설정에 따라 _**필수 필드와 옵션 필드를 구분 할 수 있습니다. (**** **<mark style="color:red;">**regex / sep 는 default 값이 적용 되지 않습니다.**</mark>** ****)**_

{% hint style="info" %}
default 값을 지정 하지 않을 경우 해당 필드는 반드시 존재 하여야 하는 필수 필드가 됩니다. 비슷한 유형의 로그에 잘 못된 정규화 룰이 적용 되는 것을 방지 하기 위해  필수 필드 이용하여 엄격한 정규화 룰을 만들 수 있습니다.
{% endhint %}

### Assign 규칙에 따른 성공/실패 판단 기준

Regex 기반의 정규화 룰은 작성한 정규식의 매칭 여부와 캡처한 값의 갯수와 Assign한 값의 갯수로 결정 됩니다. 즉 정규식에서 캡처한 결과가 N개 이면 Assign되는 값의 숫자도 동일 하여야 합니다. 마찬 가지로 Sep 기반의 정규화 룰 또한 파싱한 결과의 갯수와 Assign한 결과의 갯수가 동일 하여야 합니다.

json / kv와 같이 key:value 구조의 토큰나이저의 경우 필드는 존재 하지만 값이 null인 경우 이를 성공으로 판별 할지 실패로 판별 할지에 대한 스팩이 정의 되지 않아 모호한 부분이 있습니다. 때문에 default 값을 지정하여 값이 null 일 경우 기본값을 반환하여 정규화 룰이 성공 하도록 할 수 있습니다.

### Side effect

null 처리를 위한 default 지정 이외에 원문에 해당 키가 존재 하지 않아도 파싱을 성공 하게 할 수 있는 부수 효과를 발생 시킵니다.

{% hint style="info" %}
default 값의 원래 의도는 Regex 파서와 달리 성공 실패 여부를 판별 할 수 있는 기준이 없어 파싱 결과를 검증하기 위한 confirm 규칙 용도로 개발 되었으나 룰의 통일 성을 위해 default 규칙으로 변경 되었습니다. _**( 현재 default 값의 동작이 토큰나이저 별로 상이 하지만 추후 동일한 형상으로 동작 할 수 있도록 변경이 필요 합니다. )**_
{% endhint %}

### \[ Rule syntax ]&#x20;

"assign": \[ \["from","to","default"],\["from","to","default"],\["from","to","default"] ]



기본 표현 방법은 아래 예제를 참고 하십시오.

#### Regex example

{% code overflow="wrap" %}
```
[ Example Regex log ]
SFIMS: Corr: TCP, SrcIP: 172.21.80.234 DstIP: 172.21.68.78, Message: "UDR_ATTACK_MYSQL", Classification: Login

Regex: ^SFIMS: Corr: ([^,]*), SrcIP: ([^,]*), DstIP: ([^,]*), Message: "([^,]*)".*

> Group 1 : TCP
> Group 2 : 172.21.80.234
> Group 3 : 172.21.68.78
> Group 4 : UDR_ATTACK_MYSQL
```
{% endcode %}

```json
[ Rule example ]
{
  "id": 10011,
  "tag": "example",
  "capture": "(.*)",
  "key": "*\"SFIMS\"*",
  "tokenizer": "reg",
  "tokenizeRule": [
    "(?:SymantecServer:)[^,]*,IP 주소:\\s([^,]*),(?:[^,]*,)+?위험 요소 이름:\\s([^,]*),(?:발생:\\s[^,]*),([^,]*),(?:[^,]*,)*?이벤트 시간:\\s([^,]*),(?:[^,]*,)"
  ],
"assign": [
    ["1","protocol",""],
    ["2","sourceIp",""],
    ["3","destIp",""],
    ["4","signature",""]
  ]
}
```

#### Json example

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
  ],
  "assign": [
    [ "person/id","userId",""],
    [ "person/hobby(1)","firstHobby","None"],
    [ "person/hobby","Interest","None"]
  ]
}
```

#### kv example

```
[ Example log ]
date=2020-01-01|id=10021|code=A0032

* item delimiter = [|]
* key:value delimiter = [=]

Rule: [|]::[=]

> date => 2020-01-01
> id => 10021
> code => A0032

```

```json
[ Rule example ]
{
  "id": 10011,
  "tag": "example",
  "capture": "(.*)",
  "key": "\"date\"*",
  "tokenizer": "kv",
  "assign": [
    [ "date","logtime","None"],
    [ "id","userId",""],
    [ "code","signature",""]
  ]
}
```

#### sep example

```
[ Example log ]
2020-01-01,10021,A0032

* item delimiter = [,]
Rule: [,]

> 1 => 2020-01-01
> 2 => 10021
> 3 => A0032

```

```json
[ Rule example ]
{
  "id": 10011,
  "tag": "example",
  "capture": "(.*)",
  "key": "*\"2020\"*",
  "tokenizer": "sep",
  "tokenizeRule": [ 
     "[|]"
  ],
  "assign": [
    [ "1","logtime",""],
    [ "2","userId",""],
    [ "3","code",""]
  ]
}
```
