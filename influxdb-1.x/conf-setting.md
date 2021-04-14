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

Telegraf 에는 구성 [agent] 섹션에서 구성 할 수있는 몇 가지 옵션이 있다.

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

- interval : 이 메트릭을 수집하는 빈도.

  일반 플러그인은 단일 전역 간격을 사용하지만 하나의 특정 입력을 적게 또는 더 자주 실행해야하는 경우 여기서 이를 구성 할 수 있다.

<br>

- name_override : 측정의 기본 이름을 무시한다. (기본값은 입력 이름)

<br>

- name_prefix : 측정 이름에 첨부 할 접두사를 지정한다.

<br>

- name_suffix : 측정 이름에 첨부 할 접미사를 지정한다.

<br>

- tags : 특정 입력의 측정에 적용 할 태그 맵이다.

<br>
<br>

**Output Configuration**

<br>

모든 출력에 사용할 수 있는 일반 구성 옵션은 없다.

<br>
<br>

**Aggregator Configuration**

<br>

- period : 각 애그리 게이터를 비우고 지울 기간.

  이 기간 이외의 타임 스탬프와 함께 전송 된 모든 메트릭은 수집기에서 무시된다.

<br>

- delay : 각 애그리 게이터가 플러시되기 전의 지연.

  이는 애그리 게이터가 플러시되고 입력이 동일한 간격으로 수집되는 경우 애그리 게이터가 입력 플러그인에서 메트릭을 수신하기 전에 대기하는 시간을 제어하는 ​​것이다.

<br>

- drop_original : true 인 경우 애그리 게이터가 원래 메트릭을 삭제하고 출력 플러그인으로
  전송되지 않는다.

<br>

- name_override : 측정의 기본 이름을 무시한다. (기본값은 입력 이름)

<br>

- name_prefix : 측정 이름에 첨부할 접두사를 지정한다.

<br>

- name_suffix : 측정 이름에 첨부 할 접미사를 지정한다.

<br>

- tags : 특정 입력의 측정에 적용 할 태그 맵이다.

<br>
<br>

**Processor Configuration**

<br>

- order : 프로세서가 실행되는 순서.

  이를 지정하지 않으면 프로세서 실행 순서는 임의적이다.

<br>

- namepass : glob 패턴 문자열의 배열.

  측정 이름이 이 목록의 패턴과 일치하는 포인트만 방출된다.

<br>

- namedrop : namepass 의 반대.

  일치하는 것이 발견되면 포인트가 삭제된다.

  이름 namepass 테스트를 통과 한 후 포인트에서 테스트된다.

<br>

- fieldpass : glob 패턴 문자열의 배열.

  필드 키가 이 목록의 패턴과 일치하는 필드만 방출된다.

  출력에는 사용할 수 없다.

<br>

- fielddrop : 의 역 fieldpass.

  패턴 중 하나와 일치하는 필드 키가 있는 필드는 포인트에서 삭제된다.

  출력에는 사용할 수 없다.

<br>

- tagpass : 태그 키를 glob 패턴 문자열의 배열에 매핑하는 테이블.

  테이블에 태그 키가 포함 된 포인트와 패턴 중 하나와 일치하는 태그 값만 방출된다.

<br>

- tagdrop :의 역 tagpass.

  일치하는 것이 발견되면 포인트가 삭제된다.

  tagpass 테스트를 통과 한 후 포인트에서 테스트된다.

<br>

- taginclude : glob 패턴 문자열의 배열.

  패턴 중 하나와 일치하는 태그 키가있는 태그만 방출된다.

  태그를 기준으로 전체 포인트를 전달하는 tagpass 와 달리 taginclude 는 포인트에서 일치하지 않는 모든 태그를 제거한다.

  이 필터는 입력 및 출력 모두에 사용할 수 있지만 처리 지점에서 태그를 필터링하는 것이 더 효율적이므로 입력에 사용하는 것이 좋다.

