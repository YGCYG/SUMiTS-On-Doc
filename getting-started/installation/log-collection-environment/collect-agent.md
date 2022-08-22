---
description: 다양한 센서와 보안장비로부터 이벤트를 수집합니다.
---

# Collect Agent

Collector Agent는 open source인 Fluent-bit를 사용합니다.

Fluent-bit 실행을 위해서는 반드시 필요한 설정인 Basic Configuration file들이 필요합니다. 실행 스크립트는 start-fluentbit.sh 입니다.

### 1. Basic Configuration

Basic Configuration file들에는 센서와 보안 장비들에 따라 다르게 설정하는 fluent-bit.conf 파일과 들어오는 Event의 포멧을 변환하지 않고 Aggregator로 보내기 위한 parser.conf, 마지막으로 수집된 이벤트를 보낼 Aggregator의 전송 정보를 설정하는 upstreams.conf 를 필수적으로 필요로합니다.&#x20;

* Basic Configuration 을 구성하는 파일들은 각각의 Section을 가지고 모든 Section은 대문자로 표기합니다.
* 기본적으로 Docker Image에 포함되어 있는 Configuration의 설정값을 권장합니다.
* 수집되는 센서와 보안장비의 이벤트마다 fluent-bit.conf 파일의 INPUT Section의 설정 차이가 있습니다.
* 각 파라미터들의 default 값은 [\[Log Collector Parameter Tuning\] ](../../../engine/log-collector/parameter-tuning/)페이지를 참고하시기 바랍니다.
* 이 외에 별도의 추가 설정이 필요할 경우 [\[ Fluent-bit Configuration \]](https://docs.fluentbit.io/manual/administration/configuring-fluent-bit/classic-mode/configuration-file) 공식문서를 참고바랍니다.

#### 1) Fluent-bit configuration

Collect Agent 실행을 위해 메인 configuration file이고 \[SERVICE], \[INPUT], \[OUTPUT] 섹션을 가지고 있습니다.

<details>

<summary>[SERVICE]</summary>

* \[SERVICE] 섹션은 서비스 관련된 전역 속성들을 정의합니다.
* 해당 섹션의 속성들은 기본 값을 권장하며 Log path만을 개별적으로 수정할 수 있습니다.

</details>

<details>

<summary>[INPUT]</summary>

* \[INPUT] 섹션은 센서나 보안장비에서 이벤트를 보내는 프로토콜을 정의합니다.
* name : 프로토콜의 이름으로 현재 지원하는 프로토콜은 File, TCP/UDP, HTTP, MQTT 입니다.

</details>

<details>

<summary>[OUTPUT] </summary>

* \[OUTPUT] 섹션은 수집한 이벤트를 전송하는 plugin입니다.
* Aggregate Server로 이벤트를 TCP전송하는 것을 기본으로 합니다.
* Aggregator의 전송정보는 upstreams.conf에서 설정합니다.

</details>

#### 2) Upstream configuration

Aggregator로 이벤트를 전송하기 위한 전송 정보를 설정하는 configuration입니다.

<details>

<summary>UPSTREAM</summary>



</details>

#### 3) Parser configuration

수집된 Event는 JSON, PlainText 등 포맷에 관계없이 Event 그대로 Aggregator로 전송을 위해서 File INPUT 플러그인은 별도의 parser가 필요합니다. 기본 parser는 `sumit-custom` 을 사용하며 변경하지 않을 것을 권장합니다.

### 2. 실행/중지

Fluent-bit 설치 디렉토리로 이동하여 `start-fluentbit.sh` 스크립트를 실행합니다. 실행시 발생하는 Error에 대해서는 [\[Collect Agent Trouble shooting\] ](../../../engine/log-collector/trouble-shooting/)페이지를 참고 하시기 바랍니다.

{% hint style="info" %}
권장 설치 경로 : /opt/sumits/fluent-bit
{% endhint %}

```shell
$ ./start-fluentbit.sh
$ ./stop-fluentbit.sh
```

### 3. Logging

Collect Agent는 fluent-bit configuration의 \[SERVICE]섹션에서 설정한 log-path로 저장이 되어 확인이 가능합니다. 기본적으로 설정되는 log-path는 `/var/log/fluent-bit/logs` 이며 기본 값을 권고합니다. logs에 기록되는 로그는 fluent-bit를 실행했을 때  발생하는 error 로그입니다. 자세한 설정은 [공식문서 \[Fluent-bit service 섹션\]](https://docs.fluentbit.io/manual/administration/configuring-fluent-bit/classic-mode/configuration-file) 참고 바랍니다.&#x20;
