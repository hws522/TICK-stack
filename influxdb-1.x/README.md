# InfluxDB 1.x ver - EOCR Monitoring Setting

<br>
<br>

### **설치**

---

<br>

windows 기준으로 Telegraf, InfluxDB, Chronograf 설치방법을 정리한다.

<br>
<br>

**InfluxDB**

<br>

- https://dl.influxdata.com/influxdb/releases/influxdb-1.8.4_windows_amd64.zip

  링크를 눌러 설치 한다. 👆

<br>

- 원하는 경로에 압축을 푼다. (ex> D:\InfluxData\influxdb\influxdb-data)

<br>

- influxdb.conf 파일로 들어가서 설정된 경로를 바꾼다. 이때 경로는 처음 InfluxDB를 설치할 때 설정해줬던 경로이다.

  - [meta]

    dir = "D:/InfluxData/influxdb/influxdb-data/meta"

  - [data]

    dir = "D:/InfluxData/influxdb/influxdb-data/data"

    wal-dir = "D:/InfluxData/influxdb/influxdb-data/wal"

    max-values-per-tag = 0

  - [http]

    enabled = true

    auth-enabled = false

  `influxdb.conf 파일에는 많은 설정값들이 있지만, 일단 가장 간단한 설정만 하도록 한다.`

<br>

- 시스템 환경 변수 편집으로 들어가서 환경변수를 설정해준다.

  - 시스템 변수에서 새로 만들기를 누른다.

  - 변수 이름은 INFLUXDB_CONFIG_PATH

  - 변수 값에는 influxdb.conf 파일이 있는 곳의 경로를 넣어준다.

    (ex> D:\InfluxData\influxdb\influxdb-data\influxdb.conf)

  - 확인을 눌러 설정을 종료한다.

<br>

- influxd.exe 파일을 실행시킨다.

<br>

- 인터넷 주소창에 localhost:8086/query 를 검색하여
  {"error":"missing required parameter \"q\""}
  가 나오면 설치가 완료된 것이다.

<br>

- nssm 을 이용해서 influxd.exe 를 서비스 등록해준다.

<br>
<br>

**Telegraf**

<br>

- https://dl.influxdata.com/telegraf/releases/telegraf-1.18.0_windows_amd64.zip

  링크를 눌러 설치 한다. 👆

<br>

- 원하는 경로에 압축을 푼다. (ex> D:\telegraf)

<br>

- telegraf.conf 파일로 들어가서 설정값을 변경한다.

