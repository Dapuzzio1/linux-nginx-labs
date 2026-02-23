Linux & NGINX Labs

A curated collection of hands-on labs demonstrating practical Linux administration, NGINX configuration, and real-world web infrastructure concepts.

Purpose

This repository documents structured lab exercises designed to strengthen:

Linux system fundamentals

Networking and port management

NGINX configuration and reverse proxy design

Cloud-based deployment (AWS EC2)

Troubleshooting and verification using CLI tools

Labs Included
1. Reverse Proxy Lab

Configured NGINX to act as a reverse proxy forwarding traffic from port 9090 to a backend Python service running on port 3000.

Key concepts covered:

nginx.conf modification

proxy_pass configuration

Localhost vs public IP understanding

Security group considerations

Process management in Linux

curl, ss, and service validation

Architecture:
Client → NGINX (9090) → Backend Service (3000)

Environment

AWS EC2 (Free Tier)

Amazon Linux

NGINX

Python 3

Git & GitHub for version control

Objective

Build practical infrastructure skills through incremental, documented lab exercises.
