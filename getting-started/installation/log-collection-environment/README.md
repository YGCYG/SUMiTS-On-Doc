# Log collection environment

## Before Installation

SUMiTS On-prem Collector는 Docker Image를 파일로 배포하여 설치되기 때문에 아래 Components를 필요로 합니다.

* Docker&#x20;

### 경로 선택 및 압축 해제

Update history 를 참고하여 설치하고자 하는 버전의 바이너리를 다운로드 하고 압축을 해제합니다. 바이너리의 구성은 다음과 같습니다.

* Fluent-bit / Fluentd Docker Image
* Script File : Docker Image 설치 스크립트

{% hint style="info" %}
권장 설치 경로 : /opt/sumits/fluentd

&#x20;                          /opt/sumits/fluent-bit
{% endhint %}
