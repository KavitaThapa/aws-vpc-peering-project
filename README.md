# aws-vpc-peering-project
Hands-on AWS project demonstrating VPC Peering, private EC2 connectivity, route tables, security groups, and connectivity failure recovery. Copy
# AWS VPC Peering Project

## Project Overview

This project demonstrates private network communication between two Amazon Virtual Private Clouds (VPCs) using AWS VPC Peering.

The environment uses Amazon EC2 instances, custom route tables, security groups, and a bastion host to demonstrate private connectivity and secure administrative access.

The project also includes a controlled failure test to investigate how security-group rules affect network communication.

## Project Objectives

- Create two VPCs with separate IPv4 CIDR blocks.
- Establish a VPC Peering connection between the VPCs.
- Configure route tables for communication between private subnets.
- Access private EC2 instances through a bastion host using SSH.
- Configure security groups for SSH and ICMP traffic.
- Test connectivity in both directions.
- Simulate a connectivity failure by removing an ICMP rule and verify recovery after restoring it.

## AWS Services and Concepts

- **Amazon VPC:** Isolated virtual networks
- **VPC Peering:** Private communication between VPCs
- **Amazon EC2:** Bastion host and application instances
- **Route Tables:** Network traffic routing
- **Security Groups:** Instance-level traffic control
- **SSH:** Secure access to private instances
- **ICMP:** Connectivity testing with ping

## Environment

- **Cloud Provider:** Amazon Web Services (AWS)
- **Region:** Asia Pacific (Mumbai) — `ap-south-1`
- **Project Type:** Hands-on cloud networking project

## Project Status

Core network configuration and connectivity tests have been completed. Documentation, evidence organization, and final project presentation are in progress.
