# Resilience in EC2 Image Builder<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

The EC2 Image Builder service allows you to distribute images built in one Region with other Regions, giving them multi\-Region resiliency for AMIs\. There is no mechanism to "back up" image pipelines, recipes, or components\. You can store the recipe and component documents outside of the Image Builder service, such as in an Amazon S3 bucket\. 

The EC2 Image Builder cannot be configured for High Availability \(HA\)\. You can distribute images to multiple Regions to make the images more highly available\. 