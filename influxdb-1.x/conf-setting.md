# Configuring Telegraf and InfluxDB

<br>
<br>

### **Telegraf Configuration**

---

<br>

Telegraf 의 config 파일에 대한 좀 더 세부적인 내용을 정리합니다.

<br>
<br>

**Agent configuration**

<br>

Telegraf 에는 구성 [agent] 섹션에서 구성 할 수있는 몇 가지 옵션이 있다 .

<br>

- interval : 모든 입력에 대한 기본 데이터 수집 간격

<br>

- round_interval : 수집 간격을 설정된 interval 의 간격으로 반올림한다.

  예를 들어, interval가 10s로 설정된 경우 항상 : 00, : 10, : 20 ... 에 수집한다.

<br>

- metric_batch_size : Telegraf는 메트릭을 최대 metric_batch_size 메트릭 배치로 출력에 보낸다.

<br>

- metric_buffer_limit : Telegraf는 metric_buffer_limit 각 출력에 대한 메트릭을 캐시 하고 성공적인 쓰기 시, 이 버퍼를 플러시한다.

  이 값은 metric_batch_size 의 배수여야 하며 2배 이상이어야 한다.

<br>

- collection_jitter : 컬렉션 지터는 임의의 양으로 컬렉션을 지터하는 데 사용된다.

  각 플러그인은 수집하기 전에 지터 내에서 임의의 시간 동안 휴면상태가 된다.

  이것은 sysfs 와 같은 것들을 동시에 쿼리하는 많은 플러그인을 피하기 위해 사용될 수 있으며, 이는 시스템에 상당한 영향을 줄 수 있다.

<br>

- flush_interval : 모든 출력에 대한 기본 데이터 플러시 간격.

  최대 flush_interval은 flush_interval + flush_jitter 이다.

<br>

- flush_jitter : 플러시 간격을 무작위로 지터한다.

  이는 주로 많은 수의 Telegraf 인스턴스를 실행하는 사용자의 쓰기 급증을 방지하기위한 것이다.

  예를 들어, flush_jitter 5s 및 flush_interval 10s 는 플러시가 10-15 초마다 발생 함을 의미한다.

<br>

- precision : 기본적으로 정밀도는 수집 간격과 동일한 타임 스탬프 순서로 설정되며 최대 값은 1이다.

  logparser 및 statsd 와 같은 서비스 입력에는 정밀도가 사용되지 않는다. 유효한 값은 "ns","us" (또는"µs"), "ms","s" 다.

<br>

- logfile : 로그 파일 이름을 지정한다. 빈 문자열은 stderr 에 로그인하는 것을 의미한다.

<br>

- debug : 디버그 모드에서 Telegraf를 실행한다.

<br>

- quiet : Telegraf를 자동 모드로 실행한다 (오류 메시지만 해당).

<br>

- hostname : 비어있는 경우 기본 호스트 이름을 재정의한다. os.Hostname().

<br>

- omit_hostname : true이면 Telegraf agent 에서 "host" 태그를 설정하지 않는다.

<br>
<br>

**Input Configuration**

<br>
