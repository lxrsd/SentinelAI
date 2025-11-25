---
title: "Planning"
weight: 33
---

## Scenario: IAM Least-Privilege Planning

After completing your IAM root-cause analysis, you receive this email with specific requirements:

:::alert{header="From: Judy Hopps <cto@company.com>" type="warning"}
**Subject: IAM Remediation Requirements - Specific Permissions Needed**

Team,

Great work finding out the root cause of these permissions! Now here's the thing - we DO need these roles, but with much more restrictive permissions.

After talking to each team, here's what they actually need:

1. **2 admin roles (Finance, Operations)** - They don't need full admin access. They only need Cost Explorer access, and only during work hours (8 AM PST - 5 PM PST).

2. **2 EC2 full-access roles (Development, Testing)** - They only need EC2 access for projects with their respective department tags on EC2 instances.

3. **1 S3 full-access role (Marketing)** - This can stay but I don't want them able to modify any S3 bucket policies or configurations.

4. **1 Lambda full-access role (Engineering)** - This can stay the same, but we actually need 2 MORE roles like this for additional engineering teams.

5. **1 Lambda full-access user (DevOps)** - We need this to only be available from December 1-31, 2025, and only allow access from our corporate office IP (1.1.1.1/32).

I need a detailed plan showing how we'll implement these changes safely without breaking anything.

Thanks,  
Judy Hopps  
CTO
:::

**Planning Goals:** Create a detailed remediation plan that implements least-privilege access for each role based on the CTO's specific requirements while ensuring business continuity.

---

## Using the Security Agent

:::alert{header="Agent Session" type="info"}
Continue working with your Kiro CLI security agent session from the Investigation phase. If you need to start a new session. Make sure you are in the right directory:
```bash
cd /home/ec2-user/SMB306/identity-access-management
```
Then resume your security agent session.
```bash
kiro-cli chat --agent security-agent --resume
```
:::

The security agent will use insights from your previous phases to create targeted remediation plans based on the CTO's requirements.

---

## Planning Exercise: Least-Privilege Remediation Strategy

**Objective:** Create a comprehensive remediation plan that implements the CTO's specific permission requirements for each role while maintaining business operations.

**Your Task:** Ask the security agent to generate a detailed remediation plan addressing each of the CTO's requirements. Your prompt should include:

- Specific policy changes for each role based on CTO requirements
- Time-based access controls for Finance/Operations roles
- Tag-based access controls for Development/Testing roles
- Read-only S3 access for Marketing role
- Creation of 2 additional Lambda roles for Engineering
- IP and date restrictions for DevOps user
- Implementation timeline and rollback procedures

::::expand{header="Step-by-step walk-through" defaultExpanded=true}
A detailed remediation plan addresses each of the CTO's specific requirements. Here's an example prompt:

:::code{showCopyAction=true showLinenumbers=false language=text}
I need to create a least-privilege remediation plan based on specific requirements from our CTO. Please help me design IAM policies for each role:

FINANCE & OPERATIONS ROLES (Admin → Cost Explorer):
- Remove AdministratorAccess policy
- Add Cost Explorer read-only access (ce:Get*, ce:Describe*, ce:List*)
- Implement time-based access: 8 AM PST - 5 PM PST only
- Use IAM condition keys for time restrictions (aws:CurrentTime)
- Provide policy JSON for both roles

DEVELOPMENT & TESTING ROLES (EC2 Full → Tag-Based):
- Remove AmazonEC2FullAccess policy
- Add EC2 access restricted to instances with Department tag matching role
- Development role: only EC2s tagged Department=Development
- Testing role: only EC2s tagged Department=Testing
- Use IAM condition keys for tag-based access (ec2:ResourceTag/Department)
- Provide policy JSON for both roles

MARKETING ROLE (S3 Full → Read-Only):
- Remove AmazonS3FullAccess policy
- Add S3 read-only access (s3:Get*, s3:List*)
- Explicitly deny S3 bucket policy modifications (s3:PutBucketPolicy, s3:DeleteBucketPolicy)
- Provide policy JSON

ENGINEERING LAMBDA ROLES (Keep + Add 2 More):
- Keep existing SMB306-Engineering-LambdaRole with AWSLambda_FullAccess
- Create 2 new roles: SMB306-Engineering-LambdaRole-Team2 and SMB306-Engineering-LambdaRole-Team3
- All 3 roles should have identical Lambda full access permissions
- Provide CloudFormation template for new roles

DEVOPS USER (Lambda Full → Time & IP Restricted):
- Keep AWSLambda_FullAccess policy
- Add condition: only accessible December 1-31, 2025
- Add condition: only from corporate IP 1.1.1.1/32
- Use IAM condition keys (aws:CurrentTime, aws:SourceIp)
- Provide policy JSON with both conditions

IMPLEMENTATION PLAN:
- Testing procedures for each policy change
- Rollback procedures if issues arise
- Monitoring and validation steps
:::

After the prompt executes, the security agent will:
1. Generate specific IAM policy JSON for each role

![](/static/images/iam-planning-q1.png)

2. Provide CloudFormation templates for new resources

![](/static/images/iam-planning-q2.png)

3. Create an implementation timeline with safety checks

![](/static/images/iam-planning-q3.png)

4. Include rollback procedures for each change

![](/static/images/iam-planning-q4.png)

::::

::::expand{header="Expected Planning Outcomes" defaultExpanded=true}
Your remediation plan should include **specific IAM policies** for each role addressing the CTO's requirements. **Finance/Operations roles** get Cost Explorer access with time-based conditions (8 AM-5 PM PST). **Development/Testing roles** get tag-based EC2 access using resource tags. **Marketing role** gets S3 read-only with explicit deny on bucket policies. **Engineering** keeps existing role plus 2 new identical roles. **DevOps user** gets time-restricted (December 2025) and IP-restricted (1.1.1.1/32) access.

**Key Policy Components:**

**Time-Based Access** (Finance/Operations):
```json
"Condition": {
  "StringLike": {
    "aws:CurrentTime": ["*T08:00:00Z/*", "*T17:00:00Z"]
  }
}
```

**Tag-Based Access** (Development/Testing):
```json
"Condition": {
  "StringEquals": {
    "ec2:ResourceTag/Department": "Development"
  }
}
```

**Read-Only with Deny** (Marketing):
```json
"Effect": "Deny",
"Action": ["s3:PutBucketPolicy", "s3:DeleteBucketPolicy"]
```

**Date and IP Restrictions** (DevOps):
```json
"Condition": {
  "DateGreaterThan": {"aws:CurrentTime": "2025-12-01T00:00:00Z"},
  "DateLessThan": {"aws:CurrentTime": "2025-12-31T23:59:59Z"},
  "IpAddress": {"aws:SourceIp": "1.1.1.1/32"}
}
```

**Implementation Timeline:**
- **Day 1**: Finance/Operations roles (Cost Explorer + time restrictions)
- **Day 2**: Development/Testing roles (tag-based EC2 access)  
- **Day 3**: Marketing role (S3 read-only)
- **Day 4**: Engineering new roles (2 additional Lambda roles)
- **Day 5**: DevOps user (time + IP restrictions)
- **Day 6**: Final validation and monitoring setup

**Note**: For this workshop, you'll implement all changes in a single CloudFormation stack update rather than phasing over multiple days.
::::

**Checkpoint (25 minutes):** You should now have a detailed remediation plan with specific IAM policies for each role and rollback procedures.

## Next Steps

With your detailed remediation plan ready, you're prepared to move to the **Remediation** phase where you'll implement these least-privilege policies and validate they work as expected.

