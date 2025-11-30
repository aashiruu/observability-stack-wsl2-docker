# observability-stack-wsl2-docker
## Full-Stack Observability on WSL2 Using Docker (Prometheus + Grafana + Loki + Promtail)
This project is a complete observability stack running on WSL2 + Docker Desktop, built to monitor:
System metrics (Node Exporter)
Application & host logs (Promtail + Loki)
Real-time dashboards (Grafana)
Alerting with Gmail SMTP
Prometheus rules + alerts
It runs entirely via Docker Compose, making it lightweight, portable, and extremely easy to deploy on any machine.
| Tool              | Purpose              |
| ----------------- | -------------------- |
| **Prometheus**    | Metrics collection   |
| **Node Exporter** | Linux system metrics |
| **Loki**          | Logs backend         |
| **Promtail**      | Log shipping         |
| **Grafana**       | Dashboards + Alerts  |
| **Gmail SMTP**    | Email notifications  |

## Project Structure

```arduino
observability-stack/
│
├── docker-compose.yml
│
├── prometheus/
│   └── prometheus.yml
│
├── promtail/
│   └── promtail-config.yaml
│
├── loki/
│   └── local-config.yaml
│
└── dashboards/
    └── .json    Grafana dashboard exports
```

## Requirements
1. Docker Desktop (WSL2 integration enabled)
2. Ubuntu-22.04 (WSL2)
3. Ports required:
 :. 3000 → Grafana
 :. 9090 → Prometheus
 :. 9100 → Node Exporter

:3100 → Loki
<img width="1364" height="336" alt="image" src="https://github.com/user-attachments/assets/cfba6dd1-6ae9-4806-8a30-6d8632785915" />
## docker ps output
<img width="1358" height="623" alt="image" src="https://github.com/user-attachments/assets/8eda26fb-163d-4488-ae60-7f509a246b2f" />
## Prometheus Targets Page
<img width="1349" height="610" alt="image" src="https://github.com/user-attachments/assets/9df7ab89-563e-4990-92a6-236432b6e1e9" />
## Node Exporter (ID: 1860
<img width="1345" height="612" alt="image" src="https://github.com/user-attachments/assets/fae1c956-5a55-4d9d-bedb-05e3950ffb0a" />
<img width="1353" height="617" alt="image" src="https://github.com/user-attachments/assets/ff9c7601-b660-443e-abb5-b686abbdb261" />
## Prometheus stats (ID: 3662
<img width="1349" height="604" alt="image" src="https://github.com/user-attachments/assets/b9132938-b4ea-4e6b-9899-796a911ab63e" />
## Loki logs view

## How to Run the Stack
1. Clone the repo
```bash
git clone https://github.com/aashiruu/observability-stack-wsl2-docker
cd observability-stack-wsl2-docker
```
3. Start the stack
```nginx
docker compose up -d
```
Check running containers:
```
docker ps
```
You should see:
i. grafana
ii. prometheus
iii. loki
iv. promtail
v. node-exporter

4. Access services
   
| Service       | URL                                                            |
| ------------- | -------------------------------------------------------------- |
| Grafana       | [http://localhost:3000](http://localhost:3000)                 |
| Prometheus    | [http://localhost:9090](http://localhost:9090)                 |
| Loki          | [http://localhost:3100/metrics](http://localhost:3100/metrics) |
| Node Exporter | [http://localhost:9100/metrics](http://localhost:9100/metrics) |

Default login:
```pgsql
Username: admin
Password: admin
```
## Grafana Email Alerts Configuration

Already included via environment variables:

```ini
GF_SMTP_ENABLED=true
GF_SMTP_HOST=smtp.gmail.com:587
GF_SMTP_USER=your-email@gmail.com
GF_SMTP_PASSWORD=your-app-password
GF_SMTP_FROM_ADDRESS=your-email@gmail.com
GF_SMTP_FROM_NAME=Grafana Alerts
```
Works with Gmail App Passwords (not regular passwords).
## Observability Flow
```csharp
[Node / Docker Logs]
            ↓
         Promtail
            ↓
           Loki
            ↓
        Grafana Logs Panel

[System Metrics] → Node Exporter → Prometheus → Grafana
```

## Alert Rules
Located in:
```bash
prometheus/alert-rules.yml
```
Includes:
1. High CPU Usage Alert
2. High Memory Usage Alert
3. Node Down Alert
4. High Disk Usage Alert
Alerts fire → Prometheus → Grafana → Gmail.

## Sample Prometheus Alerts
```yaml
groups:
  - name: system-alerts
    rules:
      - alert: HighCPUUsage
        expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100) > 80
        for: 1m
        labels:
          severity: critical
        annotations:
          description: "CPU usage is above 80% for over 1 minute."
```

## Dashboards Included
Located under */dashboards:*
1. Node Exporter Full (ID: 1860)
2. Prometheus Stats
3. Loki Log Explorer
4. Container Overview Dashboard
Import them from Grafana → Dashboards → Import.

## Why This Project Is Useful
This repo demonstrates real-world observability engineering:

1. Infrastructure monitoring
2. Logs + Metrics correlation
3. Alerting pipeline
4. Dockerized monitoring stack
5. Working on WSL2 (production-like local env)
6. Zero-config deployment for teams

## Shutdown
```nginx
docker compose down
```
