---
description: 정규화 룰은 수집한 이벤트를 파싱하고 구문분석하여 의미 있는 필드를 생성하는 규칙 입니다.
---

# Normalize Rule

### 정규화 룰 구조

* id: \[Numeric] => Unique id
* tag: String => Device type OR Category를 구분하기 위한 Tag성 필드
* capture: String => Payload 메세지 중 구문 분석을 하고자 하는 영역을 선택 하기 위한 정규식
* key: String => payload 유형을 판별하기 위한 시그니처 Pattern, 이 패턴으로 적용 할 정규화 룰을 선택 합니다.
* tokenizer: payload 구문 분석을 위한 Tokenizer를 선택 합니다.
  * json => json message format의 payload를 파싱 합니다.
  * regex => 정규식 기반으로 payload를 파싱 합니다.
  * kv => key:value message format의 payload를 파싱 합니다.
  * sep => 특정 문자로 구분된 message format의 payload를 파싱 합니다.
* tokenizerRule: 선택한 토큰나이저에 맞는 룰을 작성 합니다.
* assign: 토큰나이즈된 값을 저장할 필드를 지정 합니다.
* ref: 특수한 테이블 데이터의 참조가 필요 한 경우 참조 규칙을 작성 한다. ex: assets, event category&#x20;

```json
{
  "id": String,
  "tag": String,
  "capture": String,
  "key": String,
  "tokenizer": String,
  "tokenizeRule": [ String ],
  "assign": [[String ]],
  "ref": [
    {
      "seq": Numeric,
      "tableName": String,
      "lookup":  String]
    }
  ]
}
```
