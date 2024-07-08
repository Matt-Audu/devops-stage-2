# Backend - FastAPI with PostgreSQL

This directory contains the backend of the application built with FastAPI and a PostgreSQL database.

## Prerequisites

- Python 3.8 or higher
- Poetry (for dependency management)
- PostgreSQL (ensure the database server is running)

### Installing Poetry

To install Poetry, follow these steps:

```sh
curl -sSL https://install.python-poetry.org | python3 -
```

Add Poetry to your PATH (if not automatically added):

## Setup Instructions

1. **Navigate to the backend directory**:
    ```sh
    cd backend
    ```

2. **Install dependencies using Poetry**:
    ```sh
    poetry install
    ```

3. **Set up the database with the necessary tables**:
    ```sh
    poetry run bash ./prestart.sh
    ```

4. **Run the backend server**:
    ```sh
    poetry run uvicorn app.main:app --reload
    ```

5. **Update configuration**:
   Ensure you update the necessary configurations in the `.env` file, particularly the database configuration.


## Dockerization

### Prerequisites

- Install python
- Create a `Dockerfile` in the `backend` directory:

```
# Use the official python base image
FROM python:3.11.3 

# Set the working directory
WORKDIR /app

# Install poetry as a dependency and package manager for python
RUN pip install poetry

# Copy the dependency files to the working directory
COPY pyproject.toml poetry.lock ./

# Configure Poetry to not create virtual environments and install dependencies
RUN poetry config virtualenvs.create false \
    && poetry install --no-interaction --no-ansi

# Copy the entire content of the current directory to the container
COPY . .

# Expose port 8000 to allow external access to the application
EXPOSE 8000

Make the prestart.sh script executable
RUN chmod +x ./prestart.sh

# Set the command to run the application using Uvicorn
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## Configure docker-compose file with postgresdb

The docker compose file should contain configurations for frontend, backend, adminer, postgresdb and proxy manager. Frontend should depend on the backend, backend should depend on the postgresdb to ensure successful deployment. The adminer controls the postgresdb

```
backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: backend
    ports:
      - "8000:8000"
    env_file: ./backend/.env
    environment:
      - DB_HOST=postgresdb
      - DB_USER="app"
      - DB_PASSWORD="changethis123"
      - DB_NAME="app"
      - DB_PORT=5432
    depends_on:
      - postgresdb

  postgresdb:
    image: postgres
    container_name: postgresdb
    environment:
      - POSTGRES_PASSWORD=changethis123
      - POSTGRES_USER=app
      - POSTGRES_DB=app
    volumes:
      - pgdata:/var/lib/postgresql/data 
  
  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

  nginxproxymanamger:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: always
    ports:
      - '80:80'
      - '443:443'
      - '8090:81'
    volumes:
      - ./nginx/data:/data
      - ./nginx/letsencrypt:/etc/letsencrypt
    environment:
      - DISABLE_IPV6=true

volumes:
  pgdata:
  ```
  



