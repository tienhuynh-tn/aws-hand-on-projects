# AWS ECS EC2 Multi-AZ Architecture

## Overview

This project demonstrates a highly available containerized application deployed on Amazon ECS using the EC2 launch type.

The architecture follows AWS best practices by distributing workloads across multiple Availability Zones, using an Application Load Balancer for traffic distribution, Amazon ECR for container image management, and private subnets for application workloads.

The solution is designed to provide scalability, fault tolerance, and secure access while keeping container workloads isolated from the public internet.

---

## Architecture Diagram

![Architecture Diagram](./AWS-hand-on-project-project-2.png)

---

## Architecture Highlights

- Amazon ECS (EC2 Launch Type)
- Amazon ECR
- Application Load Balancer (ALB)
- Route 53
- Multi-AZ Deployment
- Public and Private Subnets
- NAT Gateway
- Bastion Host
- ECS Service
- ECS Task Definition
- High Availability Design

---

## Architecture Components

### Amazon Route 53

Provides DNS resolution and routes user traffic to the Application Load Balancer.

### Application Load Balancer (ALB)

Receives incoming requests and distributes traffic across ECS tasks running in multiple Availability Zones.

Features:

- Layer 7 Load Balancing
- Health Checks
- High Availability
- Traffic Distribution

### Amazon Elastic Container Registry (ECR)

Stores Docker container images used by ECS tasks.

Benefits:

- Private container registry
- Versioned images
- Secure image storage
- Native ECS integration

### Amazon ECS

Amazon Elastic Container Service orchestrates container deployment and management.

Responsibilities:

- Container scheduling
- Service discovery
- Task monitoring
- Container lifecycle management

### ECS Service

Ensures the desired number of tasks remain running.

Features:

- Self-healing
- Automatic task replacement
- Integration with ALB
- High availability

### ECS Task Definition

Defines how containers run, including:

- Docker image
- CPU allocation
- Memory allocation
- Environment variables
- Port mappings

### Amazon EC2 Container Instances

Host ECS tasks using the ECS EC2 launch type.

Benefits:

- Infrastructure control
- Custom AMIs
- Flexible resource allocation
- Cost optimization

### Bastion Host

Provides secure SSH access to resources within private subnets.

### NAT Gateway

Allows private workloads to access AWS services and external repositories without exposing them to the internet.

---

## Network Design

### VPC

Provides network isolation for all resources.

### Public Subnets

Contain:

- Application Load Balancer
- Bastion Host
- NAT Gateway

### Private Subnets

Contain:

- ECS Container Instances
- ECS Tasks

Application containers are not directly accessible from the internet.

---

## Traffic Flow

### User Request Flow

```text
User
 ↓
Browser
 ↓
Route 53
 ↓
Application Load Balancer
 ↓
ECS Service
 ↓
ECS Tasks
```

### Container Deployment Flow

```text
Developer
 ↓
Docker Build
 ↓
Amazon ECR
 ↓
ECS Cluster
 ↓
ECS Tasks
```

### Administrative Access Flow

```text
AWS User
 ↓
SSH
 ↓
Bastion Host
 ↓
Private ECS Instances
```

---

## High Availability Design

### Multi-AZ Deployment

```text
Availability Zone 1
 ├── Public Subnet
 │   ├── Bastion Host
 │   └── NAT Gateway
 └── Private Subnet
     └── ECS Tasks

Availability Zone 2
 ├── Public Subnet
 └── Private Subnet
     └── ECS Tasks
```

Benefits:

- Fault tolerance
- Reduced downtime
- Increased availability

### ECS Service Recovery

```text
Task Failure
 ↓
ECS Detects Failure
 ↓
Launch Replacement Task
```

### Availability Zone Failure

```text
AZ Failure
 ↓
ALB Routes Traffic
 ↓
Healthy Tasks In Remaining AZ
```

Application remains available.

---

## Security Design

### Network Isolation

- Application workloads run in private subnets
- No direct public access to ECS tasks

### Security Groups

Restrict communication between:

- ALB and ECS Tasks
- Bastion Host and EC2 Instances

### Bastion Access

```text
Admin
 ↓
SSH
 ↓
Bastion Host
 ↓
Private Resources
```

Only administrators can access internal resources.

---

## Scalability Design

### Horizontal Scaling

```text
Increased Traffic
 ↓
ECS Service Scaling
 ↓
Additional Tasks
```

### Load Balancing

Application Load Balancer distributes requests across healthy ECS tasks.

Benefits:

- Improved performance
- High availability
- Better user experience

---

## AWS Services Used

| Category | Services |
|-----------|-----------|
| DNS | Amazon Route 53 |
| Container Orchestration | Amazon ECS |
| Container Registry | Amazon ECR |
| Compute | Amazon EC2 |
| Load Balancing | Application Load Balancer |
| Networking | Amazon VPC, NAT Gateway |
| Security | Security Groups |
| Administration | Bastion Host |

---

## Skills Demonstrated

- Amazon ECS
- Amazon ECR
- Docker
- Container Orchestration
- Application Load Balancer
- Route 53
- VPC Design
- Public and Private Subnets
- NAT Gateway
- Bastion Host
- Multi-AZ Architecture
- High Availability
- Cloud Networking
- Infrastructure Design

---

## Challenges & Design Decisions

### Why ECS EC2 Instead of Fargate?

- More control over infrastructure
- Better understanding of container orchestration
- Lower cost for long-running workloads

### Why Private Subnets for ECS Tasks?

- Improved security
- Reduced attack surface
- No direct internet exposure

### Why Application Load Balancer?

- Layer 7 routing support
- Native ECS integration
- Automatic health checks

### Why Multi-AZ Deployment?

- Protect against Availability Zone failures
- Improve application availability
- Increase resilience

---

## Future Improvements

- ECS Service Auto Scaling
- AWS CloudWatch Monitoring
- AWS Systems Manager Session Manager
- Amazon RDS PostgreSQL
- Amazon ElastiCache Redis
- CI/CD Pipeline with GitHub Actions
- AWS WAF
- Blue/Green Deployment with AWS CodeDeploy

---

## Reference
- [How to Run a Application on AWS ECS: A Step-by-Step Guide](https://blog.cloudmentor.pro/posts/ecs)

---

## Conclusion

This project demonstrates a production-style containerized application architecture using Amazon ECS, Amazon ECR, and Application Load Balancer. The solution provides high availability, scalability, security, and efficient container orchestration while following AWS Well-Architected Framework principles.