<br>

- tagexclude : 의 역 taginclude. 패턴 중 하나와 일치하는 태그 키가있는 태그는 포인트에서 삭제됩니다.

<br><br>

### **InfluxDB Configuration**

---

<br>

InfluxDB 는 구성파일 (influxdb.conf) 및 환경 변수를 사용하여 구성된다.

구성 옵션의 주석 처리를 제거하지 않으면 시스템은 기본 설정을 사용한다.

<br>

기간을 지정하는 구성 설정은 다음 기간 단위를 지원한다.

- ns(nanoseconds)
- us(microseconds)
- ms(milliseconds)
- s(seconds)
- m(minutes)
- h(hours)
- d(days)
- w(weeks)

<br>
<br>

**Environment Variables**

<br>

모든 구성 옵션은 구성 파일 또는 환경 변수에서 지정할 수 있다.

환경 변수는 구성 파일의 해당 옵션을 재정의한다.

구성 파일 또는 환경 변수에 구성 옵션이 지정되지 않은 경우, InfluxDB는 내부 기본 구성을 사용한다.

<br><br>

**Using the configuration file**

<br>

InfluxDB 시스템에는 구성 파일의 모든 설정에 대한 내부 기본값이 있다.

기본 구성 설정을 보려면, influxd config 명령을 사용한다.

주석 처리 된 설정은 내부 기본값으로 설정된다.

주석 처리되지 않은 설정은 내부 기본값보다 우선한다.

로컬 구성 파일이 모든 구성 설정을 포함 할 필요는 없다.

<br><br>

### **Global settings**

<br>

**reporting-disabled = false**

<br>

- 회사의 InfluxData 는 주로 실행중인 노드에서 보고 된 데이터를 사용하여 다양한 InfluxDB 버전의 채택률을 추적한다.

  이 데이터는 InfluxData가 지속적인 InfluxDB 개발을 지원하는 데 도움이 된다.

  reporting-disabled 옵션은 데이터를 24시간마다 usage.influxdata.com 에 보고한다.

  각 보고서에는 무작위로 생성 된 식별자, OS, 아키텍처, InfluxDB 버전 및 데이터베이스 수 , 측정 값 및 고유 시리즈가 포함 된다.

  이 옵션을 true 로 설정하면 보고 기능이 비활성화된다.

<br><br>

**bind-address = "127.0.0.1 : 8088"**

<br>

- 백업 및 복원을 위해 RPC 서비스에 사용할 바인드 주소.

<br><br>

### **Metastore settings**

<br>

**[meta]**

이 섹션에서는 사용자, 데이터베이스, 보존 정책, 샤드 및 연속 쿼리에 대한 정보를 저장하는 InfluxDB 메타스토어의 매개 변수를 제어한다.

<br>

- dir = "/var/lib/influxdb/meta"

  meta 디렉토리. meta 디렉토리의 파일에는 meta.db 가 포함된다.

  <br>

- retention-autocreate = true

  데이터베이스가 생성될 때 기본 보존 정책 자동 생성 기능을 자동으로 생성한다.

  보존 정책 자동 생성기는 기간이 무한하며 데이터베이스의 기본 보존 정책으로 설정되기도 한다.

  이 정책은 쓰기 또는 쿼리가 보존 정책을 지정하지 않을 때 사용된다.

  데이터베이스를 작성할 때 이 보존 정책이 작성되지 않도록 하려면 이 설정을 false 로 설정한다.

<br>

- logging-enabled = true

  메타 서비스의 메시지 로깅을 사용한다.

<br><br>

### **Data settings**

<br>

**[data]**

이 섹션에서는 InfluxDB 의 실제 샤드 데이터가 있는 위치와 WAL에서 데이터가 플러시되는 방법을 제어한다. dir 은 시스템에 적합한 위치로 변경해야 할 수도 있지만 WAL 설정은 고급 구성이다. 기본값은 대부분의 시스템에서 작동한다.

