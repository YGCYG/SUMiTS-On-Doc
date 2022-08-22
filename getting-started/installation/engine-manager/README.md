# Engine manager

## Before Installation

Engine Manager가 동작 하기 위해서는 아래 Components를 필요로 합니다.&#x20;

* JAVA 11
* Nginx
* Postgresql (외부 DB 사용 시 불필요)

### 경로 선택 및 압축 해제

​[Update history](../../overview/update-history/) 를 참고 하여 설치하고자 하는 버전의 바이너리를 다운로드 하고 압축을 해제 합니다. 바이너리의 디렉토리 구성은 다음과 같습니다.

* bin: engine manager 바이너리 실행 스크립트
* conf: 로깅 및 추가 설정
* logs: Application 로그
* pipelines: JSON format의 pipeline 저장 경로

{% hint style="info" %}
권장 설치경로: /opt/sumits/engine-manager
{% endhint %}

```bash
$ mkdir /opt/sumits/
$ cp sumits-on-prem-engine-manager-1.0.tgz /opt/sumits
$ tar -xvf sumits-on-prem-engine-manager-1.0.tgz
```

