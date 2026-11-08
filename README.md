# Project 19 - AWS Cloud Support Break-Fix Troubleshooting

## Overview
Deliberately broke a working AWS environment six different ways and documented how each fault was diagnosed and fixed using the same tools a cloud support engineer uses: Reachability Analyzer, CloudWatch Logs, CloudTrail, and load balancer health status.

## Tools Used
EC2, VPC, Security Groups, Network ACLs, Route Tables, Reachability Analyzer, IAM, Lambda, CloudWatch Logs, CloudTrail, Application Load Balancer

## Troubleshooting Cases

| # | Symptom | Investigation tool | Root cause | Fix |
|---|---|---|---|---|
| 1 | Web page times out | Reachability Analyzer | Security group missing port 80 rule | Re-added HTTP inbound rule |
| 2 | Web page times out, security group is correct | Reachability Analyzer | Network ACL deny rule on port 80 | Deleted the deny rule |
| 3 | Web page times out, SG and NACL are correct | Route table review, Reachability Analyzer | No 0.0.0.0/0 route to the internet gateway | Restored the default route |
| 4 | Lambda returns AccessDenied | CloudWatch Logs, CloudTrail | Execution role lacked S3 read permission | Attached a read-only S3 policy |
| 5 | Lambda fails after 1 second | CloudWatch Logs | Timeout set below function runtime | Raised timeout to 10 seconds |
| 6 | Target group shows Unhealthy while the ALB still serves traffic | Target group Targets tab | Health check path /health doesn’t exist, so nginx returns 404 | Changed the health check path to / |

## Case Write-Ups
For each case: Symptom, Investigation, Root cause, Fix, Prevention (see screenshots).

## Key Learnings
- Troubleshoot network faults in order: security group, network ACL, route table, then the instance itself
- Reachability Analyzer names the exact blocking component
- CloudWatch Logs show what the function did; CloudTrail shows what AWS denied and why
- Health check settings are a common cause of unhealthy targets
- An ALB sends traffic to unhealthy targets when all targets in the group are failing, so health check status needs its own monitoring.

## Cost
Approximately $0.10 to $0.30. All resources deleted after the project.

---
*Part of my AWS Cloud Engineering Portfolio | [View all projects](https://github.com/dcprice79)*
