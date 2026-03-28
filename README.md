Azure Hub-and-Spoke Network Architecture 🌐
Show Image
Show Image
Show Image
📋 Project Overview
This project demonstrates a real-world Hub-and-Spoke network topology on Microsoft Azure — the most widely used enterprise network architecture. All traffic between spoke networks is routed through a centralized Azure Firewall in the hub, providing centralized security and traffic control.

🏗️ Architecture Diagram
                        [Internet]
                            |
                      [Azure Bastion]
                            |
              ┌─────────────────────────────┐
              │         HUB VNet            │
              │       10.0.0.0/16           │
              │                             │
              │  ┌─────────────────────┐    │
              │  │   Azure Firewall    │    │
              │  │     10.0.4.4        │    │
              │  └─────────────────────┘    │
              └──────────┬──────────────────┘
                         │
              ┌──────────┴──────────┐
              │                     │
   ┌──────────▼──────────┐ ┌───────▼─────────────┐
   │   Spoke-1 VNet      │ │   Spoke-2 VNet       │
   │   10.1.0.0/16       │ │   10.2.0.0/16        │
   │   (Production)      │ │   (Development)      │
   │                     │ │                      │
   │   vm-spoke1         │ │   vm-spoke2          │
   │   10.1.1.4          │ │   10.2.0.4           │
   └─────────────────────┘ └──────────────────────┘

☁️ Azure Services Used
ServicePurposeAzure Virtual Network (VNet)Private network isolationVNet PeeringConnecting Hub and Spoke networksAzure FirewallCentralized traffic inspection and controlAzure BastionSecure RDP/SSH access to VMsRoute Tables (UDR)Force traffic through Azure FirewallVirtual MachinesWorkload simulation in each spokeResource GroupLogical grouping of all resources

📁 Resource Configuration
Hub VNet — vnet-hub (10.0.0.0/16)
SubnetAddress RangePurposesnet-shared-services10.0.1.0/24Shared servicesAzureFirewallSubnet10.0.4.0/26Azure FirewallAzureFirewallManagementSubnet10.0.2.0/26Firewall managementGatewaySubnet10.0.3.0/27VPN GatewayAzureBastionSubnet10.0.6.0/26Azure Bastion
Spoke VNets
VNetAddress SpaceEnvironmentvnet-Spoke-110.1.0.0/16Productionvnet-Spoke-210.2.0.0/16Development
VNet Peerings
PeeringDirectionStatushub-to-spoke1Hub → Spoke-1Connected ✅spoke1-to-hubSpoke-1 → HubConnected ✅hub-to-spoke2Hub → Spoke-2Connected ✅spoke2-to-hubSpoke-2 → HubConnected ✅
Route Tables
Route TableSubnetDestinationNext Hoprt-spoke1vnet-Spoke-1/snet-workload10.2.0.0/16Azure Firewall (10.0.4.4)rt-spoke2vnet-Spoke-2/snet-workload10.1.0.0/16Azure Firewall (10.0.4.4)
Firewall Rules
Rule CollectionPriorityActionSourceDestinationallow-spokes100Allow10.1.0.0/16, 10.2.0.0/1610.1.0.0/16, 10.2.0.0/16

🚀 Deployment Steps
Phase 1 — Resource Group & Hub VNet

Created resource group rg-hub-spoke
Deployed Hub VNet with 5 subnets

Phase 2 — Spoke VNets

Created vnet-Spoke-1 for Production workloads
Created vnet-Spoke-2 for Development workloads

Phase 3 — VNet Peering

Configured bidirectional peering between Hub and Spoke-1
Configured bidirectional peering between Hub and Spoke-2
Enabled gateway transit on hub side

Phase 4 — Azure Firewall

Deployed Azure Firewall in AzureFirewallSubnet
Configured with forced tunneling support
Private IP: 10.0.4.4

Phase 5 — Firewall Rules

Created network rule collection allow-spokes
Allowed all traffic between Spoke-1 and Spoke-2

Phase 6 — Route Tables

Created UDR for Spoke-1 pointing to Firewall
Created UDR for Spoke-2 pointing to Firewall
Associated route tables to workload subnets

Phase 7 — Test VMs & Bastion

Deployed Windows Server 2019 VMs in each spoke
Deployed Azure Bastion for secure access
Verified network connectivity via tracert


💡 Key Learnings

Hub-and-Spoke is the standard enterprise network pattern in Azure
Azure Firewall must be in a dedicated /26 subnet named exactly AzureFirewallSubnet
Route Tables (UDR) override Azure's default routing to force traffic through the firewall
VNet Peering is non-transitive — spoke-to-spoke traffic must go through the hub
Azure Bastion provides secure browser-based RDP without exposing public IPs
Forced tunneling requires a separate AzureFirewallManagementSubnet


💰 Estimated Cost
ResourceApprox CostAzure Firewall~$1/hourAzure Bastion (Basic)~$0.19/hourVMs (Windows Server)~$0.08-0.15/hour eachVNets & PeeringNearly free

⚠️ Always delete expensive resources (Firewall, Bastion, VMs) after testing!


📚 References

Azure Hub-Spoke Architecture — Microsoft Docs
Azure Firewall Documentation
Azure Bastion Documentation
User Defined Routes


👤 Author
Satyabrata

🔗 LinkedIn
💻 GitHub


⭐ If you found this helpful, please star this repository!
