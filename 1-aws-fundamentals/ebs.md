# EBS Volume

* An EC2 machine loses its root volume (main drive) when it is manually terminated.
* Unexpected terminations might happen from time to time (AWS would email you)
* Sometimes, you need a way to store your instance data somewhere
* An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
* It allows your instances to persist data
* At least one EBS is attached to EC2 when instance launches, and it used to install operating system. You could add more EBS to add application, data and so on. 

#### EBS Volume
* It’s a network drive (Not a physical drive)
    * It uses the network to communicate the instance, which means there might be a bit of latency
    * It can be detached from an EC2 instance and attached to another one quickly
* It’s locked to an Availability Zone (AZ)
    * An EBS Volume in us-east-1a cannot be attached to us-east-1b
    * To move a volume across, you first need to snapshot it
    * Alternatively replicate in the same AZ. 
* Have a provisioned capacity (size in GBs and IOPs)
    * You get billed for all the provisioned capacity
    * You can increase the capacity of the drive over time

#### IOPS vs Throughput
* IOPS: measures number of read and write operations per second. Important metric for quick transactions, low latency apps, transactional workloads. The ability to action reads and writes very quickly. 
  - Provisioned IOPS SSD (IO1 or IO2)
* Throughput: measures the number of bits read or written per second (MB/s). Important metric for large datasets, large I/O size, complex queries. The ability to deal with large datasets. 
  - Throughput Optimized HDD (ST1). 

#### EBS Volume Types
- EBS Volumes come in 4 types 
- GP2 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads 
  - SSD: A solid-state drive is a solid-state storage device that uses integrated circuit assemblies to store data persistently, typically using flash memory, and functioning as secondary storage in the hierarchy of computer storage
- IO1 (SSD): Highest-performance SSD volume for mission-critical low-latency or high- throughput workloads 
- ST1 (HDD): Low cost HDD volume designed for frequently accessed, throughput- intensive workloads 
  - HDD: A hard disk drive, hard disk, hard drive, or fixed disk is an electro-mechanical data storage device that stores and retrieves digital data using magnetic storage and one or more rigid rapidly rotating platters coated with magnetic material. 
- SC1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads 
- EBS Volumes are characterized in Size | Throughput | IOPS
- When in doubt always consult the AWS documentation
-  Only GP2 and IO1 can be used as boot volumes

#### EBS Volume Types Use Cases
1. GP2
- Recommended for most workloads 
- System boot volumes
- Virtual desktops
- Low-latency interactive apps
- Development and test environments
- Good for boot volumes or deployment and test application which are not latency sensitive.
   - GP3: latest version for General purpose. 

2. IO1 [Provisioned IOPS SSD]
- Critical business applications that require sustained IOPS performance, or more than 16,000 IOPS per volume (gp2 limit)
-  Large database workloads, such as: MongoDB, Cassandra, Microsoft SQL Server, MySQL, PostgreSQL, Oracle
-  High performation option and most expensive. 
-  For I/O instensive application, latensy sensitive
   - IO2: latest generation for provisioned IOPS. More IOPS and higher durability than IO1, with same price. 
   - IO2 Block Express: SAN (Storage Area Network) in the cloud: highest performance, sub-millisecond latency. Same durability with IO2. 

3. ST1 [Throughput Optimized HDD]
- Streaming workloads requiring consistent, fast throughput at a low price. Frequently-accessed, throughput-intensive workloads. 
- Big data, Data warehouses, Log processing, ETL, and log processing. 
- Low-cost HDD volume
- Apache Kafka
 - Cannot be a boot volume
 
4. SC1 [Cold HDD]
- Throughput-oriented storage for large volumes of data that is infrequently accessed
- Scenarios where the **lowest storage cost** is important
- Cannot be a boot volume
- Good choice for colder data
- Application not require good performance; less frequently-accessed data. 

#### EBS Volume Types Summary
- gp2: General Purpose Volumes (cheap)
- gp3: the latest generation. 
- io1: Provisioned IOPS (expensive)
- st1: Throughput Optimized HDD
- sc1: Cold HDD, Infrequently accessed data

#### EBS Volume Resizing
* Feb 2017: You can resize your EBS Volumes
* After resizing an EBS volume, you need to repartition your drive
* Highly scalable.

EBS Snapshots
* EBS Volumes can be backed up using “snapshots”
* Snapshots only take the actual space of the blocks on the volume
* If you snapshot a 100GB drive that only has 5 gb of data, then your EBS snapshot will only be 5 gb
* Snapshots are used for:
    * Backups: ensuring you can save your data in case of catastrophe
    * Volume migration
        * Resizing a volume down
        * Changing the volume type
        * Encrypt a volume

#### EBS Encryption
* When you create an encrypted EBS volume, you get the following:
    * Data at rest is encrypted inside the volume
    * All the data in flight moving between the instance and the volume is encrypted
    * All snapshots are encrypted
    * All volumes created from the snapshots are encrypted
* Encryption and decryption are handled transparently (you have nothing to do)
* Encryption has a minimal impact on latency
* EBS Encryption leverages keys from KMS (AES-256)
* Copying an unencrypted snapshot allows encryption
* If the EBS volume is unencrypted, then the snapshot is also unencrypted. 

#### EBS vs. Instance Store
* Some instance do not come with Root EBS volumes
* Instead, they come with “instance Store”
* Instance store is physically attached to the machine
* Pros:
    * Better I/O performance
* Cons:
    * On termination, the instance store is lost
    * You can’t resize the instance store
    * Backups must be operated by the user
* Overall, EBS-backed instances should fit most applications workloads

#### EBS Summary

* EBS can be attached to only one instance at a time
* EBS are locked at the AZ level
* Migrating an EBS volume across AZ means first backing it up (snapshot), then recreating it in the other AZ
* EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
* Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated. (You can disable that)
* In some cases, it's better to externalize your RDS database so that it won't get deleted when you delete your elastic beanstalk enviornment
* Elastic Beanstalk relies on CloudFormation

#### EBS Volume Types - Use cases 

* Big Data / Data Warehouses / Log Processing : ST1 (HDD)
* Lowest storage cost : SC1 (HDD)
* NoSQL such as MongoDB, Cassandra or MSQL : IO1 (SSD)
* Low latency applications : GP2 (SSD) 
