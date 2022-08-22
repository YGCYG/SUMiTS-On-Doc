# Apache Kafka

## Apache Kafka

SUMiTS-Onprem V 1.0은 kafka\_2.12-3.2.0 버전을 필요로 합니다. kafka에 대한 자세한 설명은 공식 문서를 참고 하시기 바랍니다.

{% embed url="https://kafka.apache.org/" %}

#### 1. Download \[ kafka\_2.12-3.2.0 Binary]

{% embed url="https://kafka.apache.org/downloads" %}

#### 2. 경로 선택 및 압축 해제

설치 하고자 하는 위치로 다운로드 받은 파일을 복사하여 압축을 해제 합니다.

{% hint style="info" %}
권장 설치경로: /opt/sumits/kafka\_2.12-3.2.0
{% endhint %}

```
$ mkdir /opt/sumits
$ cp kafka_2.12-3.2.0.tgz /opt/sumits
$ tar -xvf kafka_2.12-3.2.0.tgz
```

#### 3. Data 저장 디렉토리 생성

카프카는 데이터를 File system에 저장 합니다. 따라서 데이터를 저장할 별도의 디렉토리를 생성 합니다.

{% hint style="info" %}
Data 디렉토리는 System 운영에 영향을 주지않도록 물리적으로 분리된 DISK 공간을 사용 하길 권장 합니다.
{% endhint %}

<details>

<summary>Example</summary>

```
//예제의 경로는 권장 하지 않습니다.
$ mkdir /opt/sumits/kf_data
```

</details>

#### 4. Configuration

카프카 환경설정은 카프카설치 디렉토리 아래 config/server.properties에서 설정 합니다. 자세한 옵션 설명은 공식 문서를 참고 하십시오

<details>

<summary>Options</summary>

* broker.id
* log.dirs
* zookeeper.connect

</details>

<details>

<summary>Example</summary>

```
$ vi /opt/sumits/kafka_2.12-3.2.0/config/server.properties

broker.id=1
log.dirs=/opt/sumits/kf_data //예제의 경로는 권장 하지 않습니다.
zookeeper.connect=10.10.10.1:2181,10.10.10.11:2181,10.10.10.12:2181
```

</details>

#### 5. Kafka 클러스터 구성

카프카 클러스터를 구성하기 위해서는 물리적으로 분리된 Machine이 필요 합니다. 아래 절차에 따라 클러스터를 구성 할 수 있습니다.

* 클러스터에 참여 하는 모든 Machine에 설치 과정 1 \~ 4까지의 작업을 동일하게 수행해 줍니다.
* 각 노드별 Configuration의 broker.id를 중복 되지 않게 설정 합니다. \[ 과정 4 ]

#### 6. 실행

클러스터에 참여하는 모든 Machine에서 다음 명령을 실행 합니다. 이 예제에서는 백그라운드 실행을 위해 -daemon 옵션을 추가 하였습니다.

```
$ /opt/sumits/kafka_2.12-3.2.0/bin/kafka-server-start.sh -daemon /opt/sumits/kafka_2.12-3.2.0/config/server.properties
```