<br>

- dir = "/var/lib/influxdb/data"

  InfluxDB 가 데이터를 저장하는 디렉토리. 이 디렉토리는 변경가능하다.

<br>

- wal-dir = "/var/lib/influxdb/wal"

  미리 쓰리 로그(Write Ahead Log) 파일의 디렉토리 위치다.

<br>

- wal-fsync-delay = "0s"

  fsync 를 하기 전에 쓰기가 대기하는 시간.

  여러 fsync 호출을 일괄 처리하려면 0 보다 긴 기간을 사용해야한다.

  이것은 디스크 속도가 느리거나 WAL 쓰기 경합이 발생할 때 유용하다.

  fsyncs 의 기본값은 0s. WAL에 쓸 때마다 수행된다.

  ssd disk 가 아니라면 0ms - 100ms 범위의 값이 권장된다.

<br>

- index-version = "inmem"

  새 샤드에 사용할 샤드 인덱스 유형.

  기본값은 시작시 다시 생성되는 메모리 내의 인덱스다.

  tsi1 값은 더 높은 카디널리티 데이터 세트를 지원하는 디스크 기반 인덱스를 사용한다.

<br>

- trace-logging-enabled = false

  TSM 엔진 및 WAL 내에서 추가 디버그 정보에 대한 자세한 로깅을 사용한다.

  추적 로깅은 TSM 엔진 문제를 디버깅하는 데 더 유용한 출력을 제공한다.

<br>

- query-log-enabled = true

  실행 전에 구문 분석된 쿼리의 로깅을 활성화한다.

  쿼리 로그는 문제 해결에 유용 할 수 있지만 쿼리에 포함 된 모든 민감한 데이터를 기록한다.

<br>

- validate-keys = false

  들어오는 쓰기의 유효성을 검사하여 키에 유효한 유니 코드 문자만 있는지 확인한다.

  이 설정은 모든 키를 확인해야하므로 약간의 오버 헤드가 발생한다.

<br>

### **Settings for the TSM engine**

<br>

- cache-max-memory-size = "1g"

  쓰기 거부를 시작하기 전에 샤드 캐시가 도달할 수있는 최대 크기.

  유효 메모리 크기 접미사는 k, m, g (대소문자 구별 - 1K = 1,024).

  크기 접미사가 없는 값은 바이트 단위다. (ex> 1073741824)

<br>

- cache-snapshot-memory-size = "25m"

  엔진이 캐시를 스냅 샷하고 TSM 파일에 기록하여 메모리를 확보하는 크기.

  유효 메모리 크기 접미사은 k, m, g (대소 문자 구별, 1K = 1,024).

  크기 접미사가없는 값은 바이트 단위다.

<br>

- cache-snapshot-write-cold-duration = "10m"

  샤드가 쓰기 또는 삭제를 수신하지 않은 경우, 엔진이 캐시를 스냅 샷하고 새 TSM 파일에 쓰는 시간 간격.

<br>

- compact-full-write-cold-duration = "4h"

  쓰기 또는 삭제를 수신하지 않은 경우, TSM 엔진이 샤드의 모든 TSM 파일을 압축하는 시간 간격.

<br>

- max-concurrent-compactions = 0

  한 번에 실행할 수있는 동시 전체 및 레벨 압축의 최대 수.

  기본값 0 은 압축을 위해 런타임시 CPU 코어의 50%가 사용된다.

  명시적으로 설정하면 압축에 사용되는 코어 수가 지정된 값으로 제한된다.

  이 설정은 캐시 스냅 샷에 적용되지 않는다.

<br>

- compact-throughput = "48m"

  TSM 압축이 디스크에 쓰는 초당 최대 바이트 수.

  기본값은 "48m"(480,000,000).

  short burst 는 compact-throughput-burst 에 의해 설정된 더 큰 값에서 발생할 수 있다.

<br>

