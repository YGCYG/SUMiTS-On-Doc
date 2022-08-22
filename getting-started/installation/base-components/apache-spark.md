# Apache Spark

## Apache Spark

#### 1. Download

SUMiTS-Onprem V 1.0은 _**Scala 2.12로 빌드된 Spark 3.3.0**_ 버전을 필요로 합니다. 이 문서에서는 Stand alone 모드로 동작하기 위한 설치와 환경을 설명 합니다. Cluster mode 동작을 위한 설정 공식 문서를 참고 하십시오.

{% embed url="https://spark.apache.org/downloads.html" %}

#### 2. 경로 선택 및 압축 해제

설치 하고자 하는 위치로 다운로드 받은 파일을 복사하여 압축을 해제 합니다.

{% hint style="info" %}
권장 설치경로: /opt/sumits/spark-3.3.0-bin-hadoop2
{% endhint %}

```
$ mkdir /opt/sumits
$ cp spark-3.3.0-bin-hadoop3.tgz /opt/sumits
$ tar -xvf spark-3.3.0-bin-hadoop3.tgz
```

#### 3. Configuration

spark-env.sh 파일을 편집하여 Spark 환경 설정을 합니다. 자세한 옵션 설정은 공식 문서와 Example을 참고 하십시오.

* SPARK\_DAEMON\_MEMORY=\[할당 가능한 메모리]
* SPARK\_WORKER\_MEMORY=\[할당 가능한 메모리]
* SPARK\_MASTER\_IP= \[ Master 노드로 사용 될 Server의 호스트명 또는 IP ]
* JAVA\_HOME=\[ JAVA 설치 경로 ]
* SPARK\_WORKER\_OPTS="-Dspark.worker.cleanup.enabled=true"

```
$ cd /opt/sumits/spark-3.3.0-bin-hadoop2/conf
$ cp spark-env.sh.template spark-env.sh
# vi spark-env.sh
```

<details>

<summary>Example: spark-env.sh </summary>

```
export SPARK_DAEMON_MEMORY=1g
export SPARK_WORKER_MEMORY=8g
export SPARK_MASTER_IP=localhost
export JAVA_HOME=/usr/lib/jvm/jdk-11.0.3

SPARK_WORKER_OPTS="-Dspark.worker.cleanup.enabled=true"
```

</details>

#### 4. Cluster 구성

Spark cluster에 참여할 모든 Server에 Spark 바이너리를 다운받아 해제 합니다. [\[ 과정 2번 참고 \]](apache-spark.md#2.)&#x20;

Work node는 별도의 설정이 필요하지 않습니다. \[ [과정 3번은 생략](apache-spark.md#3.-configuration) ]&#x20;

#### 5. 이중화 구성 \[ Optional ]

Master 노드 이중화 구성을 위해서는 Zookeeper가 필요 합니다. Master 노드에 참여할 Server에 \[ 과정 2 ]를 참고하여 Spark 설치 후 \[ 과정 3 ]을 참고하여 아래 옵션을 추가 합니다. 모든 Master 노드에 동일한 설정을 하여야 합니다.

* spark-env.sh에 아래 옵션을 추가하고 \[Zookeeper Connections] 정보를 설정 합니다. 자세한 설정은 Example과 공식 문서를 참고 하십시오.

> SPARK\_DAEMON\_JAVA\_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=<mark style="color:blue;">**\[Zookeeper Connections]**</mark> -Dspark.deploy.zookeeper.dir=/spark"

<details>

<summary>Example: spark-env.sh </summary>

```
export SPARK_DAEMON_MEMORY=1g
export SPARK_WORKER_MEMORY=8g
export SPARK_MASTER_IP=localhost
export JAVA_HOME=/usr/lib/jvm/jdk-11.0.3

SPARK_WORKER_OPTS="-Dspark.worker.cleanup.enabled=true"
SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=server1:2181,server2:2181,server3:2181 -Dspark.deploy.zookeeper.dir=/spark"
```

</details>

#### 6. 실행

* Master 노드 실행: Master 노드에 참여하는 Machine의 Spark 설치 경로로 이동하여 sbin/start-master.sh를 실행 합니다.

<details>

<summary>Example: start-master.sh</summary>

```
$ /opt/sumits/spark-3.3.0-bin-hadoop2/sbin/start-master.sh
```

</details>

*   Slave 노드 실행: Slave 노드에 참여할 Machine의 Spark 설치 경로로 이동하여 sbin/start-slave.sh를 실행 합니다. start-slave.sh는 실행시 Master node의 연결 정보를 필요로 합니다.



    * start-slave.sh spark://\[ master connection info ]

<details>

<summary>Example: start-slave.sh</summary>

```
// spark://[ master node의 host_name:port OR Ip:port ]
$ /opt/sumits/spark-3.3.0-bin-hadoop2/sbin/start-slave.sh spark://MyServer:7077
```

</details>

{% hint style="info" %}
동일한 Machine이 Master / Slave 노드에 모두 참여 하고 싶다면 start-master.sh, start-slave.sh 스크립트를 실행 합니다.
{% endhint %}
