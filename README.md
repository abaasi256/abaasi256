# Abaasi Kisuule
### Cloud Security Engineer | AWS | DevSecOps | Application Security

<div align="left">

[![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat-square&logo=amazon-aws&logoColor=white)](#)
[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white)](#)
[![Python](https://img.shields.io/badge/Python-14354C?style=flat-square&logo=python&logoColor=white)](#)
[![Security](https://img.shields.io/badge/Security-555555?style=flat-square&logo=security&logoColor=white)](#)

</div>

## Professional Summary

Cloud Security Engineer focused on building resilient AWS environments and securing SDLC pipelines. I specialize in identifying cloud misconfigurations, hardening serverless architectures, and integrating automated security checks (SAST, DAST, SCA) into engineering workflows. I secure infrastructure by prioritizing pragmatic risk reduction over alert fatigue. I build automated CI/CD guardrails, enforcing infrastructure-as-code (IaC) security standards before deployments hit production. Focused on practical security improvements that can be implemented quickly without slowing down engineering teams.

## Security Approach

- **Attacker Mindset:** I evaluate systems based on practical attack vectors—focusing heavily on API abuse, IAM privilege escalation, SSRF, and credential exposure in CI/CD environments.
- **Defender Mindset:** I implement least-privilege and zero-trust patterns in AWS environments, enforce infrastructure-as-code security standards, and deploy automated guardrails that prevent misconfigurations from ever reaching production.
- **Focus Areas:** Deep dive into AWS IAM configurations, API Gateway security, secrets management, and cloud misconfiguration prevention.

## ₿ Exchange Security Focus

I am particularly interested in securing high-risk financial systems such as cryptocurrency exchanges. I approach these systems assuming attackers target APIs first, keys will be leaked, and abuse will be automated at scale.

**Focus areas:**
- Preventing API abuse (rate limiting, replay attacks, key misuse)
- Securing authentication flows and session handling
- Protecting against privilege escalation in multi-tenant environments
- Detecting anomalous transaction patterns and automation abuse

## 🔬 Deep Dive: IAM Privilege Escalation Risk

During Terraform review, I identified roles allowing:
- `iam:PassRole` on wildcard resources
- Overly broad `s3:*` permissions

**Risk:**
An attacker with limited access could escalate privileges by passing a more privileged role to an AWS compute service (like EC2 or Lambda) they control, then extracting the temporary credentials.

**Fix:**
- Restricted `iam:PassRole` to only specific, required ARNs.
- Scoped S3 permissions to required actions only (e.g., `s3:GetObject` on specific buckets).
- Added policy validation via **Checkov** rules in CI/CD to prevent reintroduction.

**Result:**
Drastically reduced theoretical privilege escalation paths across the AWS environment without blocking developer velocity.

## Engineering Implementations

### 1. DevSecOps CI/CD Platform
- **System Purpose:** A centralized CI/CD infrastructure for microservices, automating code deployment and infrastructure provisioning.
- **Abuse Scenarios Considered:** 
  - Attacker discovering hardcoded secrets in a git repository and gaining initial AWS access.
  - Supply chain attack via vulnerable open-source dependencies leading to remote code execution (RCE).
  - Malign insider or compromised developer account deploying overly permissive IAM policies to establish persistence.
- **Security Findings:** Identified potential credential spillage, overly broad `s3:*` and `iam:PassRole` permissions in IaC configurations, and vulnerable open-source dependencies.
- **Security Improvements:**
  - Integrated **Checkov** and **tfsec** into GitHub Actions to block non-compliant Terraform applies.
  - Deployed **Trivy** for container image scanning and **Snyk** for dependency tracking.
  - Automated secret scanning via pre-commit hooks to halt commits containing authentication tokens.

### 2. Secure Serverless Architecture (AWS Lex/Lambda)
- **System Purpose:** Event-driven serverless API relying on AWS Lex and Lambda functions for processing stateful interactions.
- **Abuse Scenarios Considered:**
  - Injection via unsanitized input payloads in Lambda handlers resulting in unauthorized backend data access.
  - API abuse through lack of rate limiting leading to resource exhaustion and excessive AWS billing.
  - Attacker leveraging a compromised Lambda function to execute broad AWS CLI commands due to excessive execution role permissions.
- **Security Findings:** Default Lambda IAM execution roles possessed blanket DynamoDB read/write access. API endpoints lacked usage rate limiting, leaving the system vulnerable to DoS or logic abuse.
- **Security Improvements:**
  - Hardened IAM execution roles to strict least-privilege, explicitly allowing access solely to required ARNs.
  - Put APIs behind strict AWS WAF rate-limiting rules and API Gateway usage plans.
  - Implemented regex-based input validation and payload sanitization to neutralize command injection vectors.

### 3. Automated Cloud Security & Incident Response
- **System Purpose:** Cloud monitoring framework aggregating audit logs and executing automated scripts in response to anomalies.
- **Abuse Scenarios Considered:**
  - Attacker tampering with or deleting CloudTrail logs to cover tracks during an infrastructure compromise.
  - Delayed manual incident response allowing a leaked AWS access key to be used for crypto-mining (resource hijacking).
- **Security Findings:** Incident mitigation required manual intervention, elevating Time to Mitigate (TTM). CloudTrail logs were not fully protected against destructive actions by privileged accounts.
- **Security Improvements:**
  - Configured **AWS EventBridge** to capture GuardDuty findings and trigger Lambda auto-remediation (e.g., fast-isolating compromised EC2 instances or revoking leaked IAM credentials).
  - Enforced AWS KMS encryption and MFA-delete on S3 security data lakes to ensure the integrity of forensic logs.

## 🚧 Current Focus

- Building a serverless security audit tool to detect:
  - IAM misconfigurations
  - Exposed APIs
  - Weak authentication flows
- Simulating real-world attack scenarios against cloud-native applications.
- Improving detection of misconfigurations before deployment (shift-left security).

## Technology Stack

- **Cloud Security:** AWS (IAM, KMS, WAF, GuardDuty, Security Hub, Config), IAM Least-Privilege
- **Infrastructure as Code:** Terraform, CloudFormation, Docker
- **Security Automation:** GitHub Actions, Python, Bash
- **AppSec Tools:** Semgrep, Snyk, Trivy, Checkov, tfsec, OWASP ZAP

## Professional Connections

- **LinkedIn**: [Abaasi Kisuule](https://www.linkedin.com/in/abaasi-k-b79420340)
- **GitHub**: [@abaasi256](https://github.com/abaasi256)
- **Email**: abaasi.dev@gmail.com