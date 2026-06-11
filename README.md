# LEMP stack with Monitoring
A Docker Compose based LEMP environment with monitoring and observability components.

## Table of Contents
- [Overview](#overview)
- [Repository Structure](repository-structure)
- [Requirements](#requirements)
- [Environment Variables](#environment-variables)
- [Notes](#notes)
- [WordPress Files](#wordpress-files)
- [Running the Stack](#running-the-stack)
- [Port Mapping](#port-mapping)
- [CI Pipeline](#ci-pipeline)
- [Future Improvements](#future-improvements)

## Overview
This project provides a containerized LEMP stack consisting of:
- Nginx
- PHP-FPM
- MariaDB
- Prometheus
- Grafana
- Node Exporter
 
Project was created as a personal homelab environment for learning Docker, Docker Compose, monitoring (basics), and CI/CD with GitHub Actions.
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
The following directories are expected to exist locally and are mounted as Docker volumes: \
- `./wordpress`
- `./mariadb`
- `./prometheus/data`

**These directories are not stored in the repository.**

## WordPress Files

This repository does not contain WordPress source files.
Before starting the stack, download and extract the latest WordPress release into the `./wordpress` directory.
The resulting structure should contain files such as:
```text
wordpress/
   ├── wp-admin/
   ├── wp-content/
   ├── wp-includes/
   ├── index.php
   └── wp-config-sample.php
```
The ./wordpress directory is mounted into both the Nginx and PHP containers.\
After extracting WordPress files, create a `wp-config.php` file.

You can use `wp-config-sample.php` as a template:

```bash
cp ./wordpress/wp-config-sample.php ./wordpress/wp-config.php
```
Then you should update the database settings to match the values defined in .env:
```text
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpress' );
define( 'DB_PASSWORD', 'password' );
define( 'DB_HOST', 'db' );
```
**DB_HOST must be set to db, which is the MariaDB service name defined in docker-compose.yml.**

## Running the Stack

Build and start all services:
```bash
docker compose up -d
```
View all available containers:
```bash
docker compose ps -a
```
View container logs:
```bash
docker compose logs
```
Stop the stack:
```bash
docker compose down
```

## Port mapping 
| Service | Internal port | External port | Local access |
| :--- | :---: | :---: | :--- |
| **Nginx** | `80` | `80` | `http://localhost` |
| **MariaDB** | `3306` |  | |
| **Prometheus** | `9090` | `9090` | `http://localhost:9090` |
| **Grafana** | `3000` | `3000` | `http://localhost:3000` |
| **Node Exporter** | `9100` | `9100` | `http://localhost:9100` |

## CI Pipeline
GitHub Actions workflow contains two jobs:
### Validate
Checks Docker Compose configuration:
```bash
docker compose config
```
### Build
Builds the custom PHP image:
```bash
docker compose build
```
## Future Improvements
- [ ] Add Docker healthchecks
- [ ] Add cAdvisor metrics collection
- [ ] Configure Grafana dashboards provisioning
- [ ] Add automated MariaDB backups
- [ ] Add integration tests to GitHub Actions
- [ ] Deploy stack on a VPS
- [ ] Implement HTTPS with Let's Encrypt
- [ ] Add Terraform infrastructure provisioning
- [ ] Add Ansible configuration management
- [ ] Migrate services to Kubernetes
