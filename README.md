# Azure Hub-and-Spoke Network Architecture

## Project Overview
This project demonstrates a real-world Hub-and-Spoke network topology 
on Microsoft Azure — the most widely used enterprise network architecture.

## Architecture
- HUB VNet (10.0.0.0/16) — Central network with Azure Firewall
- Spoke-1 VNet (10.1.0.0/16) — Production environment
- Spoke-2 VNet (10.2.0.0/16) — Development environment

## Services Used
- Azure Virtual Networks
- VNet Peering
- Azure Firewall
- Azure Bastion
- Route Tables (UDR)
- Virtual Machines (Windows Server 2019)

## Key Configurations
- Firewall Private IP: 10.0.4.4
- All spoke traffic routed through central firewall
- Secure VM access via Azure Bastion (no public IPs)

## What I Learned
- Hub-spoke is the standard enterprise Azure network pattern
- Route Tables force traffic through the firewall
- VNet Peering is non-transitive
- Azure Bastion provides secure browser-based RDP
