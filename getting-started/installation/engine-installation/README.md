# Engine Installation

## Before Installation

SUMiTS On-prem 엔진이 동작 하기 위해서는 아래 Components를 필요로 합니다. Base components의 구성은 링크를 참조 하십시오. \[ [Base components](../base-components/) ]

* JAVA 11
* Apache Spark
* Apache Zookeeper
* Apache Kafka

### 경로 선택 및 압축 해제

[Update history](../../overview/update-history/) 를 참고 하여 설치하고자 하는 버전의 바이너리를 다운로드 하고 압축을 해제 합니다. 바이너리의  디렉토리 구성은 다음과 같습니다.

* bin: engine 바이너리  실행 스크립트
* conf: 로깅 및 추가 설정
* logs: Application 로그

{% hint style="info" %}
권장 설치경로: /opt/sumits/engine
{% endhint %}

```
$ mkdir /opt/sumits
$ cp sumits-on-prem-engine-1.0.tgz /opt/sumits
$ tar -xvf sumits-on-prem-engine-1.0.tgz
```

### Zookeeper Node 생성

bin 디렉토리에 포함된 create\_znode.sh을 열어 config를 수정 합니다.

```
$ vim /opt/sumits/bin/create_znode.sh
$ sh /opt/sumits/bin/create_znode.sh
```

<details>

<summary>Example</summary>

```
#######config####################################
ZK_CON=10.10.10.1:2181
ZK_BIN=/opt/sumits/apache-zookeeper-3.7.1-bin
#################################################
```

</details>

## Engine

{% content-ref url="normalizer.md" %}
[normalizer.md](normalizer.md)
{% endcontent-ref %}

{% content-ref url="live-detector.md" %}
[live-detector.md](live-detector.md)
{% endcontent-ref %}

{% content-ref url="threshold-analyzer-graph.md" %}
[threshold-analyzer-graph.md](threshold-analyzer-graph.md)
{% endcontent-ref %}

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
