# Sep Tokenizer

### 고정된 구분자를 가진 형식의 로그 파싱을 위한 토큰 나이저&#x20;

CSV, TSV와 같이 특정한 문자로 구분되어 있는 로그를 파싱 하기 위한 토큰나이저 입니다. 토큰나이즈 한 값의 key는 Sequence한 int 인덱스로 반환 합니다.

### \[ Rule Syntax ]

파싱을 위해서는 item을 구분하기 위한 delimiter를 필요로 합니다.\
"tokenizer":"sep"\
"tokenizeRule":\[ "\[item delimiter]" ]\


기본 표현 방법은 아래 예제를 참고 하십시오.

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
  ]
}
```
