# CloudStack
## What is Apache CloudStack?
Apache CloudStack is an open source Infrastructure-as-a-Service platform that manages and orchestrates pools of storage, network, and computer resources to build a public or private IaaS compute cloud.  

With CloudStack you can:  

Set up an on-demand elastic cloud computing service.
Allow end-users to provision resources

## Cloud Infrastructure Overview
Resources within the cloud are managed as follows:  

- Regions: A collection of one or more geographically proximate zones managed by one or more management servers. 
- Zones: Typically, a zone is equivalent to a single datacenter. A zone consists of one or more pods and secondary storage.  
- Pods: A pod is usually a rack, or row of racks that includes a layer-2 switch and one or more clusters.  
- Clusters: A cluster consists of one or more homogenous hosts and primary storage.  
- Host: A single compute node within a cluster; often a hypervisor.  
- Primary Storage: A storage resource typically provided to a single cluster for the actual running of instance disk images. (Zone-wide primary storage is an option, though not typically used.)  
- Secondary Storage: A zone-wide resource which stores disk templates, ISO images, and snapshots.