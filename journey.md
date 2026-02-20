Project Overview

- This project demonstrates the deployment of a containerized background task processing system on AWS.
  The system includes:

- FastAPI (Backend API)

- Celery (Background Worker)

- Redis (Message Broker)

- RDS (Database)

- Nginx (Frontend Hosting)

- Docker & Docker Compose

- AWS EC2 Deployment

- CloudWatch Monitoring

Phase 1: Application Development
Backend (FastAPI)

Implemented the following endpoints:

1. POST /notify/

- Accepts email input

- Triggers a Celery background task

- Returns a task_id

2. GET /task_status/{task_id}

- Polls task status

Returns:

- PENDING

- STARTED

- SUCCESS

- FAILURE

Displays task result when completed

3. /health

- Used for service health checking

- Later used for monitoring validation

- Celery Worker

- Configured Celery with Redis as broker

- Worker listens for background tasks

- Simulated async processing

- Returns result after execution

Redis

- Used as message broker

- Runs as Docker container

- Handles communication between backend and worker


Phase 2: Containerization

Created:

- Dockerfile for backend

- Dockerfile for worker

- docker-compose.yml

- Services defined:

- backend

- worker

- redis

- nginx

Docker Networking

- Created a custom Docker network so that:

- Backend communicates with Redis

- Worker communicates with Redis

Phase 3: Frontend Development

- Created a single-page frontend (frontend/index.html) with:

- Email input field

- Calls POST /notify/

- Receives task_id

- Polls GET /task_status/{task_id}

Displays:

- Task progress

- Status updates

- Final result

- Error handling

- Progress bar

Ensured proper API base URL configuration for EC2 deployment.



Phase 4: Cloud Infrastructure & Deployment
Infrastructure as Code (Terraform)

- The cloud infrastructure was provisioned using Terraform to ensure reproducibility and automation.

- The following resources were created using Terraform:

1. Amazon EC2

- Ubuntu EC2 instance provisioned via Terraform

- Used as the compute host for running application containers

- For simplicity and time constraints in this assignment, environment variables were created manually on the EC2 instance.
  In a production-grade setup, this would be automated using AWS Systems Manager Parameter Store or AWS Secrets Manager to avoid manual configuration and improve security.

2. Amazon RDS

- Managed RDS database instance provisioned via Terraform

- Backend configured to connect to RDS using environment variables

3. Amazon ECR (Elastic Container Registry)

- ECR repositories created Manually

Separate repositories for:

- Backend image

- Worker image

Terraform ensured consistent and repeatable infrastructure provisioning.

CI/CD Pipeline (GitHub Actions)

- A GitHub Actions workflow was implemented to automate deployment.

Pipeline steps:

- Build Docker images (backend & worker)

- Authenticate with AWS

- Push images to Amazon ECR

- SSH into EC2 instance

- Pull latest images from ECR

- Restart containers

This eliminated manual deployment steps and enabled continuous delivery.

Local Development Environment

For local testing:

- Docker Compose was used to run:

- Backend

- Worker

- Redis

- Nginx

Docker Compose was used only for development and validation purposes.

Production Setup

In the cloud environment:

- Containers were pulled from Amazon ECR

- All services run inside a single EC2 instance

- Redis runs as a container on EC2

- RDS runs as a managed AWS database service

- Frontend is served using Nginx container

Deployment Flow Summary

Developer pushes code →
GitHub Actions builds Docker images →
Images pushed to Amazon ECR →
EC2 pulls latest images →
Containers restarted automatically →
Application updated in production.


Issues Faced & Solutions

- 405 Method Not Allowed

Problem:

- Health endpoint accepted POST only

- Monitoring tool sent GET request

Solution:

- Updated monitoring configuration

Understood HTTP method handling

Failed to Fetch (Frontend Error)

Problem:

- Frontend used localhost

- When deployed on EC2, needed public IP

Solution:

- Updated API base to EC2 public IP

Docker Networking Issue

Problem:

Containers couldn't communicate

Redis hostname mismatch

Solution:

- created custum docker network (appnet)

- Ensured same Docker network


Phase 5: Monitoring & Observability

- EC2 Monitoring

  Used CloudWatch → Metrics → EC2

Monitored:

  CPU Utilization

  Network In / Out

  Status checks

Since all containers run on the same EC2 instance, EC2 metrics cover:

  Backend

  Worker

  Redis

  Nginx

-  RDS Monitoring

   Used RDS Monitoring tab.

Observed:

   CPU utilization

   Freeable memory

   Database connections

IOPS

No manual setup required (managed service).

- CloudWatch Logs

   Created log groups:

   backend-logs

   worker-logs

Monitored application logs via:
CloudWatch → Logs → Log groups → Log streams


Final Architecture Overview

User
↓
EC2 Instance
├── Nginx (Frontend)
├── FastAPI Backend
├── Redis
├── Celery Worker
↓
RDS Database

Monitoring:

EC2 → Infrastructure metrics

RDS → Managed metrics

CloudWatch Logs → Application logs


- Project Completion Status

   All required components implemented:

   Containerized application

   Background task processing

   Frontend polling system

   AWS deployment

   Monitoring setup

   Documentation

This project demonstrates a complete cloud-based, containerized background processing system with monitoring and observability.





Used technologies

- FastAPI
- Celery
- Redis
- Amazon RDS
- Amazon EC2
- Amazon ECR
- Docker
- Docker Compose
- Nginx
- Terraform
- GitHub Actions
- Amazon CloudWatch
