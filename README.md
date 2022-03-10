# DOCKER---PROMETHEUS-AND-GRAFANA-WITH-DOCKER-COMPOSE

Docker Compose is used for running multiple containers as one service. If you have an application/ stack requiring different services, docker compose enables creating one file which would start all containers as one service and avoid the need to start them separately. It is also possible to start one service at a time with docker compose

To be able to use Docker Compose, you should have both Docker and Docker compose installed on your system.

Prometheus is a time series database, making use of exporters to monitor various servers/services while Grafana is one of the most popular monitoring tools. When combined with Prometheus, Grafana offers powerful visualization tool for time series data.

To run Prometheus and Grafana with docker compose, we need to create a docker compose file defining the individual services (Prometheus and Grafana), the images to be used, ports running on and any other thing necessary.

```
version: '3.3'

networks:
  monitoring:
    driver: bridge

volumes:
  prometheus_data: {}


services:
  prometheus:
    command:  
    - --config.file=/etc/prometheus/prometheus.yml
    - --storage.tsdb.path=/data/prometheus
    
    container_name: prometheus
    #environment:
    # LISTEN_ADDRESS: 127.0.0.1
    
    
    ports:
      - "9090:9090"        

    image: prom/prometheus:latest
    user: root
    restart: unless-stopped
    networks: 
      - monitoring
    volumes:
      - /etc/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/data/prometheus        



  nodeexporter:
    image: prom/node-exporter:latest
    container_name: nodeexporter
    restart: unless-stopped
    ports:
      - "9100:9100"

    networks:
      - monitoring    



  grafana:
    image: grafana/grafana:8.3.3
    container_name: grafana 
    environment:
      - GF_SECURITY_ADMIN_USER=${ADMIN_USER:-admin}
      - GF_SECURITY_ADMIN_PASSWORD=${ADMIN_PASSWORD:-admin}
      - GF_USERS_ALLOW_SIGN_UP=false

    restart: unless-stopped
    ports:
      - "3000:3000" 
      
 ```
 
 
     
