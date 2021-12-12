[//] <> (AZ-104 | Michael Teske | Pluralsight)

# <ins>***Configure VMs for High Availability and Scalability***</ins>
---

## <ins>*Availability Zones*</ins>

### *High Availability Constructs:*

- Availability Zones
  - Distribute VMs across Azure Regions
    - Good for application deployments
    - 3 zones per regions
  - Standard SKU load balancers are Availability Zone aware
  - Standard SKU PIPs are required
  - Only applicable if they are available in your Azure Region
  - 99.99% SLA when deployed to 2 or more Availability Zones
- Fault Domains
  - Logical group of hardware in an Azure DC
  - VMs in the same fault domain share common power source and physical network switch
  - Essentially a server rack within an Azure Dc
- Update Domains
  - Essentially individual servers
  - Protect against normal maintenance updates
    - Applies updates to hardware
  - VMs created in the same update domain will be restarted together during planned maintenance
  - Only one update domain restarted at a time
- Availability Sets
  - Essential for building reliable cloud solutions
  - Groups VMs to distribute across a single DC
    - Minimize disruptions caused by maintenance or outages
    - Good for VMs
  - Cannot add a VM to an Availability Set post deployment
    - Must be done at time of creation
  - 99.95% SLA when a VM has 2 or more instances deployed in the same Availability Set or in the same Dedicated Host Group
- Scale Sets
  - Group of load balanced VMs
  - Can scale automatically based on demand or schedule
  - 2 or more VMs recommended
  - No additional cost other than the extra instances
  - Can be deployed across multiple update/fault domains

