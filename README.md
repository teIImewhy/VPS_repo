# LEMP stack with Monitoring
A Docker Compose based LEMP environment with monitoring and observability components.
## Overview
This project provides a containerized LEMP stack consisting of:
- Nginx
- PHP-FPM
- MariaDB
- Prometheus
- Grafana
- Node Exporter
 
The stack is intended for learning Docker, Docker Compose, monitoring, and CI/CD with GitHub Actions.
## Repository Structure

```text
├── .env
├── docker-compose.yml
├── nginx/
│   └── default.conf
├── php/
│   └── Dockerfile
├── prometheus/
│   └── prometheus.yml
├── .gitignore
└── .github/
       └── workflows/
             └── homelab-ci.yml
```
## Requirements
- Docker Engine
- Docker Compose

## Environment Variables
Create a .env file in the project root directory.

Example:
```env
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=admin

MYSQL_DATABASE=wordpress
MYSQL_USER=wordpress
MYSQL_PASSWORD=password
MYSQL_ROOT_PASSWORD=rootpassword
```
## Notes
The following directories are expected to exist locally and are mounted as Docker volumes:
./wordpress
./mariadb
./prometheus/data

**These directories are not stored in the repository.**

## Running the Stack
Build and start all services:

> `docker compose up -d`
View all available containers:

> `docker compose ps -a`
View container logs:

> `docker compose logs`
Stop the stack:

> `docker compose down`

## Port mapping 
| Service | Internal port | External port | Local access |
| :--- | :---: | :---: | :--- |
| **Nginx** | `80` | `80` | `http://localhost` |
| **MariaDB** | `3306` | `3306` | |
| **Prometheus** | `9090` | `9090` | `http://localhost:9090` |
| **Grafana** | `3000` | `3000` | `http://localhost:3000` |



