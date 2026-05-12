# Analytics Platform

> Open-source product analytics, session recording, feature flags, and A/B testing — all in one platform you can self-host.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## What is this?

This is a self-hostable, open-source product analytics platform inspired by [PostHog](https://posthog.com). It gives you full visibility into how users interact with your product — without sending data to a third party.

Key principles:

- **Privacy first** — your data stays on your infrastructure.
- **No sampling** — capture every event, not just a statistical subset.
- **Developer friendly** — SDKs for web, mobile, and backend; full API access.
- **All-in-one** — analytics, session replay, feature flags, A/B tests, and surveys in a single deployment.

---

## Features

| Feature | Description |
|---|---|
| **Event Analytics** | Track custom events, pageviews, and user actions with filtering, grouping, and funnels. |
| **Session Recording** | Replay real user sessions including clicks, scrolls, and network activity. |
| **Feature Flags** | Roll out features gradually with percentage-based or user-property targeting. |
| **A/B Testing** | Run experiments tied directly to your analytics data. |
| **User Identification** | Stitch anonymous and identified sessions across devices. |
| **Cohorts & Retention** | Analyze groups of users and measure how well you keep them. |
| **Dashboards** | Build shareable dashboards from any combination of insights. |
| **Self-hostable** | Deploy on your own infrastructure using Docker or Kubernetes. |

---

## Quick Start

### Docker Compose (recommended for local development)

```bash
git clone https://github.com/<your-org>/<repo-name>.git
cd <repo-name>
cp .env.example .env   # edit values as needed
docker compose up -d
```

Open `http://localhost:8000` and complete the setup wizard.

### Kubernetes (production)

Helm chart and values reference are available in [`charts/`](charts/).

```bash
helm repo add analytics https://charts.example.com
helm install analytics analytics/analytics -f values.yaml
```

---

## Architecture

```
┌──────────────┐     events      ┌───────────────┐
│  Client SDKs │ ─────────────►  │  Ingestion API │
│  (JS/iOS/    │                 │  (Node / Go)   │
│   Android)   │                 └───────┬───────┘
└──────────────┘                         │
                                         ▼
                                  ┌─────────────┐
                                  │  Message    │
                                  │  Queue      │
                                  │  (Kafka)    │
                                  └──────┬──────┘
                                         │
                            ┌────────────┼────────────┐
                            ▼            ▼             ▼
                     ┌──────────┐ ┌──────────┐ ┌──────────┐
                     │ClickHouse│ │PostgreSQL│ │  Redis   │
                     │(events)  │ │(metadata)│ │(cache/   │
                     └──────────┘ └──────────┘ │flags)    │
                                               └──────────┘
                                         │
                                         ▼
                               ┌──────────────────┐
                               │  Query / API      │
                               │  Layer            │
                               └────────┬─────────┘
                                        │
                                        ▼
                               ┌──────────────────┐
                               │  Frontend (React) │
                               └──────────────────┘
```

---

## SDKs

| Platform | Package | Status |
|---|---|---|
| JavaScript (Browser) | `@analytics/js` | Coming soon |
| Node.js | `@analytics/node` | Coming soon |
| Python | `analytics-python` | Coming soon |
| iOS (Swift) | `AnalyticsSwift` | Coming soon |
| Android (Kotlin) | `analytics-android` | Coming soon |
| Go | `analytics-go` | Coming soon |

---

## Configuration

All configuration is driven by environment variables. Copy `.env.example` to `.env` and adjust:

| Variable | Default | Description |
|---|---|---|
| `DATABASE_URL` | — | PostgreSQL connection string |
| `CLICKHOUSE_URL` | — | ClickHouse connection string |
| `KAFKA_BROKERS` | — | Comma-separated list of Kafka brokers |
| `REDIS_URL` | — | Redis connection string |
| `SECRET_KEY` | — | Random secret used for signing sessions |
| `ALLOWED_HOSTS` | `localhost` | Comma-separated list of allowed hostnames |
| `DISABLE_SIGNUP` | `false` | Set to `true` to prevent new registrations |

---

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

1. Fork the repository.
2. Create a feature branch: `git checkout -b feat/my-feature`.
3. Commit your changes following [Conventional Commits](https://www.conventionalcommits.org/).
4. Open a pull request against `main`.

For larger changes, open an issue first to discuss the approach.

---

## Self-hosting

Detailed deployment guides are in the [`docs/`](docs/) directory:

- [Docker Compose deployment](docs/deploy-docker.md)
- [Kubernetes / Helm deployment](docs/deploy-kubernetes.md)
- [Upgrading](docs/upgrading.md)
- [Backup & restore](docs/backup.md)

---

## License

This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Divyansh Khatri
