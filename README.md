# Airflow 3.0 Standalone Deployment

This repository provides a minimal, standalone Apache Airflow 3.0 deployment using Docker Compose, ideal for local development and testing. It includes essential services like the API server, scheduler, and DAG processor, with optional components like the triggerer.

## Features
- **LocalExecutor** for standalone task execution.
- **PostgreSQL** as the metadata database.
- **Health checks** for service reliability.
- **Customizable** via `requirements.txt` and environment variables.
- **Optional services** for advanced features (e.g., deferred tasks).

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Quick Start
1. **Clone the repository**:
   ```bash
   git clone https://github.com/xtructt/airflow-standalone.git
   cd airflow-standalone
   ```
2. **Build the Docker image**:
   ```bash
   docker compose build
   ```
3. **Initialize the database**:
   ```bash
   docker compose up airflow-init
   ```
   - On all operating systems, you need to run database migrations and create the first user account. After initialization is complete, you should see a message like this:
     ```
     airflow-init_1       | Upgrades done
     airflow-init_1       | Admin user airflow created
     airflow-init_1       | 3.0.0
     start_airflow-init_1 exited with code 0
     ```
   - The account created has the login `airflow` and the password `airflow`.
4. **Start all services**:
   ```bash
   docker compose up -d
   ```
5. **Access the Airflow UI**:
   - Visit [http://localhost:8080](http://localhost:8080)
   - Default credentials: `airflow` / `airflow`

## Configuration
- **requirements.txt**: Add Python dependencies for your DAGs.
- **Environment Variables**: Customize via `.env` file (see [Airflow Docs](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#setting-the-right-airflow-user)).
- **Fernet Key**: Set `AIRFLOW__CORE__FERNET_KEY` for production (generate with `python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"`).

## Optional Services
- **Triggerer**: Enable for deferred tasks (e.g., sensors). Disable by commenting out `airflow-triggerer` in `docker-compose.yml`.
- **DAG Processor**: Essential for DAG parsing; do not remove.

## Cleaning Up
To reset the environment:
- The docker-compose environment is a “quick-start” one, not designed for production, and the best way to recover from any problem is to clean it up and restart from scratch.
1. **Stop and remove containers**:
   ```bash
   docker compose down --volumes --remove-orphans
   ```
   - Run this command in the directory containing the `docker-compose.yaml` file.
2. **Delete the project directory**:
   ```bash
   rm -rf airflow-3.0-standalone
   ```
3. **Re-clone and start over**:
   ```bash
   git clone https://github.com/your-username/airflow-3.0-standalone.git
   cd airflow-3.0-standalone
   ```

## Troubleshooting
- **DAGs not loading**: Ensure `airflow-dag-processor` is running (`docker ps`).
- **Database issues**: Check PostgreSQL logs (`docker logs postgres-1`).
- **Health checks**: Verify service health with `curl http://localhost:8080/health`.

## Contributing
Contributions are welcome! Please open an issue or submit a pull request.

## License
This project is licensed under the Apache License 2.0.