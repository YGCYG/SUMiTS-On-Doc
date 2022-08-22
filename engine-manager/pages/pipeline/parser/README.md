---
description: >-
  Normarlize Rule의 configuration을 작성하는 Node로, Tokenizer별로 Tokenize Rule을 지정하여 샘플
  로그를 Tokenizing하여 Assign Table을 생성
---

# Parser

### Log parsing sequence

1. Tokenizer 선택
2. Sample log sub-sampling (capture)
3. Tokenize Rule 등록
4. Assign table set

### Capture

Capturing Regex를 등록하여 샘플 로그 중 Tokenizing할 sub-sample을 추출하며, JSON tokenizer의 경우 pretty JSON으로 captured Log가 표현되고, 나머지 Regex / Separator / Key-Value tokenizer의 경우 capture한 sub-sample이 노란색으로 highlight 된다.

### Tokenizer

* JSON
* Regex
* Separator
* Key-Value

### ~~Kind~~ (-> Tag변경)

~~Normalize rule 배포 시 private key로 사용~~

### Key

Normalize rule **group**의 **group key**로 사용

### Assign

Log parsing sequence를 수행 후, tokenize 된 값을 key-value 쌍으로 table에 표시하고 assign 값을 사용자가 입력하여 normalize rule 의 assign value를 생성
