---
description: Log Collector 실행하는데 필요한 메인 Configuration의 parameter 설정 내용입니다.
---

# Parameter Tuning

Log Collector의 실행을 위한 메인 configuration은 Source Input, Output, 그리고 설치장비에 따라 설정해주어야하는 Parameters 종와 그 값이 다릅니다. &#x20;

Parameters가 가지는 필수 값 이외에 `path`, `chunk_size` 와 같은 특정 값들은 테스트를 통해 설정한 기본 값이며 그 값을 권장합니다. 하지만 설치 장비 환경과 성능에 따라 변동이 가능하므로 자세한 내용은 [\[Fluentd의 buffer, performance Tuning 등 관련 문서\]](https://docs.fluentd.org/deployment/performance-tuning-single-process), [\[Fluent-bit의 buffering 등 관련 문서\]](https://docs.fluentbit.io/manual/administration/buffering-and-storage) 를 참고 바랍니다.
