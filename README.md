# common_docker
A list of useful docker images to run.


# PROMETHEUS
- Open source telemetry and alerting solution
  https://prometheus.io

- INSTALL
  - Pull down the image:
    ~~~
    docker pull prom/prometheus
    ~~~
- RUN
  - Run prometheus image on local port
    ~~~
    docker run --name prometheus -d -p 9090:9090 prom/prometheus
    ~~~
  - Alternately, it can be configured to run with custom yaml :
    ~~~
    docker run --name prometheus -d -p 9090:9090 -v /our/path/to/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
    ~~~
  - Prometheus will be accessible via http://localhost:9090
- CONFIGURE
  - Configure the prometheus.yml:
    ~~~
    scrape_configs:
    - job_name: 'spring-boot-application'
      metrics_path: '/actuator/prometheus'
      scrape_interval: 15s # This can be adjusted based on our needs
      static_configs:
        - targets: ['localhost:8080']
    ~~~
- CONFIGURE SPRING
  - Configure the application.properties or application.yml
    ~~~
    management.endpoints.web.exposure.include=*
    management.endpoint.health.show.details=always
    ~~~
  - Configure your pom.xml
    ~~~
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    
    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-registry-prometheus</artifactId>
    </dependency>
    ~~~
  
  - Or alternately the build.gradle:
    ~~~
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'io.micrometer:micrometer-registry-prometheus'
    ~~~

# GRAFANA
  Query, Visualize and Alert on data
  https://grafana.com
  https://grafana.com/docs/grafana/latest/setup-grafana/installation/run-grafana-docker-image/
  - INSTALL
  - Pull down the image:
    ~~~
    docker pull grafana/grafana
    ~~~
  - RUN
    - Run prometheus image on local port
    ~~~
    docker run -d --name=grafana -p 3000:3000 grafana/grafana
    ~~~
  - CONFIGURE
    - Configure grafana
      - Open web interface http://localhost:9090
      - Navigate to Configuration > Data Sources > Add data source
      - Add prometheus url: http://localhost:9090
  - CUSTOMIZE in SPRINGBOOT
    Add a metrics service as below
    ~~~
    @Component
    public class CustomMetricsService {
    
        private final Counter customMetricCounter;
    
        public CustomMetricsService(MeterRegistry meterRegistry) {
            customMetricCounter = Counter.builder("custom_metric_name")
              .description("Description of custom metric")
              .tags("environment", "development")
              .register(meterRegistry);
        }
    
        public void incrementCustomMetric() {
            customMetricCounter.increment();
        }
    }
    ~~~
# OPEN API
- TODO
  
# SQLITE
- Configure a docker image with SQLite
- TODO: https://thriveread.com/sqlite-docker-container-and-docker-compose/
