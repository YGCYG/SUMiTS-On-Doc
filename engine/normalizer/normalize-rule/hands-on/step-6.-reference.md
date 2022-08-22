# step 6. Reference

Reference는 원본 값을 기준으로 특정 데이터 테이블을 조회 또는 참조 하여 값을 생성 하는 기능 입니다. Reference 데이터와 인덱스로 구성 됩니다. 데이터는 일반적인 Table 형태로 저장 되며 각 레코드는 Unique key를 가지고 있어야 합니다. 인덱스는 빠른 데이터 조회를 위해 역색인(Inverted index)구조 로 구성 되어 있습니다.

{% hint style="danger" %}
Reference는 검색 기반으로 동작 합니다. 따라서 검색 조건에 부합 하는 데이터가 다수(중복) 일 경우 우선 검색 결과 1건에 대해서만 반환 합니다.
{% endhint %}

