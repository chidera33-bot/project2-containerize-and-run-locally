
# Project 2: Containerize and Run Locally

## Project Overview

This project demonstrates how to containerize a simple web application using Docker, manage multiple services with Docker Compose, and run an AWS Lambda function locally using AWS SAM CLI.

## Technologies Used

- Docker Desktop and WSL2 Ubuntu
- Node.js 24
- Nginx Alpine
- Redis 7 Alpine
- Docker Compose
- AWS SAM CLI with Node.js 22
- Git

## Project Files

- `Dockerfile` - Multi-stage Docker build
- `Dockerfile.single` - Single-stage Docker build
- `compose.yaml` - Nginx and Redis services
- `index.html` - Simple webpage
- `local-fn/` - AWS SAM Lambda function

## Running the Application

From the project root, run:

```bash
docker compose up -d --build
```

Check the containers:

```bash
docker compose ps
```

Test the website:

```bash
curl -I http://localhost:8080
```

Expected result: `HTTP/1.1 200 OK`

Stop the application:

```bash
docker compose down
```

## Testing Redis

Start the services, then run:

```bash
docker compose exec cache redis-cli ping
```

Expected result: `PONG`

## Running the AWS Lambda Function

```bash
cd local-fn
sam local invoke HelloWorldFunction
```

Expected response:

```json
{"statusCode":200,"body":"{\"message\":\"hello world\"}"}
```

To invoke the function with a sample event:

```bash
sam local invoke HelloWorldFunction -e events/event.json
```

## Multi-Stage Docker Build

The project uses a multi-stage Dockerfile.

The first stage uses Node.js to build the application. The second stage uses Nginx Alpine to serve the finished files.

In local testing, the single-stage image was approximately 1.65 GB, while the multi-stage image was approximately 93.6 MB, reducing image size by approximately 94.3%.

This approach reduces the final image size because the runtime image does not need the complete Node.js build environment.

## Docker Compose and Redis

Docker Compose manages two separate services: Nginx for serving the website and Redis as a cache service.

Redis includes a health check. Docker Compose waits for Redis to become healthy before starting the web service.

During troubleshooting, Redis was stopped while Nginx remained running. The website still returned HTTP 200 OK, demonstrating that this simple static webpage can continue serving without Redis.

## Verification

The application was successfully tested from a fresh Git clone.

The cloned project built using Docker Compose, Redis became healthy, and the website returned HTTP 200 OK.

The AWS SAM Lambda function also returned status code 200 during local testing.


# Project 2: Containerize and Run Locally

## Project Overview

This project demonstrates how to containerize a simple web application using Docker, manage multiple services with Docker Compose, and run an AWS Lambda function locally using AWS SAM CLI.

## Technologies Used

- Docker Desktop and WSL2 Ubuntu
- Node.js 24
- Nginx Alpine
- Redis 7 Alpine
- Docker Compose
- AWS SAM CLI with Node.js 22
- Git

## Project Files

- `Dockerfile` - Multi-stage Docker build
- `Dockerfile.single` - Single-stage Docker build
- `compose.yaml` - Nginx and Redis services
- `index.html` - Simple webpage
- `local-fn/` - AWS SAM Lambda function

## Running the Application

From the project root, run:

```bash
docker compose up -d --build
```

Check the containers:

```bash
docker compose ps
```

Test the website:

```bash
curl -I http://localhost:8080
```

Expected result: `HTTP/1.1 200 OK`

Stop the application:

```bash
docker compose down
```

## Testing Redis

Start the services, then run:

```bash
docker compose exec cache redis-cli ping
```

Expected result: `PONG`

## Running the AWS Lambda Function

```bash
cd local-fn
sam local invoke HelloWorldFunction
```

Expected response:

```json
{"statusCode":200,"body":"{\"message\":\"hello world\"}"}
```

To invoke the function with a sample event:

```bash
sam local invoke HelloWorldFunction -e events/event.json
```

## Multi-Stage Docker Build

The project uses a multi-stage Dockerfile.

The first stage uses Node.js to build the application. The second stage uses Nginx Alpine to serve the finished files.

In local testing, the single-stage image was approximately 1.65 GB, while the multi-stage image was approximately 93.6 MB, reducing image size by approximately 94.3%.

This approach reduces the final image size because the runtime image does not need the complete Node.js build environment.

## Docker Compose and Redis

Docker Compose manages two separate services: Nginx for serving the website and Redis as a cache service.

Redis includes a health check. Docker Compose waits for Redis to become healthy before starting the web service.

During troubleshooting, Redis was stopped while Nginx remained running. The website still returned HTTP 200 OK, demonstrating that this simple static webpage can continue serving without Redis.

## Verification

The application was successfully tested from a fresh Git clone.

The cloned project built using Docker Compose, Redis became healthy, and the website returned HTTP 200 OK.

The AWS SAM Lambda function also returned status code 200 during local testing.
