# KV Tokenizer

### Key:value pair 형식의 로그 파싱을 위한 토큰 나이저&#x20;

규칙적인 Key:Value 구조를 갖춘 로그를 파싱하기 위한 토큰 나이저 입니다.&#x20;

### \[ Rule Syntax ]

파싱을 위해서는 Key:Value 구분자, Item 구분자 두개의 구분자가 필요 합니다.\
expression: \[item delimiter]::\[key value delimiter]

"tokenizer":"kv"\
"tokenizeRule":\[ "\[item delimiter]::\[key value delimiter]" ]



기본 표현 방법은 아래 예제를 참고 하십시오.

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
  "tokenizeRule": [ 
     "[|]::[=]"
  ]
}
```
