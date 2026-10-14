# OSAI Flight Recorder

OSAI Flight Recorder is a one-machine, open-source observability lab for a Rust-based AI operations agent. It combines the existing working OSAI runtime with HostScope, OpenTelemetry, self-hosted SigNoz, and the SigNoz MCP server.

## What runs

- **OSAI Rust agent** — Axum UI/API, Linux scanner, reasoning, history, guarded actions.
- **PostgreSQL** — operational metadata and Cognee database.
- **RustFS** — S3-compatible raw evidence storage.
- **Cognee** — local long-term memory pipeline.
- **llama.cpp + Qwen GGUF** — local inference.
- **HostScope** — vendor-neutral Rust host snapshot and OTLP metrics exporter.
- **OpenTelemetry Collector** — host metrics, Docker metrics/logs, journald logs, and local OTLP ingress.
- **SigNoz** — traces, metrics, logs, dashboards, alerts, and MCP analysis.

The architecture and track strategy are documented in `docs-OSAI-SigNoz-All-Tracks-Strategy.md`.

## Fastest installation

Use a fresh **Ubuntu 24.04+** or **AlmaLinux/RHEL 9/10** machine with systemd, at least **12 GB RAM**, **4 CPU cores**, and about **60 GB disk**.

```bash
unzip OSAI-Flight-Recorder-ready-to-run.zip
cd OSAI-Flight-Recorder
chmod +x START-HERE.sh
sudo ./START-HERE.sh up
```

The default installation downloads the Qwen GGUF model. For a telemetry-only smoke test:

```bash
sudo DOWNLOAD_MODEL=0 ./START-HERE.sh up
```

## Service URLs

| Service | URL |
|---|---|
| OSAI UI/API | `http://127.0.0.1:8000` |
| Cognee API | `http://127.0.0.1:8001` |
| llama.cpp/Qwen | `http://127.0.0.1:8088` |
| SigNoz UI | `http://127.0.0.1:8080` |
| SigNoz MCP | `http://127.0.0.1:18000/mcp` |
| RustFS API | `http://127.0.0.1:9000` |
| RustFS console | `http://127.0.0.1:9001` |

The MCP host port is deliberately `18000`: OSAI owns port `8000`.

## Operator commands

```bash
sudo ./START-HERE.sh status
sudo ./START-HERE.sh demo
sudo ./START-HERE.sh evidence
sudo ./START-HERE.sh logs
sudo ./START-HERE.sh token
sudo ./START-HERE.sh down
```

Equivalent aliases are available through `./osai local-*`. The original GCP/OpenTofu commands remain available: `deploy`, `observe`, `demo`, `evidence`, `status`, `tunnel`, and `destroy`.

## Demonstrate an incident

```bash
sudo /opt/osai/OS.rs/scripts/inject-cognee-failure.sh
# Inspect SigNoz, then restore:
sudo /opt/osai/OS.rs/scripts/restore-cognee.sh
```

This stops only the local Cognee container, generates requests and telemetry, and leaves PostgreSQL, RustFS, OSAI, and SigNoz running so the dependency failure is visible.

## SigNoz MCP setup

1. Open SigNoz at `http://127.0.0.1:8080` and create the first account.
2. Create an API key under SigNoz settings.
3. Use the endpoint and client examples in `observability/mcp/README.md`.

## Foundry files

`observability/casting.yaml` is the source configuration. The included `casting.yaml.lock` is a bootstrap placeholder because the fully resolved lock is environment/version generated. `START-HERE.sh up` runs `foundryctl gauge`, `forge`, and `cast`, replacing it with the real resolved lock. Before final submission, run:

```bash
sudo OBS_DIR=/opt/osai/OS.rs/observability \
  /opt/osai/OS.rs/observability/scripts/refresh-foundry-lock.sh
```

Then commit the generated `observability/casting.yaml.lock`.

## Validation status

See `VALIDATION-REPORT.md`. Static syntax, structure, manifest, JSON, YAML, and secret/state checks are included. Full Docker and Rust execution must be performed on the target Linux machine because this packaging environment has neither Docker nor Cargo.

## Security

No populated credentials, Terraform state, provider cache, GGUF model, `.env` files, or Rust build targets are included. Runtime credentials are generated locally and encrypted through systemd credentials. Read `SECURITY.md` before exposing any port beyond loopback.

## Hackathon timing warning

This package was prepared on **July 14, 2026**, before the announced **July 20, 2026** hackathon start. Read `HACKATHON-COMPLIANCE.md` and obtain organizer confirmation before submitting pre-event implementation work.