- compact-throughput-burst = "48m"

  short burst 동안 TSM 압축이 디스크에 쓰는 최대 초당 바이트 수.

<br>

- tsm-use-madv-willneed = false

  true 일 경우, MMap Advisory 값 MADV_WILLNEED 는 입력/출력 페이징 측면에서 매핑된 메모리 영역을 처리하는 방법과 TSM 파일과 관련하여 매핑된 메모리 영역에 대한 액세스를 예상하는 방법에 대해 커널에 조언한다.

  이 설정은 일부 커널(CentOS 및 RHEL 포함) 에서 문제가 있으므로 기본값은 false.

  값을 true로 변경하면 경우에 따라 Disk가 느린 사용자에게 도움이 될 수 있다.

<br>

### **In-memory (inmem) index settings**

<br>

- max-series-per-database = 1000000

  데이터베이스 당 허용되는 최대 시리즈 수.

  기본 설정은 1000000.

  데이터베이스 당 무제한 시리즈 수를 허용 하려면 설정을 0 으로 변경하면 된다.

  포인트로 인해 데이터베이스의 시리즈 수가 max-series-per-database 를 초과하면 InfluxDB는 포인트를 쓰지 않고 다음 오류와 함께 500 을 반환한다.

      {"error":"max series per database exceeded: <series>"}

<br>

- max-values-per-tag = 100000

  태그 키당 허용되는 최대 태그 값 수.

  기본값은 100000.

  태그 키당 태그 값 수를 제한하지 않으려면 설정을 0으로 변경하면 된다.

  태그 값으로 인해 태그 키의 태그 값 수가 태그당 최대값을 초과할 경우, InfluxDB는 포인트를 쓰지 않고 partial write 오류를 반환한다.

<br>

### **TSI (tsi1) index settings**

<br>

- max-index-log-file-size = "1m"

  인덱스 WAL(Write-ahead Log) 파일이 인덱스 파일로 압축되는 임계값(바이트).

  크기가 작을수록 로그 파일이 더 빨리 압축되고 쓰기 처리량을 희생하여 힙 사용량이 줄어든다.

  크기가 클수록 압축 빈도가 낮아지고, 더 많은 시리즈 인메모리를 저장하며, 더 높은 쓰기 처리량을 제공한다.

  올바른 크기 접미사는 k, m, g (대소문자 구분, 1024 = 1k).

  크기 접미사가 없는 값은 바이트다.

<br>

- series-id-set-cache-size = 100

  TSI 인덱스에서 이전에 계산된 시리즈 결과를 저장하는 데 사용되는 내부 캐시 크기.

  일치하는 태그 키-값 술어를 가진 후속 쿼리를 실행할 때 다시 계산하지 않고 캐시된 결과가 캐시에서 빠르게 반환된다.

  이 값을 0으로 설정하면 캐시가 비활성화되어 쿼리 성능 문제가 발생할 수 있다.

  데이터베이스에 대한 모든 측정에서 정기적으로 사용되는 태그 키-값 술어 집합이 100 보다 큰 경우에만 이 값을 증가시켜야 한다.

  캐시 크기가 증가하면 힙 사용량이 증가할 수 있다.

<br>

### **Query management settings**

<br>

**[coordinator]**

이 섹션에는 쿼리 관리를 위한 구성 설정이 포함되어 있다.

<br>

- write-timeout = "10s"

  쓰기 요청이 "시간 초과" 오류가 호출자에게 반환될 때까지 대기하는 기간.

  기본값은 10초다.

<br>

- max-concurrent-queries = 0

  인스턴스에서 허용된 최대 실행 쿼리 수.

  기본 설정 (0) 에서는 쿼리 수를 제한하지 않는다.

<br>

- query-timeout = "0s"

  InfluxDB가 쿼리를 종료하기 전에 쿼리가 실행될 수 있는 최대 기간.

  기본 설정 (0) 에서는 시간 제한 없이 쿼리를 실행할 수 있다.

  이 설정은 (ns, us, ms, s, m, h, d, w) 다.

