---
title: "Discovery"
weight: 31
---

## Scenario: Emergency IAM Compliance Alert

You arrive Monday morning to find this urgent email in your inbox:

:::alert{header="From: Judy Hopps <cto@company.com>" type="error"}
**Subject: URGENT — IAM Compliance Review Before Audit**

Team,

We've received notice from our compliance auditors requesting a detailed IAM report by end of day.  
Initial scans show several roles with **too much permissions** to our production environment. This poses a major compliance and security risk.

Just to refresh your memory...our stack consist of EC2, S3 and Lambda.

I need immediate answers to the following:

1. **Which IAM roles and users currently have admin-level permissions?**  
2. **Do any of our roles have full access to our tech stack services?**  

Thanks,  
Judy Hopps  
CTO
:::

**Your Mission:** Use **Kiro CLI** to answer the CTO's questions by identifying IAM roles and users with admin-level permissions and detecting full access to EC2, S3, and Lambda services. The findings will form the baseline for investigation and remediation.

---

## Using the Security Agent

:::alert{header="Agent Session" type="info"}
For all exercises in this section, you'll be working with the Kiro CLI security agent. Start by navigating to the identity and access management directory:
```bash
cd /home/ec2-user/SMB306/identity-access-management
```
Then start your security agent session:
```bash
kiro-cli --agent security-agent
```
Keep your agent session open for the full module. The agent maintains context, improving discovery accuracy and follow-up automation.
:::

---

## Discovery Exercise: IAM Role and User Analysis

**Objective:** Answer the CTO's two questions by discovering IAM roles and users with admin-level permissions and full access to the tech stack (EC2, S3, Lambda).

**Your Task:** Create a prompt for Kiro CLI to analyze IAM roles and users. Your prompt should address the CTO's specific questions:

- Identify IAM roles and users with administrator-level permissions (Question 1)
- Detect roles with full access to EC2, S3, or Lambda services (Question 2)
- List attached policies for each role and user
- Organize findings by department for easy reporting

:::alert{header="Important: Exclude Workshop Infrastructure" type="warning"}
**When analyzing IAM roles, exclude any roles with "WS" in the name** (e.g., WSParticipantRole, WSDataPlaneRole). These are Workshop Studio infrastructure roles required for the workshop environment and should not be included in your security analysis.
:::  

::::expand{header="Step-by-step walk-through" defaultExpanded=true}
A focused prompt helps answer the CTO's specific questions. Here's an example prompt for this exercise:

:::code{showCopyAction=true showLinenumbers=false language=text}
I need to answer urgent questions from our CTO about IAM permissions before an audit. Our tech stack consists of EC2, S3, and Lambda. Please analyze:

ADMIN-LEVEL PERMISSIONS (Question 1):
- List all IAM roles with AdministratorAccess policy attached
- List all IAM users with AdministratorAccess policy attached
- Include role/user names, creation dates, and department (from tags)
- EXCLUDE any roles with "WS" in the name (workshop infrastructure roles)

FULL ACCESS TO TECH STACK (Question 2):
- Identify roles with AmazonEC2FullAccess or ec2:* permissions
- Identify roles with AmazonS3FullAccess or s3:* permissions
- Identify roles with AWSLambda_FullAccess or lambda:* permissions
- Include which department each role belongs to (from Department tag)

SUMMARY FOR CTO:
- Organize findings by department for easy reporting
- Highlight the most critical overprivileged roles
- Provide a count of total admin roles vs. full-access roles
:::

After the prompt executes, the security agent will:
1. Identify all roles and users with admin permissions

![](/static/images/iam-discovery-q1.png)

2. Detect full access to EC2, S3, and Lambda services

![](/static/images/iam-discovery-q2.png)

3. Organize findings by department for the CTO's report

![](/static/images/iam-discovery-q3.png)

4. Provide actionable insights for the compliance audit

![](/static/images/iam-discovery-q4.png)

::::

::::expand{header="Expected Discovery Findings" defaultExpanded=true}
Your analysis should reveal **2 admin roles**, **2 EC2 full-access roles**, **1 S3 full-access role**, **1 Lambda full-access role**, and **1 Lambda full-access user**. Findings should be organized by department: **Finance** (admin), **Operations** (admin), **Development** (EC2), **Testing** (EC2), **Marketing** (S3), **Engineering** (Lambda role), and **DevOps** (Lambda user).

**Answering the CTO's Questions:**

With your discovery complete, you should now be able to answer the CTO's urgent questions:

1. **"Which IAM roles and users currently have admin-level permissions?"** → You should have identified 2 roles with AdministratorAccess: `SMB306-Finance-AdminRole` and `SMB306-Operations-AdminRole`.

2. **"Do any of our roles have full access to our tech stack services?"** → You should have found multiple roles with full access: 2 roles with EC2 full access (Development, Testing), 1 role with S3 full access (Marketing), 1 role with Lambda full access (Engineering), and 1 user with Lambda full access (DevOps).

You're now ready to present a clear picture to the CTO: the organization has significant overprivileged access across multiple departments that needs immediate attention before the audit.
::::

**Checkpoint (10 minutes):** You should now have answers to both CTO questions, an IAM role and user inventory organized by department, and a foundation for remediation planning.

---

## Next Steps

With discovery complete, proceed to the **Investigation** phase to understand how these overprivileged roles were created and prepare for remediation.

