# vvue-be-monitoring
configure monitoring system for spring boot application

## 모니터링 데이터 수집 도구 
> 모니터링을 통한 수집 및 통합 도구 : DataDog, InfluxDB, New Relic ..

- Prometheus와 Grafana를 선택한 이유 : 오픈소스

## Prometheus와 Grafana
### 1. Prometheus 
- 상태 데이터 수집
1) 프로메테우스 서버 (prometheus-server)
    (1) 노드 익스포터 외 여러 대상에서 공개된 메트릭 수집하는 수집기 
    (2) 수집한 시계열 데이터 저장하는 시계열 데이터베이스
    (3) 저장된 데이터를 질의하거나 수집 대상의 상태를 확인할 수 있는 웹 UI
    => 프로메테우스의 서비스 디스커버리 방법으로 수집 대상을 찾음
        - 컨피그맵에 기록된 내용을 바탕으로 대상을 읽어옴
        - 읽어온 대상에 대한 메트릭을 가져오기 위해 API 서버에 정보를 요청
        - 요청을 통해 알아온 경로로 메트릭 데이터를 수집 
2) 노드 익스포터 (node-exporter)
    노드의 시스템 메트릭 정보를 HTTP로 공개하는 역할
    설치된 노드에서 특정 파일들을 읽고, 프로메테우스 서버가 수집할 수 있는 메트릭 데이터로 변환한 후에 노드 익스포터에서 HTTP서버로 공개함
    공개한 내용을 프로메테우스 서버에서 수집
3) 얼럿매니저 (alertmanager)
    프로메테우스에 alert 규칙을 설정하고, 경보 이벤트가 발생하면 설정된 경보 메시지를 대상에게 전달
    프로메테우스 서버에서 주기적으로 경보를 보낼 대상을 감시해 시스템을 안정적으로 운영  
4) 푸시게이트웨이 (pushgateway)
    배치와 스케줄 작업 시 수행되는 일회성 작업들의 상태를 저장하고 모아서 프로메테우스가 주기적으로 가져갈 수 있도록 공개
    짧은 시간 동안 실행되고 종료되는 배치성 프로그램의 메트릭을 저장하거나 외부망에서 접근할 수 없는 내부 시스템의 메트릭을 프록시 형태로 제공  

### 2. Grafana
- 프로메테우스로 수집한 데이터를 시각화 

## 모니터링 대상(서버1), 모니터링 서버(서버2) 구성
Spring Boot Actuator Prometheus 엔드포인트 : http://<2번 서버 IP>:8080/actuator/prometheus

- Prometheus : `http://localhost:9090/`
- Grafana : `http://localhost:3000/`