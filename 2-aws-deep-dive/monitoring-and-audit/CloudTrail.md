# CloudTrail
Record all actions taken when provsioning and modifying resources in AWS accounts.
Store historical logs, by default 90 days. Integrate with other services. 

## Pricing
- first CT in each region is free. (Not counting S3 or Lamnda data events)
- $2 per 100K management event
- $0.1 per 100 K data events. 

## Set Up CloudTrail
- Create Trail: apply trail to all regions
- Data event: S3, Lambda
- Enable log file validation: prevent altering the logs after creations. 

## Querying Events
- 
