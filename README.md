#  AWS Integrated GRC Platform
### Governance • Risk • Compliance — Cloud-Native Automation on AWS

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=flat&logo=amazon-aws)
![CloudFormation](https://img.shields.io/badge/CloudFormation-IaC-blue?style=flat&logo=amazon-aws)
![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat&logo=python)
![MySQL](https://img.shields.io/badge/MySQL-8.0.40-blue?style=flat&logo=mysql)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)
![Tests](https://img.shields.io/badge/Tests-22%2F22%20Passing-brightgreen?style=flat)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen?style=flat)

---

##  Project Overview

A **fully automated, cloud-native GRC (Governance, Risk & Compliance) platform** deployed on AWS using Infrastructure as Code. This capstone project demonstrates enterprise-grade compliance monitoring, risk management, and audit-ready infrastructure across six industry-standard frameworks — with zero manual intervention after deployment.

> **The Problem:** Organizations spend up to $280,000/year on manual GRC processes. Data breaches average $4.45M. Non-compliant organizations face fines 3x higher than compliant ones.
>
> **The Solution:** An automated AWS platform that reduces GRC costs by 93% — from ~$280,000/year to ~$500/year.

---

##  Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    SECURITY LAYER                           │
│   CloudTrail  •  AWS Config  •  KMS  •  IAM  •  CloudWatch  │
├─────────────────────────────────────────────────────────────┤
│                   APPLICATION LAYER                         │
│      Lambda Functions  •  EventBridge  •  SNS Alerts        │
├─────────────────────────────────────────────────────────────┤
│                      DATA LAYER                             │
│    RDS MySQL 8.0.40  •  DynamoDB (3 tables)  •  S3 (2)     │
├─────────────────────────────────────────────────────────────┤
│                    NETWORK LAYER                            │
│    VPC  •  Public/Private Subnets  •  NAT Gateway  •  ALB  │
└─────────────────────────────────────────────────────────────┘
         Region: us-east-1  |  Deployed via CloudFormation
```

---

##  Key Features

-  **Automated Compliance Monitoring** — Lambda checks compliance every hour via EventBridge
-  **Real-time Alerting** — CloudWatch + SNS email alerts when compliance drops below 80%
-  **6 Compliance Frameworks** — ISO 27001, NIST CSF, PCI DSS, HIPAA, GDPR, SOC 2
-  **Enterprise Security** — KMS encryption, VPC isolation, IAM least privilege
-  **Risk Register** — 6 risks tracked with scores, owners, and mitigation strategies
-  **Audit Trail** — CloudTrail + AWS Config recording every change 24/7
-  **Fully Tested** — 22/22 automated tests passing across 8 test classes
-  **Cost Efficient** — ~$500/year vs $280,000/year for manual GRC (93% savings)

---

##  AWS Services Used

| Service | Purpose |
|---|---|
| **CloudFormation** | Infrastructure as Code — 2 stacks, 20+ resources |
| **RDS MySQL 8.0.40** | Relational database — frameworks, controls, risks, assets |
| **DynamoDB** | NoSQL tables — risk register, controls, compliance status |
| **S3** | Encrypted storage — evidence and compliance reports |
| **Lambda (Python 3.11)** | Serverless compliance monitoring and DB initialization |
| **EventBridge** | Scheduled hourly compliance checks |
| **CloudWatch** | Metrics, alarms, and monitoring |
| **SNS** | Email notifications for compliance alerts |
| **KMS** | Customer-managed encryption keys for RDS and S3 |
| **CloudTrail** | Full API audit logging |
| **AWS Config** | Continuous resource configuration recording |
| **IAM** | Roles, policies, and least privilege access control |
| **VPC** | Network isolation with public/private subnets |

---

##  Compliance Frameworks

| Framework | Version | Controls | Compliance % |
|---|---|---|---|
| ISO 27001 | 2022 | 6 | 66.7% |
| NIST CSF | v1.1 | 5 | 60.0% |
| PCI DSS | v3.2.1 | 5 | 80.0% |
| HIPAA | 2013 | Roadmap | — |
| GDPR | 2018 | Roadmap | — |
| SOC 2 | 2022 | Roadmap | — |
| **TOTAL** | | **16** | **68.9%** |

---

##  Project Structure

```
GRC208-AWS-Capstone-Project/
├── cloudformation-network-stack.yaml    # VPC, subnets, security groups
├── cloudformation-database-stack.yaml  # RDS, DynamoDB, S3, KMS
├── lambda_compliance_monitor.py         # Compliance monitoring function
├── sample_data.sql                      # Database initialization script
├── test_cases.py                        # 22 automated test cases
├── grc-dashboard.jsx                    # React GRC dashboard
├── grc-dashboard.css                    # Dashboard styling
├── deploy.sh                            # Deployment automation script
├── requirements.txt                     # Python dependencies
├── README.md                            # This file
├── DEPLOYMENT_GUIDE.md                  # Step-by-step deployment guide
├── BEST_PRACTICES.md                    # AWS & GRC best practices
├── AWS_SERVICES_GUIDE.md                # AWS services deep dive
├── architecture_design.md               # Architecture documentation
└── diagrams/                            # Architecture diagrams
```

---

##  Deployment Guide

### Prerequisites
- AWS Account with appropriate permissions
- AWS CLI v2 configured
- Python 3.8+

### Phase 1 — Network Stack
```bash
aws cloudformation create-stack \
  --stack-name grc-network-stack \
  --template-body file://cloudformation-network-stack.yaml \
  --parameters ParameterKey=EnvironmentName,ParameterValue=grc-capstone \
  --region us-east-1
```

### Phase 2 — Database Stack
```bash
aws cloudformation create-stack \
  --stack-name grc-capstone-database-stack \
  --template-body file://cloudformation-database-stack.yaml \
  --parameters \
    ParameterKey=EnvironmentName,ParameterValue=grc-capstone \
    ParameterKey=DBUsername,ParameterValue=grcadmin \
    ParameterKey=DBPassword,ParameterValue=YourSecurePassword \
  --capabilities CAPABILITY_IAM \
  --region us-east-1
```

### Phase 3 — Lambda Functions
```bash
zip -r lambda_compliance_monitor.zip lambda_compliance_monitor.py
aws lambda create-function \
  --function-name grc-compliance-monitor \
  --runtime python3.11 \
  --role arn:aws:iam::ACCOUNT_ID:role/grc-lambda-role \
  --handler lambda_compliance_monitor.lambda_handler \
  --zip-file fileb://lambda_compliance_monitor.zip \
  --region us-east-1
```

### Phase 4 — Validate Deployment
```bash
# Check stack status
aws cloudformation describe-stacks \
  --query 'Stacks[*].[StackName,StackStatus]' \
  --output table

# Run test suite
python3 test_cases.py
```

---

##  Test Results

```
Ran 22 tests in 0.001s — OK

✅ TestComplianceMonitoring    (3 tests)
✅ TestRiskAssessment          (3 tests)
✅ TestDataValidation          (4 tests)
✅ TestDatabaseOperations      (3 tests)
✅ TestComplianceFrameworks    (2 tests)
✅ TestAuditLogging            (3 tests)
✅ TestReportGeneration        (2 tests)
✅ TestIntegration             (2 tests)
```

---

##  Challenges & Solutions

| Challenge | Solution |
|---|---|
| YAML indentation errors in S3 encryption | Fixed using Python scripting to replace corrupted blocks |
| RDS free tier backup retention limit | Set BackupRetentionPeriod to 0 for free tier |
| MySQL 8.0.35 unavailable in us-east-1 | Upgraded to MySQL 8.0.40 |
| RDS password special character restriction | Changed to alphanumeric compliant password |
| CloudShell cannot reach private subnet RDS | Used VPC-enabled Lambda for DB initialization |
| Lambda missing VPC permissions | Added AWSLambdaVPCAccessExecutionRole |
| DynamoDB GSI missing RANGE key | Corrected KeySchema definition |
| CloudTrail S3 bucket policy error | Added correct CloudTrail service permissions |
| AWS Config recorder failure | Restarted recorder and verified delivery channel |

---

##  Lessons Learned

- **Infrastructure as Code requires precision** — one misplaced space breaks deployments
- **Always validate templates** before deployment using `aws cloudformation validate-template`
- **Free tier limitations** must be accounted for in templates from day one
- **VPC design determines connectivity** — private subnets need deliberate access strategies
- **IAM least privilege** means every service needs its own role — not broad admin access
- **Error messages are your best friend** — each one points directly to the root cause
- **Technical compliance is the floor** — security and ethics must go further

---

##  Economic Impact

| Approach | Annual Cost | Notes |
|---|---|---|
| Traditional Manual GRC | ~$280,000 | 2 FTEs + software + audits + penalties |
| AWS GRC Platform | ~$500 | Infrastructure + services |
| **Savings** | **~$262,000** | **93% cost reduction** |

---

##  Future Roadmap

- [ ] ECS Fargate containerized web application
- [ ] API Gateway REST endpoints
- [ ] AWS Security Hub integration
- [ ] Multi-region deployment
- [ ] AI-powered risk scoring with Amazon Bedrock
- [ ] Zero-trust security model

---

##  Author

**Lilian Omoike**
GRC208 — AWS Cloud Infrastructure Capstone Project

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/your-linkedin)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat&logo=github)](https://github.com/Omoike-lilian)

---

##  License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

*Built with ❤️ on AWS | GRC208 Capstone Project | April 2026*
