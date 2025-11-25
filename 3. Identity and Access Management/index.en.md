---
title: "Adventure 2 - Identity and Access Management"
weight: 30
---

# Overview

Identity and Access Management (IAM) is the foundation of cloud security. Many organizations struggle to maintain least-privilege access, resulting in over-permissioned roles, compliance gaps, and operational risks. This module demonstrates how **Kiro CLI** helps teams automate IAM governance—from identifying risky permissions to safely remediating and preventing privilege drift across AWS environments.

## Workshop Environment: IAM Compliance Crisis

Your workshop account contains 7 overprivileged IAM entities deployed via CloudFormation:
- **2 admin roles** (Finance, Operations) with full AdministratorAccess
- **2 EC2 full-access roles** (Development, Testing) with unrestricted EC2 permissions
- **1 S3 full-access role** (Marketing) with complete S3 control
- **1 Lambda full-access role** (Engineering) with unrestricted Lambda permissions
- **1 Lambda full-access user** (DevOps) with programmatic access

These roles represent a common scenario where convenience during initial deployment leads to long-term security risks and compliance violations.

:::alert{header="Security Focus" type="success"}
Throughout this module, you'll transform these overprivileged roles into least-privilege configurations using time-based, tag-based, and IP-based access controls—while implementing automated monitoring to prevent future privilege creep.
:::

## The Challenge: Audit-Driven IAM Remediation

**The Scenario**: Your CTO, Judy Hopps, has received notice from compliance auditors about overprivileged IAM roles. You'll receive a series of emails guiding you through discovery, investigation, planning, remediation, and prevention—each building on the previous phase.

**The Reality**: Manually auditing and correcting IAM configurations is complex and error-prone. Organizations struggle to implement least-privilege access without breaking functionality, and often lack the expertise to design proper time-based, tag-based, and IP-based restrictions.

**The Kiro CLI Solution**: Kiro CLI helps you analyze IAM configurations, design least-privilege policies with advanced conditions, update CloudFormation templates, and implement automated monitoring—all while maintaining business continuity.

## Module Structure

This IAM module follows the five-phase security methodology, guiding you from detection to long-term prevention:

1. **[Discovery](01_Discovery/)** — Identify overprivileged IAM roles and users across departments
2. **[Investigation](02_Investigation/)** — Understand how roles were created and their actual usage patterns
3. **[Planning](03_Planning/)** — Design least-privilege policies with time, tag, and IP-based conditions
4. **[Remediation](04_Remediation/)** — Update CloudFormation template and deploy new policies
5. **[Prevention](05_Prevention/)** — Research monitoring options and implement automated alerts

:::alert{header="Real-World Application" type="info"}
While this module uses specific IAM roles and conditions, the workflow and Kiro CLI techniques apply to any IAM governance challenge—enabling scalable, auditable, and AI-assisted security automation.
:::

---

Ready to transform IAM risk management into an automated governance framework?  
Let's begin with the **Discovery** phase, where you'll identify overprivileged IAM roles and answer the CTO's urgent questions.