<br>

- log-queries-after = "0s"

  InfluxDB가 "Detected slow query" 메시지와 함께 쿼리를 기록 할 때 까지 쿼리가 수행 할 수있는 최대 기간.

  기본 설정 ("0") 은 InfluxDB 에 쿼리를 기록하도록 지시하지 않는다.

  이 설정은 (ns, us, ms, s, m, h, d, w) 다.

<br>

- max-select-point = 0

  SELECT 명령문이 처리 할 수 있는 최대 포인트 수.

  기본 설정 (0) 을 사용하면 SELECT 명령문이 무제한의 포인트를 처리 할 수 ​​있다.

<br>

- max-select-series = 0

  SELECT 명령문이 처리 할 수 있는 최대 시리즈 수.

  기본 설정 (0) 을 사용하면 SELECT 명령문이 무제한의 시리즈를 처리할 수 있다.

<br>

- max-select-buckets = 0

  GROUP BY time() 쿼리가 처리 할 수있는 최대 버킷 수.

  기본 설정 (0) 을 사용하면 쿼리가 무제한의 버킷을 처리 할 수 ​​있다.

<br>

### **Retention policy settings**

<br>

**[retention]**

이 설정은 오래된 데이터를 제거하기 위한 보존 정책의 시행을 제어한다.

<br>

- enabled = true

  false 로 설정하면 보존 정책을 시행하지 않는다.

<br>

- check-interval = "30m0s"

  InfluxDB가 보존 정책을 시행하기 위해 확인하는 시간 간격.

<br>

### **Shard precreation settings**

<br>

**[shard-precreation]**

이 [shard-precreation]설정은 데이터가 도착하기 전에 샤드를 사용할 수 있도록 샤드의 사전 생성을 제어한다.

생성 후 향후 시작 및 종료 시간이 모두 있는 샤드만 생성된다.

전체적으로 또는 부분적으로 과거에 있었던 샤드는 결코 미리 생성되지 않는다.

<br>

- enabled = true

  샤드 사전 생성 서비스의 활성화 여부를 결정한다.

<br>

- check-interval = "10m"

  새 샤드 사전 생성 확인이 실행되는 시간 간격.

<br>

- advance-period = "30m"

  InfluxDB가 샤드를 미리 생성하는 미래의 최대 기간.

  30m 기본값은 대부분의 시스템에 적용된다.

  향후 이 설정을 너무 많이 늘리면 비효율성이 발생할 수 있다.

<br>

### **Monitoring settings**

<br>

**[monitor]**

[monitor] 섹션 설정은 InfluxDB 시스템 자체 모니터링을 제어한다.

기본적으로 InfluxDB는 데이터를 \_internal 데이터베이스에 기록한다.

데이터베이스가 없는 경우 InfluxDB는 데이터베이스를 자동으로 생성한다.

\_internal 데이터베이스의 기본 보존 정책은 7일.

7일 보존 정책 이외의 보존 정책을 사용하려면 해당 정책을 생성해야 한다.

<br>

- store-enabled = true

  내부적으로 통계를 기록하지 않으려면 false로 설정한다.

  하지만 false 로 설정하면 설치 문제를 진단하기가 상당히 어려워진다.

<br>

- store-database = "\_internal"

  기록 된 통계의 대상 데이터베이스.

<br>

- store-interval = "10s"

  InfluxDB가 통계를 기록하는 시간 간격.

  기본값은 10초 (10s).

<br>

### **HTTP endpoints settings**

<br>

**[http]**

[http] 섹션 설정은 InfluxDB가 HTTP 엔드포인트를 구성하는 방법을 제어한다.

이러한 메커니즘은 InfluxDB로 데이터를 가져오거나 내보내는 주요 메커니즘이다.

이 섹션의 설정을 편집하여 HTTPS 및 인증을 사용하도록 설정한다.

<br>

