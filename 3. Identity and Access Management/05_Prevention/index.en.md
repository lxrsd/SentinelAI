---
title: "Prevention"
weight: 35
---

## Scenario: Evaluating Long-Term IAM Prevention Options

With remediation complete and least-privilege policies successfully deployed, you receive this company-wide memo from the CTO:

:::alert{header="From: Judy Hopps <cto@company.com>" type="info"}
**Subject: Company-Wide Memo - IAM Prevention Strategy Needed**

Team,

Great work on the IAM remediation! The audit went smoothly and we're now compliant.

However, I want to make sure we don't end up in the same situation again. I need your team to evaluate different options for **long-term IAM monitoring and prevention** and recommend the best approach for our organization.

**What I need:**
- Automatic detection when IAM permissions change on our remediated roles
- Alerts when someone tries to add admin permissions back
- Monitoring for removal of our time-based, tag-based, or IP-based conditions
- A solution that scales as we grow

**Your task:**
Research the available AWS services and approaches for IAM monitoring, then recommend the best solution for our needs. I want to understand the pros/cons of each option before we implement anything company-wide.

Present your findings and recommendation by end of week.

Thanks,  
Judy Hopps  
CTO
:::

**The Objective:** Research and evaluate different AWS approaches for IAM monitoring and prevention, then recommend and implement the best solution for the organization.

**Success Criteria:** Understand the available options, make an informed recommendation, and implement automated IAM monitoring that prevents privilege creep.

---

## Using the Security Agent

:::alert{header="Agent Session" type="info"}
Continue working with your Kiro CLI security agent session from the Remediation phase. If you need to start a new session. Make sure you are in the right directory:
```bash
cd /home/ec2-user/SMB306/identity-access-management
```
Then resume your security agent session.
```bash
kiro-cli chat --agent security-agent --resume
```
:::

The security agent can help you research AWS services, compare approaches, and generate implementation code for your chosen solution.

---

## Prevention Exercise: Research and Recommend IAM Monitoring Approach

**Objective:** Research available AWS services for IAM monitoring, evaluate their pros/cons, and recommend the best approach for long-term prevention.

**Your Task:** Ask the security agent to help you research and compare different AWS approaches for monitoring IAM changes. Your prompt should request:

- Overview of available AWS services for IAM monitoring
- Comparison of approaches (CloudWatch Events, AWS Config, Lambda, etc.)
- Pros and cons of each approach
- Cost considerations
- Complexity and maintenance requirements
- Recommendation for this specific use case

::::expand{header="Step-by-step walk-through" defaultExpanded=true}
A comprehensive research prompt helps you understand all available options. Here's an example:

:::code{showCopyAction=true showLinenumbers=false language=text}
I need to research and recommend a long-term IAM monitoring solution for our organization. We want to detect when IAM permissions change on our remediated roles and alert when someone tries to add admin permissions or remove our conditions. Please help me:

RESEARCH AWS OPTIONS:
- What AWS services can monitor IAM permission changes?
- How does CloudWatch Events/EventBridge work for IAM monitoring?
- How does AWS Config work for IAM compliance?
- How does CloudFormation Drift Detection work?
- Can Lambda be used for custom IAM validation?

COMPARE APPROACHES:
For each approach, explain:
- How it works (high-level architecture)
- What it can detect (API calls vs. policy content)
- Real-time vs. periodic monitoring
- Setup complexity (simple vs. complex)
- Ongoing maintenance requirements
- Cost implications (free tier, per-evaluation, etc.)
- Scalability considerations

SPECIFIC USE CASES:
How would each approach handle:
- Detecting when AdministratorAccess is added to a role
- Alerting when time-based conditions are removed
- Monitoring for removal of tag-based or IP-based restrictions
- Detecting new roles created without proper controls

COMPARISON SUMMARY:
Create a comparison table or summary showing:
- Approach name
- Real-time vs. periodic
- Detects API calls vs. policy content
- Setup complexity (1-5 scale)
- Ongoing maintenance (1-5 scale)
- Estimated monthly cost
- Best use case

Help me understand the trade-offs so I can make an informed decision for our organization.
:::

After the prompt executes, the security agent will:
1. Explain available AWS services for IAM monitoring

![](/static/images/iam-prevention-q1.png)

2. Compare approaches with detailed pros/cons

![](/static/images/iam-prevention-q2.png)

3. Provide cost and complexity analysis

![](/static/images/iam-prevention-q3.png)

4. Present comparison summary for decision-making

![](/static/images/iam-prevention-q4.png)

5. Help you understand trade-offs without prescribing a solution

![](/static/images/iam-prevention-q5.png)

::::

::::expand{header="Expected Research Findings" defaultExpanded=true}
Your research should reveal **4 main approaches** for IAM monitoring in AWS:

**Option 1: CloudWatch Events + SNS**
- **How it works**: EventBridge monitors CloudTrail for IAM API calls, triggers SNS notifications
- **Detects**: API calls (PutRolePolicy, AttachRolePolicy, etc.)
- **Pros**: Simple, real-time, no extra infrastructure, free tier available
- **Cons**: Detects changes but doesn't validate policy content
- **Best for**: Simple change detection, immediate alerts

