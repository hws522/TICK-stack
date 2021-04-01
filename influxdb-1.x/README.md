# InfluxDB 1.x ver - EOCR Monitoring Setting

<br>
<br>

### **설치**

---

<br>

windows 기준으로 Telegraf, InfluxDB, Chronograf 설치방법을 정리한다.

<br>

**InfluxDB**

<br>

https://dl.influxdata.com/influxdb/releases/influxdb-1.8.4_windows_amd64.zip

링크를 눌러 설치 한다. 👆

<br>

원하는 경로에 압축을 푼다. (ex> D:\InfluxData\influxdb\influxdb-data)

<br>

influxdb.conf 파일로 들어가서 설정된 경로를 바꾼다. 이때 경로는 처음 InfluxDB를 설치할 때 설정해줬던 경로이다.

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

시스템 환경 변수 편집으로 들어가서 환경변수를 설정해준다.

- 시스템 변수에서 새로 만들기를 누른다.

- 변수 이름은 INFLUXDB_CONFIG_PATH

- 변수 값에는 influxdb.conf 파일이 있는 곳의 경로를 넣어준다.

  (ex> D:\InfluxData\influxdb\influxdb-data\influxdb.conf)

- 확인을 눌러 설정을 종료한다.

<br>

influxd.exe 파일을 실행시킨다.

<br>

인터넷 주소창에 localhost:8086/query 를 검색하여
{"error":"missing required parameter \"q\""}
가 나오면 설치가 완료된 것이다.

<br>

nssm 을 이용해서 influxd.exe 를 서비스 등록해준다.

<br>
