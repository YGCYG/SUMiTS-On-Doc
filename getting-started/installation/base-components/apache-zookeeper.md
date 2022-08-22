# Apache Zookeeper

## Apache Zookeeper

SUMiTS-Onprem V 1.0은 _**Apache Zookeeper 3.7.1**_ 버전을 필요로 합니다. Zookeeper에 대한 자세한 설명은 공식 문서를 참고 하시기 바랍니다.&#x20;

{% embed url="https://zookeeper.apache.org/releases.html" %}

#### 1. Download \[ Apache ZooKeeper 3.7.1 (latest stable release) ]

{% embed url="https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.7.1/apache-zookeeper-3.7.1-bin.tar.gz" %}

#### 2. 경로 선택 및 압축 해제

설치 하고자 하는 위치로 다운로드 받은 파일을 복사하여 압축을 해제 합니다.

{% hint style="info" %}
권장 설치경로: /opt/sumits/apache-zookeeper-3.7.1-bin
{% endhint %}

```
$ mkdir /opt/sumits
$ cp apache-zookeeper-3.7.1-bin.tar.gz /opt/sumits
$ tar -xvf apache-zookeeper-3.7.1-bin.tar.gz
```

#### 3. Configuration

* **zoo.cfg 파일생성:** zoo.cfg파일을 생성하고 아래 옵션을 설정 합니다. 자세한 옵션 설명은 공식 문서를 참고 하십시오

<details>

<summary>Options</summary>

* tickTime=2000&#x20;
* initLimit=10&#x20;
* syncLimit=5&#x20;
* dataDir= \[ data 디렉토리 경로 ]
* clientPort= \[ port ]
* server.1=\[ host name or ip ]:2888:3888&#x20;

</details>

{% hint style="info" %}
권장 경로:/opt/sumits/apache-zookeeper-3.7.1-bin/conf/zoo.cfg
{% endhint %}

<details>

<summary>Example: zoo.cfg</summary>

```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data
clientPort=2181
server.1=10.10.10.10:2888:3888
```

</details>

#### 4. Data 디렉토리 생성 및 myid 부여하기

* **데이터 디렉토리 생성:** Znode 스냅샷과 트랜잭션 로그를 저장할 디렉토리를 생성 합니다.

{% hint style="info" %}
권장 데이터 경로:/opt/sumits/zkdata
{% endhint %}

```
$ mkdir /opt/sumits/data
```

* **myid 생성:** 주키퍼 노드를 구분하기 위한 myid를 생성 하여야 합니다. myid 생성 방법은 data 디렉터리 아래 "myid" 파일을 생성하여 다른 클러스터 와 중복되지 않는 정수를 입력 합니다.

<details>

<summary>Example: myid</summary>

```
$ echo 1 > /opt/sumits/data/myid
```

</details>

#### 4. 앙상블 구성하기

Zookeeper 앙상블은 쿼럼 구조로 앙상블에 참여하는 노드는 반드시 홀 수개를 유지 하여야 합니다. 따라서 앙상블을 구성하기 위해서는 홀 수의 Machine이 필요 합니다. 아래 절차에 따라 앙상블을 구성 할 수 있습니다.

* 앙상블에 참여 하는 모든 Machine에 설치 과정 1 \~ 3 까지의 작업을 동일하게 수행해 줍니다.
* 각 노드별로 과정 4번의 myid가 중복되지 않도록 순차적인 정수 id를 부여 합니다.
* 각 노드별로 과정 3번의 zoo.cfg에 앙상블에 참여하는 노드 정보를 입력 합니다.\
  노드정보 입력 형식은 다음과 같습니다.\
  \
  \- "server.\[_myid_]=\[_ip_]:2888:3888"

<details>

<summary>Example: zoo.cfg</summary>

```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data
clientPort=2181
server.1=10.10.10.10:2888:3888
server.2=10.10.10.11:2888:3888
server.3=10.10.10.12:2888:3888
```

</details>

#### 5. 실행

앙상블에 참여하는 모든 노드에서 다음 명령어를 실행 합니다 zookeeper 노드는 Foreground로 동작 하기 때문에 터미널을 종료하면 주키퍼 서비스도 종료 됩니다. 주키퍼 서비스를 데몬모드로 동작 하기 위해서는 백그라운드로 실행 시키거나 systemd에 등록하여 사용하길 권장 합니다.

```
$ /opt/sumits/apache-zookeeper-3.7.1-bin/bin/zkServer.sh start
```
