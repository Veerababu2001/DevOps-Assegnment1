# DevOps-Assengment
Containerized Background Task Processing System

Project Overview

This project demonstrates a cloud-deployed, containerized background task processing system using:

- FastAPI – Backend API

- Celery – Background task processing

- Redis – Message broker

- RDS (AWS) – Database

- Nginx – Frontend hosting

- Docker & Docker Compose – Containerization

- AWS EC2 – Deployment

- CloudWatch – Monitoring

The system allows users to trigger background tasks and monitor their status in real time.

Architecture Overview:

User
↓
EC2 Instance
├── Nginx (Frontend)
├── FastAPI Backend
├── Redis
├── Celery Worker
├── Flower Dashboard
↓
RDS Database

All services (except RDS) run inside Docker containers on a single EC2 instance.

Application Flow

- User enters email in frontend.

- Frontend calls POST /notify/ API.

- Backend triggers Celery background task.

- Task is queued in Redis.

- Worker processes the task.

- Frontend polls GET /task_status/{task_id}.

- Final result is displayed to the user.


Project Structure:

.
├── backend/
├── worker/
├── frontend/
│   └── index.html
├── docker-compose.yml
├── journey.md
├── instructions.md
└── README.md

Deployed Services
Service			Port			Description
Backend API		8000			FastAPI service
Health Endpoint		8501			Health check
Nginx			80			Frontend hosting

Monitoring & Observability

Implemented monitoring using:

- CloudWatch EC2 Metrics

- CPU Utilization

- Network In/Out

- RDS Monitoring

- CPU

- Memory

- Connections

- CloudWatch Logs

- Backend logs

- Worker logs

Key Features

- Fully containerized architecture

- Background task processing with Celery

- Real-time task status polling

- Docker-based deployment

- Cloud-based monitoring setup

- Secure EC2 deployment with proper networking

Documentation

- See journey.md for complete development and deployment journey.

- See instructions.md for local container setup instructions.

Project Status

All required deliverables implemented:

- Containerized application

- Background task execution

- Single-page frontend with polling

- AWS deployment

- Monitoring setup

- Documentation completed


Architecture Diagram:

User
  ↓
EC2 Instance
  ↓
Nginx (Frontend)
  ↓
FastAPI Backend
  ↓
Redis (Message Broker)
  ↓
Celery Worker
  ↓
RDS Database



- Architecture Explanation

   User interacts with the frontend.

   Nginx serves the frontend and forwards API requests.

   Backend (FastAPI) receives API calls.

   Redis acts as the message broker.

   Celery Worker processes background tasks.

   RDS stores persistent data.

   All application containers run inside a single EC2 instance.

   Monitoring is handled via CloudWatch
