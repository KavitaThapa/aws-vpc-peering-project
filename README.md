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

## Network Architecture and Resource Inventory

The project uses two VPCs with non-overlapping IPv4 CIDR blocks connected through a VPC Peering connection.

### VPCs and Subnets

| VPC | CIDR Block | Subnet | Subnet CIDR | Purpose |
|---|---|---|---|---|
| `ewb-vpc-a` | `10.10.0.0/16` | `ewb-a-public` | `10.10.1.0/24` | Bastion host |
| `ewb-vpc-a` | `10.10.0.0/16` | `ewb-a-private` | `10.10.2.0/24` | Private application instance |
| `ewb-vpc-b` | `10.20.0.0/16` | `ewb-b-private` | `10.20.1.0/24` | Private application instance |

### EC2 Instances

| Instance | Role | Private IPv4 | Public IPv4 |
|---|---|---|---|
| `ewb-bastion` | Bastion host | `10.10.1.174` | Assigned |
| `ewb-app-a` | Private application instance | `10.10.2.10` | None |
| `ewb-app-b` | Private application instance | `10.20.1.10` | None |

### VPC Peering

- **VPC-A:** `10.10.0.0/16`
- **VPC-B:** `10.20.0.0/16`
- **Peering connection ID:** `pcx-0b0bec775ad8c7f44`
- **Status:** Active
- **Region:** `ap-south-1` (Mumbai)

### Routing Configuration

- The public route table for VPC-A routes internet-bound traffic through its Internet Gateway.
- VPC-A has a route to `10.20.0.0/16` through the VPC Peering connection.
- VPC-A's private route table routes `10.20.0.0/16` through the peering connection.
- VPC-B's private route table routes `10.10.0.0/16` through the peering connection.
- Each VPC retains its local route for its own CIDR block.

## Security Groups

Security groups were configured to control SSH administration and ICMP connectivity testing.

| Security Group | Applied To | Inbound Access |
|---|---|---|
| `ewb-sg-bastion` | Bastion host | SSH (TCP 22) from the administrator's public IP `/32` |
| `ewb-sg-app-a` | App-A | SSH (TCP 22) from bastion private IP `10.10.1.174/32`; ICMP Echo Request from `10.20.1.10/32` |
| `ewb-sg-app-b` | App-B | SSH (TCP 22) from bastion private IP `10.10.1.174/32`; ICMP Echo Request from `10.10.2.10/32` |

The ICMP rule on App-B was temporarily removed for the controlled failure test and restored afterward.

## Connectivity Testing and Results

### 1. Forward Connectivity: App-A to App-B

- **Source:** App-A — `10.10.2.10`
- **Destination:** App-B — `10.20.1.10`
- **Command:** `ping -c 4 10.20.1.10`
- **Result:** Successful; 4 packets received and 0% packet loss.

### 2. Reverse Connectivity: App-B to App-A

- **Source:** App-B — `10.20.1.10`
- **Destination:** App-A — `10.10.2.10`
- **Command:** `ping -c 4 10.10.2.10`
- **Result:** Verified successful bidirectional connectivity.

### 3. Controlled Failure Test

The inbound ICMP Echo Request rule on `ewb-sg-app-b` was temporarily removed.

- **Test command:** `ping -c 4 10.20.1.10` from App-A.
- **Observed result:** 4 packets transmitted, 0 received, 100% packet loss.

This confirmed that the destination security-group rule was required for the ICMP test traffic to reach App-B.

### 4. Recovery Test

The ICMP Echo Request rule was restored on App-B.

- **Test command:** `ping -c 4 10.20.1.10` from App-A.
- **Observed result:** 4 packets transmitted, 4 received, 0% packet loss.

### 5. Private Instance Access

SSH access to the private application instances was verified through the bastion host. App-A and App-B had no public IPv4 addresses.

## Key Learnings

- VPC Peering enables private communication between VPCs when routing and security rules permit it.
- Routes must be configured in both directions.
- Security groups control traffic to EC2 instances.
- A bastion host provides an SSH entry point to private instances.
- Controlled failure testing helps isolate security-related connectivity problems.
- Restoring the required ICMP rule returned the ping test to a successful state.
