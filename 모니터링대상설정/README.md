1. Spring Boot 설정

- gradle 의존성 추가
```gradle
implementation 'org.springframework.boot:spring-boot-starter-actuator'
runtimeOnly 'io.micrometer:micrometer-registry-prometheus'
```

- application.yml
```yml
management:
  endpoints:
    web:
      exposure:
        include: "prometheus"
```

2. EC2 보안 그룹 설정

3. 프록시 서버 사용
- Nginx에서 외부에서 직접 엔트포인트에 접근하는 것 막기
```
server {
    listen 80;

    location /actuator/prometheus {
        proxy_pass http://localhost:8080/actuator/prometheus;  # Spring Boot 애플리케이션 주소
        allow <Prometheus-Server-IP>;  # Prometheus 서버 IP만 허용
        deny all;  # 그 외 모든 접근 차단
    }
}

```