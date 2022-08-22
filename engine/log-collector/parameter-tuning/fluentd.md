---
description: td-agent.conf parameter 설정 정보입니다.
---

# Fluentd

#### 1. Source 섹션

<details>

<summary>forward</summary>



</details>

#### 2. output 섹션

<details>

<summary>kafka2</summary>

```
<match from_bit.**>
  @type kafka2
  brokers 10.250.238.73:9092
  topic_key logStorage
  default_topic logStorage
  <format>
   @type  default-format
  </format>
  <buffer logStorage>
    @type memory
    flush_mode interval
    flush_interval 1s
    chunk_limit_size 800000
    flush_thread_count 8
    total_limit_size 100m
  </buffer>
</match>
```

</details>

#### 3. buffer 섹션&#x20;

<details>

<summary>file buffer</summary>

```
<buffer logStorage>
    @type memory
    flush_mode interval
    flush_interval 1s
    chunk_limit_size 800000
    flush_thread_count 8
    total_limit_size 100m
  </buffer>

```

</details>

