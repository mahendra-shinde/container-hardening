# Module 6: Monitoring, Logging, and Compliance

## Setting Up Monitoring and Logging for Containers
- Use Docker logging drivers (e.g., `json-file`, `syslog`, `fluentd`).
- Aggregate logs with ELK stack, Prometheus, or Grafana.

## Detecting and Mitigating Runtime Security Threats
- Monitor container activity for anomalies.
- Set up alerts for suspicious behavior.

## Compliance Checks with Docker Bench for Security
- Run: `docker run -it --net host --pid host --cap-add audit_control \
  -v /var/lib:/var/lib -v /var/run/docker.sock:/var/run/docker.sock \
  --label docker_bench_security docker/docker-bench-security`
- Review and remediate findings.
