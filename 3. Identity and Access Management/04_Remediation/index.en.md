---
title: "Remediation"
weight: 34
---

## Scenario: Executing IAM Least-Privilege Remediation

:::alert{header="From: Judy Hopps <cto@company.com>" type="success"}
**Subject: Execute IAM Remediation Plan - Let's Do This!**

Team,

Your remediation plan looks great! Let's implement these least-privilege changes and get our IAM house in order before the audit.

Execute the plan we discussed:
1. **Finance/Operations** - Cost Explorer only, work hours only
2. **Development/Testing** - Tag-based EC2 access
3. **Marketing** - S3 read-only
4. **Engineering** - Create 2 additional Lambda roles
5. **DevOps User** - December 2025 + IP restrictions

Test each change thoroughly and keep me updated on progress. I want to make sure nothing breaks!

Thanks,  
Judy Hopps  
CTO
:::

**The Execution:** You'll implement the specific least-privilege policies for each role based on the CTO's requirements, test them thoroughly, and validate business operations continue normally.

**Success Criteria:** All 5 role types updated with least-privilege access, 2 new Engineering roles created, and all changes validated with zero business disruption.

---

## Using the Security Agent

:::alert{header="Agent Session" type="info"}
Continue working with your Kiro CLI security agent session from the Planning phase. If you need to start a new session. Make sure you are in the right directory:
```bash
cd /home/ec2-user/SMB306/identity-access-management
```
Then resume your security agent session.
```bash
kiro-cli chat --agent security-agent --resume
```
:::

The security agent will help generate IAM policies and CloudFormation templates based on your approved remediation plan.

---

## Remediation Workflow

**Important:** This remediation will be implemented by **updating the existing CloudFormation template** (`SMB306/rivsmb306-identity-access-management.yaml`) in your VS Code Server and redeploying the stack. 

**Workflow Steps:**
1. Use Kiro CLI to generate updated IAM policies
2. Update the CloudFormation template with new policies
3. Deploy the updated stack using CloudFormation
4. Document changes for audit purposes

---

## Remediation Exercise: Implementing Least-Privilege Policies

**Objective:** Execute the remediation plan by updating the CloudFormation template with least-privilege policies for each role and creating the additional Engineering roles.

**Your Task:** Create a prompt that asks the security agent to help generate the updated CloudFormation template. Your prompt should include:

- Reference to the 5 specific requirements from the Planning phase
- Request for updated CloudFormation YAML with new IAM policies
- Ask for the 2 new Engineering roles to be added to the template
- Ask for CloudFormation deployment commands

::::expand{header="Step-by-step walk-through" defaultExpanded=true}
A systematic approach using CloudFormation ensures safe, repeatable implementation. Here's an example prompt:

:::code{showCopyAction=true showLinenumbers=false language=text}
I need to update the CloudFormation template (SMB306/rivsmb306-identity-access-management.yaml) to implement our IAM remediation plan. Please help me generate the updated template:

CLOUDFORMATION TEMPLATE UPDATES:

FINANCE/OPERATIONS ROLES (Cost Explorer + Time Restriction):
- Replace AdministratorAccess with inline policy for Cost Explorer
- Add time-based condition for 8 AM - 5 PM PST (aws:CurrentTime)
- Policy should include: ce:Get*, ce:Describe*, ce:List*
- Update both SMB306-Finance-AdminRole and SMB306-Operations-AdminRole

DEVELOPMENT/TESTING ROLES (Tag-Based EC2 Access):
- Replace AmazonEC2FullAccess with inline policy for tag-based access
- Development role: condition ec2:ResourceTag/Department = "Development"
- Testing role: condition ec2:ResourceTag/Department = "Testing"
- Include EC2 actions: Describe*, Start*, Stop*, Reboot*, Terminate*
- Update SMB306-Development-EC2Role and SMB306-Testing-EC2Role

MARKETING ROLE (S3 Read-Only):
- Replace AmazonS3FullAccess with inline policy for read-only
- Include S3 actions: s3:Get*, s3:List*
- Add explicit deny for: s3:PutBucketPolicy, s3:DeleteBucketPolicy
- Update SMB306-Marketing-S3Role

ENGINEERING ROLES (Create 2 Additional):
- Keep existing SMB306-Engineering-LambdaRole unchanged
- Add new role: SMB306-Engineering-LambdaRole-Team2
- Add new role: SMB306-Engineering-LambdaRole-Team3
- All 3 roles should have AWSLambda_FullAccess
- Include proper tags (Department: Engineering)

DEVOPS USER (Time + IP Restrictions):
- Keep AWSLambda_FullAccess for SMB306-DevOps-LambdaUser
- Add inline policy with conditions:
  - Date range: December 1-31, 2025 (aws:CurrentTime)
  - IP restriction: 1.1.1.1/32 (aws:SourceIp)

DEPLOYMENT INSTRUCTIONS:
- Provide the complete updated CloudFormation YAML
- Include AWS CLI command to update the stack
- Provide testing steps for each policy change
- Include rollback command if issues occur

Generate the complete updated CloudFormation template with all policy changes.
:::

After the prompt executes, the security agent will:
1. Generate the complete updated CloudFormation template

![](/static/images/iam-remediation-q1.png)

2. Include all inline policies with proper conditions

![](/static/images/iam-remediation-q2.png)

3. Add the 2 new Engineering roles

![](/static/images/iam-remediation-q3.png)

4. Provide CloudFormation deployment commands

![](/static/images/iam-remediation-q4.png)

5. Include testing and rollback procedures

::::

**Deployment Steps:**
1. Open the template in VS Code: `SMB306/rivsmb306-identity-access-management.yaml`
2. Update the template with the generated policies
3. Deploy using: `aws cloudformation update-stack --stack-name rivsmb306-identity-access-management --template-body file://SMB306/rivsmb306-identity-access-management.yaml --capabilities CAPABILITY_NAMED_IAM`
4. Monitor deployment: `aws cloudformation describe-stack-events --stack-name rivsmb306-identity-access-management`
::::

**Checkpoint (30 minutes):** You should have successfully implemented all least-privilege policies, created the new Engineering roles, and validated that all changes work as expected without business disruption.

---

## Next Steps

With your IAM remediation complete and validated, you're ready to move to the **Prevention** phase where you'll implement continuous monitoring and automated guardrails to prevent future IAM privilege creep.

