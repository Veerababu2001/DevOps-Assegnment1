Application Setup Guide (Local Development)

This document provides step-by-step instructions to run the containerized application locally for development and testing.

Prerequisites

Make sure the following tools are installed on your system:

  Docker

  Docker Compose

  Git

- Check Installations 

  docker --version
  docker compose version
  git --version

- Clone the Repository

  git clone https://github.com/Veerababu2001/DevOps-Assegnment1.git
  
  After clone enter cmd - cd DevOps-Assegnment1

- Environment Variables Setup
  
 Created .env file in backend directory

Ex: .env file 

POSTGRES_HOST=postgres
POSTGRES_PORT=5432
POSTGRES_USER=admin
POSTGRES_PASSWORD=password
POSTGRES_DB=mydb

REDIS_URL=redis://redis:6379/0
CELERY_LOG_FILE=log_celery.txt


- Build Docker Images

  docker compose build

- Start the Full Stack

  docker compose up -d

This will start:

Backend API

Worker (background task processor)

Redis

PostgreSQL (local DB)

Nginx 


Access the Application

Service				URL

Frontend (via Nginx)		http://localhost

Backend API			http://localhost:8000

Health Check			http://localhost:8000/health

Background Task APIs

- Trigger Task
   POST /notify/
- Check Task Status
   GET /task_status/{task_id}

The frontend (frontend/index.html) automatically:

Calls /notify/

Polls /task_status/{task_id}

Displays task result live

View logs 

- View all services logs
   docker compose logs -f

- View Specific Service Logs
   docker compose logs -f backend
   docker compose logs -f worker
   docker compose logs -f nginx

- Stop the Application 
  
  docker compose down

Ports Explanation
Service		Port		Description
Backend		8000		API service
PostgreSQL	5432		Database
Redis		6379		Message broker
Nginx		80		for static of UI

Rebuild After Code Changes

- docker compose build backend (if backend code chnge)
- docker compose up -d
