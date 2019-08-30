***[CloudStack]()***
- [What is Apache CloudStack](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#what-is-apache-cloudstack)
- [Cloud Infrastructure Overview](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#cloud-infrastructure-overview)
  - [About Regions](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-regions)
  - [About Zones](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-zones)
  - [About Pods](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-pods)
  - [About Clusters](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-clusters)
  - [About Hosts](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-hosts)
  - [About Primary Storage](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-primary-storage)
  - [About Secondary Storage](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/CloudStack%E6%9E%B6%E6%9E%84.md#about-secondary-storage)

***For more details, please refer to:***    
  
[http://docs.cloudstack.apache.org/en/latest/conceptsandterminology/concepts.html](http://docs.cloudstack.apache.org/en/latest/conceptsandterminology/concepts.html)
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

![regionoverview.png](https://github.com/Leanna-Lee/MyNotes/blob/master/CloudStack/img/region-overview.png)
### About Regions
To increase reliability of the cloud, you can optionally group resources into multiple geographic regions. A region is the largest available organizational unit within a CloudStack deployment. A region is made up of several availability zones, where each zone is roughly equivalent to a datacenter. Each region is controlled by its own cluster of Management Servers, running in one of the zones. The zones in a region are typically located in close geographical proximity. Regions are a useful technique for providing fault tolerance and disaster recovery.

By grouping zones into regions, the cloud can achieve higher availability and scalability. User accounts can span regions, so that users can deploy VMs in multiple, widely-dispersed regions. Even if one of the regions becomes unavailable, the services are still available to the end-user through VMs deployed in another region. And by grouping communities of zones under their own nearby Management Servers, the latency of communications within the cloud is reduced compared to managing widely-dispersed zones from a single central Management Server.

Usage records can also be consolidated and tracked at the region level, creating reports or invoices for each geographic region.


### About Zones
### About Pods
### About Clusters
### About Hosts
### About Primary Storage
### About Secondary Storage