# n8n Docker Compose Setup with PostgreSQL

This repository provides a Docker Compose configuration for running [n8n](https://n8n.io/) (a workflow automation tool) with a PostgreSQL database backend.

## Purpose

This setup allows you to self-host n8n easily using Docker, providing a persistent database storage solution with PostgreSQL. It includes basic authentication and is configured for easy deployment and management.

## Prerequisites

*   [Docker](https://docs.docker.com/get-docker/) installed on your system.
*   [Docker Compose](https://docs.docker.com/compose/install/) installed on your system.

## Setup and Installation

1.  **Clone the repository (if you haven't already):**
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2.  **Create a `.env` file:**
    Copy the example environment file:
    ```bash
    cp .env.example .env
    ```

3.  **Configure Environment Variables:**
    Edit the `.env` file and replace the placeholder values with your desired settings. See the [Configuration](#configuration) section below for details.

## Configuration (`.env` file)

The `.env` file contains the necessary configuration variables for the services:

*   `POSTGRES_DB`: The name of the PostgreSQL database for n8n.
*   `POSTGRES_USER`: The username for the PostgreSQL database.
*   `POSTGRES_PASSWORD`: The password for the PostgreSQL database user. **Choose a strong password.**
*   `N8N_BASIC_AUTH_USER`: The username for n8n's basic authentication (login).
*   `N8N_BASIC_AUTH_PASSWORD`: The password for n8n's basic authentication. **Choose a strong password.**
*   `TIMEZONE`: Your local timezone (e.g., `America/Edmonton`). Find your timezone [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

**Example `.env`:**
```dotenv
POSTGRES_DB=n8n_database
POSTGRES_USER=n8n_user
POSTGRES_PASSWORD=supersecretpassword123

N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=anothersecretpassword456

TIMEZONE=America/New_York
```

## Running with Docker Compose

1.  **Start the services:**
    Navigate to the directory containing the `docker-compose.yaml` file and run:
    ```bash
    docker compose up -d
    ```
    This command will build (if necessary) and start the `postgres` and `n8n` services in detached mode (`-d`).

2.  **Access n8n:**
    Once the containers are running, you can access the n8n web interface by navigating to `http://localhost:5678` in your web browser. You will be prompted to log in using the `N8N_BASIC_AUTH_USER` and `N8N_BASIC_AUTH_PASSWORD` you set in the `.env` file.

3.  **Stop the services:**
    To stop the running containers:
    ```bash
    docker compose down
    ```

4.  **Stop and remove volumes (use with caution):**
    To stop the containers and remove the associated data volumes (PostgreSQL data, n8n data):
    ```bash
    docker compose down -v
    ```
    **Warning:** This will delete all your n8n workflows and PostgreSQL data unless you have backups.

## Data Persistence

*   **PostgreSQL Data:** Stored in a Docker volume named `postgres_storage`.
*   **n8n Data:** Stored in a Docker volume named `n8n_storage` (mounted at `/home/node/.n8n` inside the container).
*   **Backup/Import:** The configuration includes an `n8n-import` service and mounts `./n8n/backup` to `/backup` inside the containers, suggesting a potential workflow for backing up and restoring credentials and workflows.
*   **Shared Data:** A local directory `./shared` is mounted to `/data/shared` inside the n8n container, allowing file sharing between the host and the container.

## Usage

After starting the services and logging in, you can begin creating and managing your automation workflows through the n8n web interface. Refer to the [official n8n documentation](https://docs.n8n.io/) for guides and examples.
