---
description: Collect Agent에서 수집된 이벤트를 카프카로 전송하기 위해 이벤트를 수집합니다.
---

# Aggregator

Aggregator는 open source인 Fluentd를 사용합니다.

Fluentd 실행을 위해서는 반드시 필요한 설정인 Basic Configuration file들이 필요합니다. 실행 스크립트는 `start-fluentd.sh` 입니다.

### 1. Basic Configuration

Fluentd 를 실행하기 위해서는 Basic Configuration으로 `td-agent.conf` 파일과 Event 형식을 만들기 위한 formatter 파일인 `formatter-sumit.rb` 이 필요합니다.

Docker Image안에 제공되어있는 File들의 설정값은 개별적인  특정 값은 모두 기본값을 사용하는 것을 권장합니다.

#### 1) Fluentd Configuration

Aggregator를 실행하기 위해 필수적인 메인 Configuration입니다.&#x20;

메인 Configuration은 system, source, match 섹션으로 구분되어 있습니다.&#x20;

각 파라미터들의 default 값은 [\[Log Collector Parameter Tuning\] ](../../../engine/log-collector/parameter-tuning/)페이지를 참고하시기 바랍니다.

<details>

<summary>System</summary>

* workers : 로그 트래픽이 많은 경우, 다중 프로세스 작업자 수
* log\_level : 로깅 레벨( fatal, error, warn, info, debug, trace )

#### \<log> section

* rotate\_age : Aggregator 실행 중 발생하는 로그를 저장할 때 순환 주&#x20;
* rotate\_size : 저장되는 로그 파일 크기

</details>

<details>

<summary>Source</summary>

Collect Agent의 \[INPUT] 섹션과 동일한 섹션입니다.

* @type : Collect Agent 로부터 이벤트를 받기 위해 사용되는 plugin
* tag : output 하려는 이벤트를 구분하기 위해 설정하는 별칭
* port : Collect Agent 가 이벤트를 전송하는 포트 번호
* bind : Collect Agent 에서 이벤트를 전송할 때 listen 하는 Address

</details>

<details>

<summary>Match</summary>

Source 섹션에서 모은 이벤트들을 전송하는 섹션으로 Collect Agent의 \[OUTPUT]과 동일한 기능을 담당합니다.

type 에 따라 파라미터의 종류는 달라지므로 다양한 플러그인에 대한 정보는 [\[OUTPUT Plugin\]](https://docs.fluentd.org/output) 공식 문서 페이지를 참고하기 바랍니다.

* @type : source 섹션에서 사용된 tag와 일치하는 이벤트들을 출력하려는 시스템의 타입을 설정

#### &#x20;\<buffer> section

이벤트들을 Chunk에 모은 후, OUTPUT 처리하는 방식에 필요한 섹션입니다. Chunk 를 저장하는 곳으로 데이터 유실을 최소화하기위한 파라미터들이 존재합니다.&#x20;

* @type : Chunk를 Write하는 곳을 설정
* flush\_mode : Chunk 가 flush 되는 방식
* flush\_interval : Chunk 가 조건을 만족할 때 flush 가 되기까지 걸리는 시간
* flush\_thread_\__count : &#x20;
* chunk\_limit\_size :&#x20;
* total\_limit\_size :&#x20;

</details>

<details>

<summary>label @FLUENT_LOG</summary>



</details>

#### 2) Formatter

Event의 기본 형식은 _Time, Event ID, Sender info, Raw event_로 구성 됩니다.&#x20;

형식 중, _Time_과 _EventID_는 Collect Agent에서 수집한 이벤트를 구분 짓기 위한 특정 정보이고 이 값을 Append 하기 위한 formatter가 필요합니다.

SUMiTS-Onprem 에서는 생성되어 있는 `/etc/td-agent/plugin` 하위의 `default-format` Formatter를 사용하는 것을 권장합니다.

### 2. 실행/중지

Fluentd 설치 디렉토리로 이동하여 `start-fluentd.sh` 스크립트를 실행합니다. 실행 시 발생하는 Error에 대해서는 [\[Aggregator Trouble Shooting\]](../../../engine/log-collector/trouble-shooting/) 페이지를 참고하시기 바랍니다.

{% hint style="info" %}
권장 설치 경로 : /opt/sumits/td-agent
{% endhint %}

```
$ ./start-fluentd.sh
$ ./stop-fluentd.sh
```

### 3. Logging

Aggregator 실행 중 발생한 로그는 fluentd configuration 의 label 섹션에서 설정한 path로 저장됩니다. 기본적으로 설정한 path 는 /var/log/td-agent/logs 이며 기본 경로를 권장합니다. logs에 기록되는 로그들은 Fluentd를 실행했을 때 발생하는 Error 로그입니다. 자세한 Logging 설정은 공식문서 [\[Fluentd Logging\]](https://docs.fluentd.org/deployment/logging) 섹션을 참고 바랍니다.
