# High Availability NGINX Reverse Proxy with Load Balancing and TLS

## Executive Summary

This lab demonstrates a production-style reverse proxy and high availability architecture using NGINX on a Linux-based EC2 instance. The environment includes multiple backend services managed with systemd, active load balancing, failover testing, and TLS encryption.

This project models core cloud architecture concepts used in AWS environments behind Application Load Balancers and Target Groups.

---

## Architecture Overview

Client  
→ NGINX (HTTP:9090 / HTTPS:9443)  
→ Backend A (Port 3000)  
→ Backend B (Port 3001)

NGINX acts as:

- Reverse Proxy  
- Load Balancer  
- TLS Termination Point  

Backend services are local Python HTTP servers managed as persistent Linux services.

---

## Backend Service Configuration

Two backend applications were deployed:

- backendA.service → Python HTTP server on port 3000  
- backendB.service → Python HTTP server on port 3001  

Each service:

- Is managed via systemd
- Automatically restarts on failure
- Starts at system boot
- Runs in an isolated application directory

Example validation:


---

## Load Balancing Configuration

NGINX upstream block:
upstream backends {
server 127.0.0.1:3000 max_fails=2 fail_timeout=10s;
server 127.0.0.1:3001 max_fails=2 fail_timeout=10s;
}


Key Features:

- Round-robin load balancing
- Automatic backend failure detection
- Temporary backend removal after threshold failures
- Automatic reintroduction after recovery

Reverse proxy routing:

location / {
proxy_pass http://backends
;
}


---

## Backend Identification

For validation and troubleshooting, a response header was injected:
add_header X-Backend $upstream_addr always;


Verification:
curl -I localhost:9090


This confirms which backend instance processed the request.

---

## Failover Testing

High availability was validated by intentionally stopping a backend:
sudo systemctl stop backendA


Result:

- NGINX detected failure
- Traffic routed exclusively to backendB
- No client disruption occurred

Backend recovery:
sudo systemctl start backendA


Load balancing resumed automatically.

---

## TLS Implementation

A self-signed certificate was generated using OpenSSL:
openssl req -x509 -nodes -days 365 -newkey rsa:2048


NGINX configured to terminate TLS on port 9443.

This simulates real-world HTTPS termination performed by AWS ALB with ACM certificates.

---

## Linux Concepts Demonstrated

- systemd service creation and management
- Directory permissions and ownership management
- Process management
- Port binding and verification
- Reverse proxy configuration
- TLS certificate generation
- Service failover validation
- Log inspection and service debugging

---

## Cloud Architecture Mapping

This lab directly maps to AWS production components:

| Lab Component | AWS Equivalent |
|---------------|----------------|
| NGINX         | Application Load Balancer |
| upstream block| Target Group |
| max_fails     | Health Checks |
| systemd apps  | EC2 instances / containers |
| TLS config    | ACM Certificate + HTTPS Listener |

---

## Professional Relevance

This project demonstrates applied knowledge in:

- High availability architecture
- Fault tolerance design
- Reverse proxy engineering
- Linux systems administration
- Cloud infrastructure modeling
- Production-style validation testing

The design reflects real-world cloud engineering and solutions architecture practices.

---

## Future Enhancements

- Docker containerization of backends
- Health check endpoint implementation
- Infrastructure as Code deployment (CloudFormation / Terraform)
- Deployment to AWS behind an actual ALB
- Auto Scaling integration
