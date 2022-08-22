---
description: Threshold Analyzer는 임계치 기반의 탐지와 Graph기반의 상관 탐지를 수행 합니다.
---

# Threshold Analyzer( graph )

Threshold 탐지 엔진은 실행을 위해 반드시 필요한 설정인 Basic Configuration과 동작 제어를 위한 부가적인 Control Option으로 구분 됩니다. 실행 스크립트는 start-threshold.sh 입니다.&#x20;

### 1. Basic Configuration (필수 설정)

Basic Configuration은 Application마다 고유한 설정 값이 필요한 App Config와 공통의 설정 값인 Common Config로 구분합니다.

* 이옵션은 실행되는 엔진마다 설정 차이가 있습니다.

<details>

<summary>App Config</summary>

* APP\_ROOT: 엔진 설치 디렉토리
* APP\_PATH: 엔진 바이너리 패스
* IN\_TOPIC:  정규화 데이터 토픽
* FILTERED\_TOPIC: 임계탐지, 상관탐지 용 Source 데이터 ( In data )
* ANALYZED\_TOPIC: 탐지결과 저장 토픽 ( Out data)

\[ 아래 옵션은 [Spark submitting 문서](https://spark.apache.org/docs/3.3.0/submitting-applications.html#content)를 참고 하십시오 ]

* MODULE\_NAME: 어플리케이션 이름
* TOTAL\_EXECUTOR\_CORES: Spark Worker노드에 할당 할 코어 갯수 \[ Recommend: 1 ]
* EXECUTOR\_MEMORY: Spark Worker 노드에 할당 할 메모리 \[ Recommend: 4g ]
* DRIVER\_MEMORY: Spark driver에 할당 할 메모리 \[ Recommend: 4g ]
* SP\_DURATION: 엔진 배치 타임 (ms) \[ Recommend: 8000 ]

</details>

* 이 옵션은 모든 엔진에 공통으로 사용 되는 옵션 입니다.

<details>

<summary>Common Config</summary>

* SPARK\_BIN: Spark bin 패스
* SPARK\_MASTER: Spark Master connection
* KAFKA\_BROKERS: Kafka brokers connections
* ZK\_CONN: zookeeper cluster connections

</details>

<details>

<summary>Example: start-thresold.sh</summary>

```
##########CONFIG#####################################################
MODULE_NAME=Threshold                                                                                
                                                                                                         
TOTAL_EXECUTOR_CORES=1                                                                                   
EXECUTOR_MEMORY=1g                                                                                       
DRIVER_MEMORY=1g                                                                                         
SP_DURATION=8000                                                                                         
                                                                                                         
APP_ROOT=/opt/secudium/ds-spark/                                                                         
MAIN_CLASS=com.skinfosec.dsp.adenium.dsp.iotThresholdAnalyzer.IotThresholdAnalyzer
APP_PATH=/home/beomsu/Codes/DS-Adenium/build/libs/DS-Adenium-1.0-SNAPSHOT-all.jar                        
                                                                                                         
FILTERED_TOPIC=filtered
ANALYZED_TOPIC=analyzed-event                                                                            

######################## SPARK Config #######################################                            
SPARK_BIN=/opt/secudium/spark-3.3.0/bin
SPARK_MASTER=spark://HP:7077
#############################################################################                            

######################## Kafka Config #######################################                            
IN_KAFKA_BROKERS=10.250.238.73:9092
OUT_KAFKA_BROKERS=10.250.238.37:9094                                                                     
#############################################################################                            

######################## Zookeeper Config ###################################                            
ZK_CONN=10.250.238.167:2181
#############################################################################                            

######################## Do not edit ########################################                            
#############################################################################                            
$SPARK_BIN/spark-submit --master $SPARK_MASTER --deploy-mode client --supervise --class $MAIN_CLASS \    
--driver-java-options "-Dlog4j.configuration=file:$APP_ROOT/conf/log4j.xml -Ddm.logging.name=$MODULE_NAME -Ddm.logging.path=$APP_ROOT/logs" \
--conf "spark.streaming.blockInterval=100ms" \ 
--conf "spark.locality.wait=100ms" \
--conf "spark.ui.port=4041" \
--conf "spark.executor.logs.rolling.strategy=size" \                                                     
--conf "spark.executor.logs.rolling.maxSize=100000" \                                                    
--conf "spark.executor.logs.rolling.maxRetainedFiles=5" \                                                
--conf "spark.executor.heartbeatInterval=2000" \
--total-executor-cores $TOTAL_EXECUTOR_CORES --executor-memory $EXECUTOR_MEMORY --driver-memory $DRIVER_MEMORY --name $MODULE_NAME $APP_PATH 0 1000 0 "" \                                                         
-sp:master $SPARK_MASTER \
-sp:app $MODULE_NAME \
-zk:conn $ZK_CONN \
-kf:broker $KAFKA_BROKERS \
-kf:topic $FILTERED_TOPIC \                                                                              
-kf:ds:analyzed $ANALYZED_TOPIC \                                                                        
-kf:save \
> /dev/null 2> /dev/null &

```

</details>

### &#x20;Control Option (부가 설정)

이 옵션은 엔진의 재시작이 필요한 경우 엔진이 중지된 기간 동안 Kafka Queue에 저장되어 있는 데이터의 처리 방법과 유입 량을 제어 합니다.

* 재 시작시 마지막으로 처리된 Offset 부터 이어서 처리 하고자 하는 경우 아래 옵션을 추가 하여 실행 합니다. 이 옵션은 별도의 설정 값 없이 실행 스크립트에 추가 만으로 동작 합니다.

```
-kf:restore
```

<details>

<summary>Example</summary>

```
//Restore 적용 예 
......
-kf:restore
-sp:app $MODULE_NAME \
-zk:conn $ZK_CONN \
.....
```

```
//Restore 미적용 
......
-kf:out_topic $OUT_TOPIC \
-kf:topic $IN_TOPIC \
-kf:err_topic $ERR_TOPIC \
.....
```

</details>

유입량 제어를 위한 옵션은 Spark submitting의 "spark.streaming.backpressure.enabled" 옵션을 사용 합니다. 상세 설명은 공식 문서의 Backpressure 를 참고 하십시오 \[ [Spark submitting 문서](https://spark.apache.org/docs/3.3.0/submitting-applications.html#content) ]

* Backpressure 옵션은 -kf:restore 옵션과 함께 엔진을 재시작 할 경우 짧은 시간에 대량으로 유입되는 이벤트의 양을 조절 하여 엔진의 안정적인 처리를 보장 합니다. 이 옵션이 활성화 되어 있을 경우 Kafka에서 Spark으로 유입되는 이벤트의 양을 조절 하기 때문에 _<mark style="color:red;">**이벤트 적채(처리 지연) 상태에 대해서는 정확히 모니터링 할 수 없음에 유의 하십시오.**</mark>_ 이 옵션이 활성화 되어 있을 경우 처리 지연에 대한 모니터링은 별도의 방법을 사용 하여야 합니다. [(TBD 모니터링 섹션을 참고 하시기 바랍니다.)](../../overview/faq.md)
* 옵션을 활성화 하려면 "spark.streaming.backpressure.enabled"을 true로 설정 합니다.
* "spark.streaming.kafka.maxRatePerPartition"의 값을 조정하여 최대 유입량을 변경 할 수 있습니다.

```
...
--conf "spark.streaming.backpressure.enabled=true" \
--conf "spark.streaming.kafka.maxRatePerPartition=1000" \
.....
```

### 3. Logging

Engine은 Spark application으로 실행 됩니다. 따라서 기본적인 로그 정책은 Spark submitting 옵션 설정을 따릅니다. Engine의 Logging configuration은 엔진 설치 경로의 /conf에서 설정 합니다. 엔진의 로그는 /logs에서 확인 가능 합니다. logs에 기록되는 로그는 Spark Master application의 로그 입니다. Executor의 로그는Executor가 실행되는 각 노드의 Spark 설정을 확인 하십시오. 자세한 설정은 \[[모니터링 페이지를 참고 하십시오](../../operate/monitoring.md)]

### 4. 실행/중지

엔진 설치 디렉토리로 이동하여 start-normalizer.sh를 실행 합니다. 실행 시 발생하는 Error에 대해서는 \[[Trouble shooting 페이지](../../operate/trouble-shooting.md)]를 참고 하시기 바랍니다.

{% hint style="info" %}
권장 설치경로: /opt/sumits/engine/bin
{% endhint %}

```
$ ./sart-normalizer.sh
$ ./stop-normalizer.sh 
```