**Option 2: AWS Config Rules**
- **How it works**: Continuously evaluates IAM resources against compliance rules
- **Detects**: Policy content, compliance violations, configuration drift
- **Pros**: Validates actual policies, compliance history, managed rules available
- **Cons**: More complex, costs per evaluation, not real-time (periodic)
- **Best for**: Compliance validation, policy content checking

**Option 3: CloudFormation Drift Detection**
- **How it works**: Compares current IAM state vs. CloudFormation template
- **Detects**: Any changes made outside CloudFormation
- **Pros**: Perfect for IaC approach, detects all drift
- **Cons**: Requires scheduled Lambda, not real-time, only for CFN-managed resources
- **Best for**: Infrastructure-as-code environments

**Option 4: Lambda + CloudWatch Events + SNS (Hybrid)**
- **How it works**: EventBridge triggers Lambda on IAM changes, Lambda validates policy, SNS alerts
- **Detects**: API calls AND policy content validation
- **Pros**: Most flexible, real-time, custom validation logic, detailed alerts
- **Cons**: Requires Lambda development, more complex to maintain
- **Best for**: Custom validation requirements, detailed monitoring

**Your Decision:**

Now that you understand the available options, **choose the approach that best fits your organization's needs**. Consider:
- Your technical team's capabilities
- Budget constraints  
- Need for real-time vs. periodic monitoring
- Complexity you're willing to manage
- Whether you need policy content validation or just change detection

**Need help deciding?** Ask the security agent to help you evaluate which approach best fits your specific situation based on the trade-offs you've learned.

**All approaches are valid** - the "best" choice depends on your specific requirements and constraints. In the next exercise, you'll implement your chosen approach.
::::

**Checkpoint (15 minutes):** You should now understand the available AWS options for IAM monitoring and be ready to choose the best approach for your organization.

---

## Implementation Exercise: Deploy Your Chosen IAM Monitoring Solution

**Objective:** Implement your chosen monitoring solution using CloudFormation or AWS Console to automate IAM change detection and alerting.

**Your Task:** Ask the security agent to help you implement your chosen monitoring approach. Your prompt should request:

- Implementation guide for your chosen approach
- CloudFormation template or Console setup instructions
- Any necessary code (Lambda functions, Config rules, etc.)
- Configuration for alerts and notifications
- Testing procedures to validate it works

::::expand{header="Step-by-step walk-through" defaultExpanded=true}
Implementation prompts for each approach. **Choose the one that matches your decision:**

**If you chose Option 1 (CloudWatch Events + SNS):**

:::code{showCopyAction=true showLinenumbers=false language=text}
I want to implement CloudWatch Events + SNS for IAM monitoring. Please help me create:

CLOUDFORMATION TEMPLATE:
1. SNS Topic for alerts (IAM-Compliance-Alerts)
2. EventBridge Rule monitoring IAM API calls:
   - PutRolePolicy, AttachRolePolicy, DetachRolePolicy
   - DeleteRolePolicy, UpdateAssumeRolePolicy, CreateRole
3. Filter for SMB306-* roles only
4. Email subscription to SNS topic

TESTING:
- How to test by making IAM changes
- How to verify alerts are received
- What information is included in alerts

Provide complete CloudFormation template and deployment instructions.
:::

**If you chose Option 2 (AWS Config Rules):**

:::code{showCopyAction=true showLinenumbers=false language=text}
I want to implement AWS Config Rules for IAM monitoring. Please help me create:

CLOUDFORMATION TEMPLATE:
1. AWS Config Configuration Recorder
2. S3 Bucket for Config data
3. Config Rules for IAM compliance:
   - iam-policy-no-statements-with-admin-access
   - Custom rule for time-based conditions
   - Custom rule for tag-based conditions
4. SNS Topic for compliance notifications
5. Email subscription

TESTING:
- How to trigger Config evaluations
- How to view compliance status
- How to test with policy changes

Provide complete CloudFormation template and deployment instructions.
:::

**If you chose Option 3 (CloudFormation Drift Detection):**

:::code{showCopyAction=true showLinenumbers=false language=text}
I want to implement CloudFormation Drift Detection for IAM monitoring. Please help me create:

CLOUDFORMATION TEMPLATE:
1. Lambda function to run drift detection on schedule
2. EventBridge rule to trigger Lambda daily
3. SNS Topic for drift notifications
4. IAM role for Lambda with drift detection permissions
5. Email subscription

LAMBDA CODE:
- Detect drift on rivsmb306-identity-access-management stack
- Parse drift results for IAM resources
- Send detailed alerts if drift detected

TESTING:
- How to manually trigger drift detection
- How to make changes to test drift
- How to view drift details

Provide complete CloudFormation template, Lambda code, and deployment instructions.
:::

**If you chose Option 4 (Hybrid: Lambda + CloudWatch Events + SNS):**

:::code{showCopyAction=true showLinenumbers=false language=text}
I want to implement the hybrid CloudWatch Events + Lambda + SNS approach for IAM monitoring. Please help me create:

CLOUDFORMATION TEMPLATE:
Create a complete CloudFormation template that includes:

1. SNS Topic for alerts (IAM-Compliance-Alerts)
2. EventBridge Rule monitoring these IAM API calls:
   - PutRolePolicy (inline policy changes)
   - AttachRolePolicy (managed policy attachments)
   - DetachRolePolicy (managed policy removals)
   - DeleteRolePolicy (inline policy deletions)
   - UpdateAssumeRolePolicy (trust policy changes)
   - CreateRole (new role creation)
3. Lambda function for policy validation
4. IAM role for Lambda with necessary permissions
5. Lambda permission for EventBridge to invoke

LAMBDA FUNCTION CODE:
The Lambda should:
- Parse the CloudTrail event from EventBridge
- Extract the role name and policy changes
- Validate if changes affect our SMB306 roles
- Check for:
  * Admin permission additions (AdministratorAccess, iam:*, ec2:*, s3:*, lambda:*)
  * Removal of time-based conditions (aws:CurrentTime)
  * Removal of tag-based conditions (ec2:ResourceTag/Department)
  * Removal of IP-based conditions (aws:SourceIp)
- Send detailed alert to SNS with:
  * What changed
  * Which role was affected
  * Who made the change
  * Timestamp
  * Recommended remediation action

TESTING PROCEDURES:
Provide steps to test the monitoring:
1. How to subscribe to SNS topic for email alerts
2. Test by adding AdministratorAccess to a role
3. Test by removing a condition from a policy
4. Verify alerts are received with correct details

DEPLOYMENT:
- Provide CloudFormation deployment commands
- Include validation steps
- Document how to view Lambda logs for troubleshooting

Generate the complete monitoring solution as a CloudFormation template.
:::

After the prompt executes, the security agent will:
1. Generate complete CloudFormation template

![](/static/images/iam-prevention-qq1.png)

2. Provide Lambda function code with validation logic

![](/static/images/iam-prevention-qq2.png)

3. Include deployment and testing procedures

![](/static/images/iam-prevention-qq3.png)

4. Document how to subscribe to alerts

![](/static/images/iam-prevention-qq4.png)

::::

::::expand{header="Expected Implementation Results" defaultExpanded=true}
Your implementation should create a **complete IAM monitoring system** appropriate for your chosen approach. Regardless of which option you selected, you should have:

**Core Components (all approaches):**
- ✅ **SNS Topic** for alerts and notifications
- ✅ **Email subscription** to receive alerts
- ✅ **Monitoring** for IAM changes on SMB306 roles
- ✅ **Testing** validated the system works correctly

**Approach-Specific Components:**

**If you chose CloudWatch Events + SNS:**
- EventBridge rule monitoring IAM API calls
- Immediate alerts when changes occur
- Simple architecture with minimal components

**If you chose AWS Config Rules:**
- Config recorder and S3 bucket for data
- Config rules evaluating IAM compliance
- Compliance dashboard showing status
- Periodic evaluations with compliance history

**If you chose CloudFormation Drift Detection:**
- Lambda function running scheduled drift detection
- EventBridge rule triggering daily checks
- Drift reports showing changes outside CloudFormation
- Stack-focused monitoring approach

**If you chose Hybrid (Lambda + Events + SNS):**
- EventBridge rule monitoring IAM API calls
- Lambda function validating policy content
- Custom validation logic for conditions
- Detailed alerts with remediation steps

**Testing Results (all approaches):**
- ✅ Alert/notification received when IAM changes made
- ✅ Monitoring correctly filters for SMB306 roles
- ✅ Alerts include relevant information (what, who, when)
- ✅ No false positives from non-SMB306 changes
- ✅ System scales to monitor additional roles

**Validation Checklist:**
- ✅ CloudFormation stack deployed successfully (or Console setup complete)
- ✅ All components created and configured
- ✅ SNS topic has email subscription confirmed
- ✅ Test changes trigger appropriate alerts/notifications
- ✅ Monitoring system operational and scalable
::::

**Checkpoint (30 minutes):** You should have a fully functional IAM monitoring system based on your chosen approach that automatically detects and alerts on policy changes to your remediated roles.

---

## Module Completion: IAM Governance Mastery Achieved

Congratulations! You have completed the full IAM governance lifecycle:

1. ✅ **Discovery**: Identified 7 overprivileged IAM entities
2. ✅ **Investigation**: Understood how they were created and their usage
3. ✅ **Planning**: Designed least-privilege policies for each role
4. ✅ **Remediation**: Implemented time-based, tag-based, and IP-based restrictions
5. ✅ **Prevention**: Researched options and deployed automated monitoring

:::alert{header="IAM Governance Mastery" type="success"}
You've successfully transformed from reactive IAM management to proactive, automated governance. You researched multiple AWS approaches, made an informed decision, and implemented a scalable monitoring solution that maintains least-privilege access without manual intervention.
:::

## Next Steps
With your IAM prevention framework in place, you've built organizational resilience that extends beyond this specific module. The skills you've developed - researching options, making informed decisions, and implementing automated solutions - can be applied to any AWS security domain.