- enabled = true

HTTP 끝 점이 활성화되었는지 여부를 결정한다.

HTTP 엔드포인트 대한 액세스를 비활성화하려면 값을 false 로 설정하면 된다.

InfluxDB 명령 줄 인터페이스 (CLI) 는 InfluxDB API 를 사용하여 데이터베이스에 연결된다.

<br>

- flux-enabled = false

Flux 쿼리 끝 점이 활성화되었는지 여부를 확인한다.

Flux 쿼리를 사용하려면 값을 true 로 설정하면 된다.

<br>

- bind-address = ":8086"

HTTP 서비스에서 사용하는 바인드 주소 (포트).

<br>

- auth-enabled = false

사용자 인증이 HTTP 및 HTTPS를 통해 활성화되는지 여부를 결정한다.

인증을 요구하려면 값을 true 로 설정하면 된다.

<br>

- realm = "InfluxDB"

기본 인증 문제를 발급할 때 다시 전송되는 기본 영역.

영역은 HTTP 엔드 포인트에 사용되는 JWT 영역이다.

<br>

- log-enabled = true

HTTP 요청 로깅을 사용할지 여부를 결정한다.

로깅을 비활성화하려면 값을 false 로 설정하면 된다.

<br>

- suppress-write-log = false

로그가 활성화 된 경우, HTTP 쓰기 요청 로그를 억제할지 여부를 결정한다.

<br>

- access-log-path = ""

  `log-enabled = true` 를 사용하여 상세 쓰기 로깅을 사용할지 여부를 결정하는 액세스 로그 경로.

  활성화된 경우, HTTP 요청 로깅을 지정된 경로에 쓸지 여부를 지정한다.

  influxd 가 지정된 경로에 액세스할 수 없으면 오류를 기록하고 stderr로 돌아간다.

  HTTP 요청 로깅을 사용하도록 설정한 경우, 이 옵션은 로그 항목을 기록할 경로를 지정한다.

  지정되지 않은 경우, 기본값은 stderr에 쓰는 것으로 HTTP 로그를 내부 InfluxDB 로깅과 혼합한다.

  influxd 가 지정된 경로에 액세스할 수 없는 경우, 오류를 기록하고 다시 stderr에 요청 로그를 쓴다.

<br>

- access-log-status-filters = []

  기록할 요청을 필터링한다.

  각 필터는 nnn, nnx 또는 nxx 패턴이며, 여기서 n은 숫자이고 x는 숫자에 대한 와일드카드다.

  모든 5xx 응답을 필터링하려면 5xx 문자열을 사용하면 된다.

  여러 개의 필터를 사용하는 경우 하나만 일치해야 한다.

  기본값은 no filter 로 모든 요청이 인쇄되는 필터 없음이다.

<br>

- write-tracing = false

  상세 쓰기 로깅을 사용할지 여부를 결정한다.

  쓰기 페이로드에 대한 로깅을 활성화하려면 true로 설정한다.

  true로 설정하면 로그의 모든 쓰기 문이 복제되므로 일반 용도로는 권장되지 않는다.

<br>

- pprof-enabled = true

  /net/http/pprof HTTP 엔드포인트가 사용 가능한지 여부를 결정한다.

  문제 해결 및 모니터링에 유용하다.

<br>

- pprof-auth-enabled = false

  /debug 엔드포인트에서 인증을 사용하도록 설정한다.

  사용 가능한 경우 다음 엔드포인트에 액세스하려면 관리자 권한이 필요하다.

  - /debug/pprof
  - /debug/requests
  - /debug/vars

  이 설정은 auth-enabled 또는 pprof-enabled 이 false 로 설정된 경우에는 적용되지 않는다.

<br>

- debug-pprof-enabled = false

  기본 /pprof 엔드포인트를 사용하도록 설정하고 localhost:6060에 대해 바인딩한다.

  시작 성능 문제를 디버깅하는 데 유용하다.

<br>
