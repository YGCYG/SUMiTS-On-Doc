---
description: fluent-bit.conf parameter 설정 정보입니다.
---

# Fluent-bit

기본 4칸 들여쓰기

#### 1. Source \[ INPUT ] 섹션

<details>

<summary>FILE</summary>

File 은 fluent-bit 에 tail 플러그인을 사용합니다.  Sample의  파라미터들은 모두 필수 요소이자 기본 값입니다. 파일경로 `path` 를 제외한 파라미터들은 모두 기본 값을 설정하 것을 권고합니다.

#### Sample

```ruby
[INPUT]
    name tail
    tag test.file
    path /opt/sumits/sample/cisco_fw_20
    refresh_interval 5
    parser sumits-custom
    storage.type filesystem
```

</details>

<details>

<summary>TCP</summary>

TCP 플러그인은 이벤트를 장비/센서로부터 listen하는 주소와 port 외의 parameter를 변경하지않을 것을 권장합니다.

* tcp 플러그인으로 수집되는 이벤트는 이벤트 마지막에 `\n` 개행문자가 필요합니다.&#x20;
* 기본적으로 JSON 타입의 이벤트를 수신하기 때문에 `format  none` 설정이 필요합니다.

```ruby
[INPUT]
    name tcp
    tag test.tcp
    listen 0.0.0.0
    port 5170
    format none
    separator \n
    mem_buf_limit 100mb
    storage.type filesystem
```

</details>

<details>

<summary>UDP</summary>

UDP 플러그인은 이벤트를 장비/센서로부터 listen하는 주소와 port 외의 parameter를 변경하지않을 것을 권장합니다.

* 로그의 유실 방지/backoff 기능을 위해 fileSystem storage를 사용니다.
* 다양한 이벤트 포맷을 수용하기 위해 자체 Parser `sumits-custom`을 사용합니다.

```
[INPUT]
    name syslog
    tag test.udp
    mode udp
    listen 0.0.0.0
    port 8125
    parser sumits-custom
    mem_buf_limit 100m
    storage.type filesystem
```

</details>

<details>

<summary>HTTP</summary>

HTTP 플러그인은 이벤트를 장비/센서로부터 listen하는 주소와 port 외의 parameter를 변경하지않을 것을 권장합니다.

* JSON 형식의 이벤트만 수집하기 때문에 기본적으로 장비에서 HTTP 플러그인을 통해 이벤트를 송신할 시 JSON 형식의 이벤트만 송신해야합니다.

```
[INPUT]
    name http
    tag test.http
    listen 0.0.0.0
    port 8888
    mem_buf_limit 100mb
    storage.type filesystem
```

</details>

<details>

<summary>MQTT</summary>

MQTT 플러그인은 이벤트를 장비/센서로부터 listen하는 주소와 port 외의 parameter를 변경하지않을 것을 권장합니다.

* JSON 형식의 이벤트만 수집하기 때문에 기본적으로 장비에서 HTTP 플러그인을 통해 이벤트를 송신할 시 JSON 형식의 이벤트만 송신해야합니다.

```
[INPUT]
    name mqtt
    tag test.mqtt
    listen 0.0.0.0
    port 1883
    storage.type filesystem
```

</details>

#### 2. \[ OUTPUT ] 섹션

<details>

<summary>Aggregator</summary>

Collect Agent 는 Aggregator로 수집한 이벤트를 모두 Aggregator로만 TCP 전송하도록 설정하였습니다. 후에 필요한 output이 발생할 경우, 공식문서[ \[fluent-bit output plugin\]](https://docs.fluentbit.io/manual/pipeline/outputs) 을 참고바랍니다.&#x20;

Load-balancing 등의 이유로 단일 호스트 연결 대신 여러 대의 Aggregator로 전송할 수 있습니다. 이 경우는 `upstream`파라미터를 사용하여 다중 호스트로의 연결이 가능합니다.

`upstream` 파라미터는 upstreams.conf 절대적 경로의 파일을 설정하여 기능을 사용할 수 있으며 파일명과 경로는 변경하지 않는 것을 권고합니다.

```
[OUTPUT]
    name forard
    match *
    upstream upstreams.conf
```

</details>

#### 3. UPSTREAM.CONF FILE&#x20;

<details>

<summary>NODE</summary>

* 한개의 upstream file에 여러 개의 노드를 추가하여 다중 호스트 연결 정보를 설정할 수 있습니다.

```
[UPSTREAM]
    name       forward-balancing   
[NODE]
    name       AGGREGATOR1
    host       10.250.238.72 
    port       43000

[NODE]
    name       AGGREGATOR2
    host       10.250.238.73
    port       44000
```

</details>
