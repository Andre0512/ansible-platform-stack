# Caddy + Prometheus Monitoring Stack

Ansible playbook for deploying a web server with monitoring and alerting.

**Demo:** https://demo.basche.me

## Usage
```bash
ansible-playbook -i inventory.yml playbook.yml
ansible-playbook -i inventory.yml nginx.yml
```

## Components

- **Caddy** - Web server with automatic HTTPS, reverse proxy, metrics on `:2019/metrics`
- **Nginx** - Backend server (internal network)
- **Prometheus** - Scrapes Caddy metrics, alert rules configured
- **Alertmanager** - Alert routing (webhook receiver)

## Alerts

- `CaddyServerDown` - Server unreachable for 30s
- `HighErrorRate` - HTTP 5xx errors
- `SlowResponses` - Latency > 500ms

## Files
```
├── playbook.yml
├── nginx.yml
├── inventory.yml
└── templates/
    ├── Caddyfile.j2
    ├── prometheus.yml.j2
    ├── alerts.yml.j2
    └── alertmanager.yml.j2
```

## Verification
```bash
# Web server
curl -I https://demo.basche.me

# Nginx via reverse proxy
curl https://demo.basche.me/nginx

# Metrics
curl http://localhost:2019/metrics

# Prometheus targets/alerts
http://localhost:9090/targets
http://localhost:9090/alerts
```
