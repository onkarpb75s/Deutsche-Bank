# BankOps — Service Reliability & Engineering Analytics

**Portfolio project aligned with a graduate technology engineering role**  
**Author:** Onkar Singh

BankOps is a small, runnable engineering demonstration that analyses service events, calculates operational health metrics, detects simple incident patterns, and exposes the results through a REST API.

It is inspired by the role themes in the supplied Deutsche Bank Graduate Programme description: engineering solutions, production stability and reliability, business-data analysis, infrastructure capabilities, and strategic technology. **This is an independent portfolio project, not a Deutsche Bank system and not based on Deutsche Bank internal data.**

## What it demonstrates

| Role theme | Project evidence |
|---|---|
| Design and deliver engineering solutions | Python package and documented service architecture |
| Production engineering / reliability | Health endpoint, latency/error metrics, incident rules and runbook |
| Analyse business/technology data | Per-service event counts, error rates, average and p95 latency |
| Infrastructure capabilities | Dockerfile and Docker Compose |
| Strategic technology delivery | REST API contract, configurable alert thresholds and CI workflow |
| Quality and safe change | Input validation, automated tests, GitHub Actions |

## Screenshots

The images below are generated from the synthetic sample dataset and project architecture.

![Architecture](screenshots/03-architecture.png)
![Event volume](screenshots/01-event-volume.png)
![Error counts](screenshots/02-error-counts.png)

## Features

- Validate and normalise service event data.
- Calculate event volume, error count, error rate, average latency and p95 latency.
- Flag error spikes and elevated p95 latency using configurable thresholds.
- Serve metrics through FastAPI.
- Generate a JSON report from the command line.
- Run locally or in Docker.
- Run automated tests in GitHub Actions.

## Quick start

Requires Python 3.10+.

```bash
git clone https://github.com/YOUR-USERNAME/bankops-reliability-analytics.git
cd bankops-reliability-analytics

python -m venv .venv
# Windows PowerShell: .venv\\Scripts\\Activate.ps1
# macOS/Linux:
source .venv/bin/activate

pip install -e ".[dev]"
pytest
uvicorn bankops.api:app --reload --app-dir src
```

Open:
- API health: http://127.0.0.1:8000/health
- Service metrics: http://127.0.0.1:8000/metrics/services
- Incident alerts: http://127.0.0.1:8000/incidents
- Interactive API docs: http://127.0.0.1:8000/docs

### Run the CLI

```bash
PYTHONPATH=src python -m bankops.cli --data data/sample_events.csv
```

### Run with Docker

```bash
docker compose up --build
```

## API examples

```http
GET /health
GET /metrics/services
GET /incidents?error_threshold=2&latency_threshold_ms=1000
```

Example response shape:

```json
{
  "services": [
    {
      "service": "payments-api",
      "events": 5,
      "errors": 2,
      "error_rate": 0.4,
      "avg_latency_ms": 990.0,
      "p95_latency_ms": 2010.0
    }
  ]
}
```

Values in this example are illustrative; run the API to see exact sample-data output.

## Repository structure

```text
.
├── .github/workflows/ci.yml
├── data/sample_events.csv
├── docs/architecture.md
├── docs/runbook.md
├── screenshots/
├── src/bankops/
│   ├── analytics.py
│   ├── api.py
│   └── cli.py
├── tests/test_analytics.py
├── Dockerfile
├── compose.yaml
├── pyproject.toml
└── README.md
```

## Engineering considerations and next steps

This is a local demonstration, not a production banking service. A real deployment would require authentication and authorisation, secure secrets management, encryption, audit logging, privacy controls, durable event streaming, distributed tracing, service-level objectives, alert routing, load and resilience testing, and deployment approval controls.

Possible extensions:
1. Add Prometheus metrics and Grafana dashboards.
2. Stream events through Kafka or a managed message broker.
3. Add database persistence and time-windowed aggregation.
4. Add role-based access control and API security.
5. Add anomaly detection and service dependency mapping.

## Data and responsible use

All event records are fictional synthetic data. No customer records, real banking logs, internal systems, or Deutsche Bank confidential information are included. Do not upload private, regulated, or licensed data to a public repository.

## Licence

MIT. See `LICENSE`.
