# Docker & Container Security Assessment

## Overview

This project demonstrates containerization, multi-container orchestration, vulnerability scanning, container image publishing, and image signature verification using Docker, Docker Compose, Trivy, and Cosign.

## Technology Stack

- Docker and Docker Compose
- Python backend
- Nginx frontend
- PostgreSQL database
- Trivy vulnerability scanner
- Cosign image signing and verification
- Docker Hub container registry

## Application Architecture

The application consists of three services:

- **Frontend:** Nginx, accessible at `http://localhost:8080`
- **Backend:** Python API
- **Database:** PostgreSQL with persistent storage

Docker Compose connects the services through internal networks and uses a health check to verify database readiness.

## Run the Application

1. Clone this repository.
2. Create a `.env` file using `.env.example` as a reference.
3. Set a strong PostgreSQL password in `.env`.
4. Start the services:

```bash
docker compose up -d
```

5. Open `http://localhost:8080` in your browser.
6. Check backend health:

```bash
curl http://localhost:8080/api/health
```

Stop the services with:

```bash
docker compose down
```

To remove the persistent database volume as well, use `docker compose down -v`. This permanently deletes the stored database data.

## Container Image

Published backend images:

- `suryansh12d/fest-backend:1.0.0`
- `suryansh12d/fest-backend:1.0`
- `suryansh12d/fest-backend:1.0.1`

Docker Hub repository: https://hub.docker.com/r/suryansh12d/fest-backend

The `1.0.0` release was signed using Cosign. The `1.0.1` tag was published for the unsigned-image verification test.

## Security Scanning

Trivy reports are stored in `reports/`:

- `trivy-before.txt` — baseline scan
- `trivy-after.txt` — subsequent scan
- `trivy-backend.txt` and `trivy-backend.json` — backend image scan output

Review these reports to compare findings and identify remaining vulnerabilities. A successful scan does not necessarily mean that an image is free of vulnerabilities.

## Image Signing and Verification

The Cosign public key is stored in `cosign.pub`.

Verify the signed release:

```bash
cosign verify --key cosign.pub suryansh12d/fest-backend:1.0.0
```

The `evidence/` directory contains saved verification output, including the unsigned-image test.

The `cosign.key` private key must remain private and must never be committed to this repository.

## Evidence and Screenshots

- `evidence/` — Cosign verification outputs
- `reports/` — Trivy scan reports
- `screenshots/` — assessment screenshots

## Repository Structure

```text
.
├── backend/
├── frontend/
├── compose.yaml
├── .env.example
├── .gitignore
├── cosign.pub
├── evidence/
├── reports/
├── screenshots/
└── README.md
```

## Security Notes

- Keep `.env` and private signing keys out of version control.
- Use strong credentials for the database.
- Review vulnerability reports before deploying images.
- Verify image signatures before trusting signed releases.

## Author

Suryansh Dwivedi