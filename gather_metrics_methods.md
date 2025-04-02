# Gather Metrics Methods

## 1. RED Method (Rate, Errors, Duration)

**Best for**: Request-driven services (APIs, microservices, web apps).

### Metrics

- Rate – Requests per second (throughput).
- Errors – Number of failed requests (HTTP 5xx, 4xx, timeouts).
- Duration – Latency (p50, p90, p99, avg response time).

### Use Case

Monitoring REST APIs, serverless functions, or any service where user requests are critical.

Example:
  - rate: `http_requests_total[5m]` (Prometheus)
  - errors: `sum(rate(http_request_errors_total[5m]))`
  - duration: `histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))`

**Pros**: Simple, aligns with SLOs/SLIs.
**Cons**: Less useful for background jobs or non-request-based systems.

## 2. USE Method (Utilization, Saturation, Errors)

**Best for**: Infrastructure & hardware resources (CPU, memory, disk, network).

### Metrics

- Utilization – % of resource busy (e.g., CPU usage).
- Saturation – Overloaded queues (e.g., disk I/O wait, CPU run queue).
- Errors – Hardware/OS-level failures (e.g., disk errors, NIC drops).

### Use Case

Diagnosing server performance issues (e.g., high CPU, disk latency).

Example:
  - utilization: `node_cpu_seconds_total{mode="user"}[5m]`
  - saturation: `node_load5 (system load avg)`
  - errors: `node_disk_read_errors_total`

**Pros**: Great for low-level system tuning.
**Cons**: Doesn’t cover application-level logic (e.g., business logic errors).

### 3. Four Golden Signals (Google SRE)

**Best for**: End-to-end service health (user-facing systems).

### Metrics

- Latency – Time to serve requests (successful vs. failed).
- Traffic – Demand (e.g., QPS, concurrent sessions).
- Errors – Request failures (explicit + implicit, like stale data).
- Saturation – Resource exhaustion (e.g., memory pressure, queue depth).

### Use Case

Holistic monitoring of user experience (e.g., e-commerce sites).

Example:

- traffic: `sum(rate(http_requests_total[5m]))`
- latency: `histogram_quantile(0.9, rate(http_request_duration_seconds_bucket[5m]))`
- errors: `rate(http_500_errors_total[5m])`
- saturation: `container_memory_working_set_bytes`

**Pros**: Balances infrastructure and UX concerns.
**Cons**: Requires more instrumentation than RED.
