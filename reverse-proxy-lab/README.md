# NGINX Reverse Proxy Lab

## Objective
Configure NGINX as a reverse proxy to forward traffic to a backend Python server.

## Architecture
Client → NGINX (9090) → Python (3000)

## What I Did
1. Installed NGINX on AWS EC2.
2. Started a Python backend on port 3000.
3. Modified nginx.conf to use proxy_pass.
4. Verified using curl.

## Result
NGINX successfully forwards traffic to the backend.
