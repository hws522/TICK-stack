# Telegraf, InfluxDB, Chronograf, Kapacitor - Related Information

<br>
<br>

### **TICK stack**

---

<br>

**TICK stack ?**

- InfluxData에서 만든 4가지 오픈 소스 component의 통칭
- 모니터링 데이터를 수집하고 저장하고 보여준 다음에 임계치를 정해서 alert으로 보냄.
- https://portal.influxdata.com/downloads/

**Component**

- Telegraf -> 수집

  - golnag로 작성되어 있고 약 100여개의 플러그인을 지원
  - 대표적으로 system status, database, networking, message, app 등을 수집함.
  - https://github.com/influxdata/telegraf

- InfluxDB -> 저장

  - 시계열 DB
  - https://github.com/influxdata/influxdb

- Chronograf -> 시각화

  - 시각화 툴
  - 시각화가 부족한 경우 Grafana로 교체 가능함.
  - https://github.com/influxdata/chronograf

- Kapacitor
  - 원시데이터 처리 엔진
  - HeapChart,OpsGanie, Alerts, Sensu, PagerDuty, Slack 와 통합
  - Realtime 스트리밍 데이터 전송 및 Alert
  - https://github.com/influxdata/kapacitor

<br>