```conf
# Configuration for telegraf agent
[agent]
  ## Default data collection interval for all inputs
  interval = "500ms"
  precision = "500ms"

###############################################################################
#                                  OUTPUTS                                    #
###############################################################################

# Configuration for sending metrics to InfluxDB
[[outputs.influxdb]]
  ## The full HTTP or UDP URL for your InfluxDB instance.
  ##
  ## Multiple URLs can be specified for a single cluster, only ONE of the
  ## urls will be written to each interval.
  urls = ["http://localhost:8086"]

## The target database for metrics; will be created as needed.
database = "telegraf"

## HTTP Basic Auth
username = "admin"
password = "its@1234"

###############################################################################
#                                  INPUTS                                     #
###############################################################################

[[inputs.modbus]]
  name = "Modbus-test"
  slave_id = 1
  interval = "500ms"
  timeout = "2s"
  controller = "tcp://192.168.100.31:502"
  holding_registers = [
  { name = "Curr_MAX",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1299]},
  { name = "Curr_IL1",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1300]},
	{ name = "Curr_IL2",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1301]},
	{ name = "Curr_IL3",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1302]},
	{ name = "Volt_AVG",        byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1303]},
	{ name = "Volt_L1L2",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1304]},
	{ name = "Volt_L2L3",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1305]},
	{ name = "Volt_L3L1",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1306]},
  { name = "Temp",            byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1307]},
  { name = "P_Acive",         byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1309]}, # 유효 전력
  { name = "P_Acive_Mhours",  byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1311]}, # 유효 전력 적산
  { name = "Motor_Life_Day",  byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1315]}, # 구동 일수
  { name = "Motor_Life_Hour", byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1316]}, # 구동 시간
  { name = "Motor_count", byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1317]}, #모터 동작 횟수
  { name = "Ground",          byte_order = "AB", data_type = "FLOAT32", scale=0.001, address = [1318]},
  ]

  [[inputs.modbus]]
  name = "Modbus-simulator"
  slave_id = 1
  interval = "500ms"
  timeout = "2s"
  controller = "tcp://192.168.100.75:9600"
  holding_registers = [
  { name = "Curr_MAX",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [900]},
  { name = "Curr_IL1",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [901]},
	{ name = "Curr_IL2",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [902]},
	{ name = "Curr_IL3",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [903]},
	{ name = "Volt_AVG",        byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [904]},
	{ name = "Volt_L1L2",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [905]},
	{ name = "Volt_L2L3",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [906]},
	{ name = "Volt_L3L1",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [907]},
  { name = "Temp",            byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [908]},
  { name = "P_Acive",         byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [910]}, # 유효 전력
  { name = "P_Acive_Mhours",  byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [912]}, # 유효 전력 적산
  { name = "Motor_Life_Day",  byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [916]}, # 구동 일수
  { name = "Motor_Life_Hour", byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [917]}, # 구동 시간
  { name = "Motor_count", byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [918]}, #  모터 동작 횟수
  { name = "Ground",          byte_order = "AB", data_type = "FLOAT32", scale=0.001, address = [919]},
  ]

[[inputs.modbus]]
  name = "Modbus-simulator-51"
  slave_id = 1
  interval = "500ms"
  timeout = "2s"
  controller = "tcp://192.168.100.51:9100"
  holding_registers = [
  { name = "Curr_MAX",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1299]},
  { name = "Curr_IL1",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1300]},
	{ name = "Curr_IL2",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1301]},
	{ name = "Curr_IL3",        byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1302]},
	{ name = "Volt_AVG",        byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1303]},
	{ name = "Volt_L1L2",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1304]},
	{ name = "Volt_L2L3",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1305]},
	{ name = "Volt_L3L1",       byte_order = "AB", data_type = "FLOAT32", scale=1.0,  address = [1306]},
  { name = "Temp",            byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1307]},
  { name = "P_Acive",         byte_order = "AB", data_type = "FLOAT32", scale=0.01, address = [1309]}, # 유효 전력
  { name = "P_Acive_Mhours",  byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1311]}, # 유효 전력 적산
  { name = "Motor_Life_Day",  byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1315]}, # 구동 일수
  { name = "Motor_Life_Hour", byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1316]}, # 구동 시간
  { name = "Motor_count", byte_order = "AB", data_type = "FLOAT32", scale=1.0, address = [1317]}, # 모터 동작 횟수
  { name = "Ground",          byte_order = "AB", data_type = "FLOAT32", scale=0.001, address = [1318]},
  ]



# [[aggregators.minmax]]
#   period = "100ms"          # send & clear the aggregate every 1s.
#   drop_original = false  # drop the original metrics.

# [[aggregators.basicstats]]
#   ## The period on which to flush & clear the aggregator.
#   period = "1s"
#
#   ## If true, the original metric will be dropped by the
#   ## aggregator and will not get sent to the output plugins.
#   drop_original = false
#
#   ## Configures which basic stats to push as fields
#   # stats = ["count","diff","min","max","mean","non_negative_diff","stdev","s2","sum"]

[[processors.date]]
 tag_key = "DateTime"
 date_format = "2006-01-02 15:04:05.00"
 timezone = "Local"
```

`현재 설정값에서 변경 및 추가도 가능하다. 지금은 기본적인 데이터셋이다.`

<br>

- 시스템 환경 변수 편집으로 들어가서 환경변수를 설정해준다.

  - 시스템 변수에서 새로 만들기를 누른다.

  - 변수 이름은 TELEGRAF_CONFIG_PATH

  - 변수 값에는 telegraf.conf 파일이 있는 곳의 경로를 넣어준다.

    (ex> D:\telegraf\telegraf.conf)

  - 확인을 눌러 설정을 종료한다.

<br>

- telegraf 를 서비스 등록한다.

  - telegraf 를 설치한 경로에서 cmd 창을 연다.

  - `telegraf.exe --service install --config "D:\Telegraf\telegraf.conf"` 입력.

  - `D:\Telegraf\telegraf.exe --config D:\Telegraf\telegraf.conf --test`

  - `telegraf.exe --service start`

<br>

- 컴퓨터를 재시작하여 telegraf, influxdb가 모두 작동하는지 확인한다.

<br>
<br>
