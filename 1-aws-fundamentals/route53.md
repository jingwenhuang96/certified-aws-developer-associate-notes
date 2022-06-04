# Route 53
 
Route 53 is a managed DNS (Domain Name System) 
- Alows you to map a domain name that you own to EC2 instance, load balancer, S3 buckets. 

DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.

Steps to use route53 to direct traffic to load balancer:
- Register domain name
- Resgiter EC2 instances
  - configure instance: install httpd
  - security group: http
  - create load balancer - application load balancer; HTTP; configure routing; register targets. 
- copy the DNS of load balancer: the address is not easy to remember as website; that is why use route53
- Route53 
  - hosted zone: create map between DNS of load balancer and domain name. 

In AWS, the most common records are (will be on exam):
* A: URL to IPv4
* AAAA: URL to IPv6
* CNAME: URL to URL
* ALIAS: URL to AWS resource

Route 53 can use:
* Public domain names you own
* Private domain names that can be resolved by your instances in your VPCs

Route53 has advanced features such as:
* Load balancing (through DNS - also called client load balancing)
* Health checks (although limited…)
* Routing policy: simple, failover, geolocation, geoproximity, latency, weighted

Prefer Alias over CNAME for AWS resources (for performance reasons)
