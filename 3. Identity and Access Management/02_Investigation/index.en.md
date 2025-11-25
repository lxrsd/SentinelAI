---
title: "Investigation"
weight: 32
---

## Scenario: IAM Root Cause Review

After completing the discovery analysis, you receive a follow-up email from your CTO:

:::alert{header="From: Judy Hopps <cto@company.com>" type="warning"}
**Subject: RE: IAM Audit Findings — Need Root Cause Analysis ASAP**

Team,

Excellent work identifying those overprivileged roles! Now I need to understand *how we got here* before we present to the auditors.

Specifically, I need to know:
- When were these admin and full-access roles created?
- Who or what created them (users, CloudFormation, automation)?
- Are any of these roles actually being used by our systems?

This will help us explain the situation to the auditors and plan safe remediation.

Thanks,  
Judy Hopps  
CTO
:::

---

**Your Mission:** Use **Kiro CLI** to investigate when and how the overprivileged roles were created, identify who or what created them, and determine if they're actively being used.

---

## Using the Security Agent

:::alert{header="Agent Session" type="info"}
Continue working with your Kiro CLI security agent session from the Discovery phase. If you need to start a new session. Make sure you are in the right directory:
```bash
cd /home/ec2-user/SMB306/identity-access-management
```
Then resume your security agent session.
```bash
kiro-cli chat --agent security-agent --resume
```
:::

The agent will cross-reference existing role data with CloudTrail events for deeper investigation.

---

## Investigation Exercise: Root Cause and Usage Analysis

**Objective:** Understand when the overprivileged roles were created, who/what created them, and whether they're actively being used.

**Your Task:** Use Kiro CLI to examine CloudTrail logs and IAM activity history for the roles discovered in the previous phase.

**Focus on the roles you discovered:**
- 2 admin roles (Finance, Operations)
- 2 EC2 full-access roles (Development, Testing)
- 1 S3 full-access role (Marketing)
- 1 Lambda full-access role (Engineering)
- 1 Lambda full-access user (DevOps)

::::expand{header="Step-by-step walk-through" defaultExpanded=true}
Here's an example investigation prompt to guide your analysis:

:::code{showCopyAction=true showLinenumbers=false language=text}
I need to investigate the overprivileged IAM roles we discovered to understand how they were created and if they're being used. Please analyze:

CREATION TIMELINE:
- When were the SMB306 roles created (CreateRole events in CloudTrail)?
- Who or what created them (user identity, CloudFormation, etc.)?
- Were they created all at once or over time?
- What was the source of the creation (Console, CLI, CloudFormation)?

ROLE USAGE ANALYSIS:
- Check last-used timestamps for each role
- Identify which roles have been used in the last 30 days
- Determine which AWS services are using these roles (EC2, Lambda, etc.)
- Highlight roles that have NEVER been used

DEPARTMENT ANALYSIS:
- Group findings by department (Finance, Operations, Development, Testing, Marketing, Engineering, DevOps)
- Identify which departments have the most overprivileged access
- Determine if usage patterns match department responsibilities

SUMMARY FOR CTO:
- Timeline of when overprivileged access was granted
- Identification of creation source (automation vs. manual)
- Usage status for each role (Active, Inactive, Never Used)
- Recommendations for safe remediation based on usage
:::

After the prompt executes, the security agent will:
1. Extract IAM creation events from CloudTrail

![](/static/images/iam-investigation-q1.png)

2. Analyze role usage patterns and last-used timestamps

![](/static/images/iam-investigation-q2.png)

3. Correlate roles with their departments and usage

![](/static/images/iam-investigation-q3.png)

4. Provide a comprehensive timeline for the CTO's audit report

![](/static/images/iam-investigation-q4.png)

::::

::::expand{header="Expected Investigation Findings" defaultExpanded=true}
Your investigation should reveal that **all 7 IAM entities were created by CloudFormation** during the workshop stack deployment. The roles were created as part of the `rivsmb306-identity-access-management` stack. **Usage analysis** should show that most roles have **never been used** or have **minimal activity**, indicating they're overprivileged for their actual needs.

**Key Findings by Department:**

**Finance Department** (Admin role): Created by CloudFormation, likely unused - represents excessive privilege for finance operations.

**Operations Department** (Admin role): Created by CloudFormation, may show some activity - still overprivileged for typical operations tasks.

**Development Department** (EC2 full access): Created by CloudFormation, usage depends on EC2 activity - likely needs only specific EC2 permissions.

**Testing Department** (EC2 full access): Created by CloudFormation, usage depends on testing activity - could be scoped to test environments only.

**Marketing Department** (S3 full access): Created by CloudFormation, likely minimal usage - probably only needs access to specific marketing buckets.

**Engineering Department** (Lambda full access): Created by CloudFormation, usage depends on Lambda deployments - could be scoped to specific functions.

**DevOps User** (Lambda full access): Created by CloudFormation, programmatic access user - likely needs more restrictive permissions.

**Root Cause Summary:** The overprivileged roles were created through infrastructure-as-code (CloudFormation) without proper least-privilege scoping. This is a common pattern where convenience during initial deployment leads to long-term security risks.
::::

**Checkpoint (15 minutes):** You should now understand when and how the overprivileged roles were created, their usage patterns, and which ones are safe candidates for remediation.

---

## Next Steps

With your investigation complete, you're ready to move to the **Planning** phase where you'll develop a safe remediation strategy to reduce privileges while maintaining business operations.

