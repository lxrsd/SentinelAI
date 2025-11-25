---
title: Kiro CLI Custom Agent with MCP
weight: 17
---

In this section, you will create a custom security agent for Kiro CLI. This specialized agent is designed for security investigations, compliance reviews, and architectural guidance, with a focus on small and medium-sized business environments. We will also setup Model Context Protocol (MCP) for Kiro CLI. MCP allows Kiro to connect to external tools and services, significantly enhancing its capabilities for security analysis and AWS resource management during this workshop.

:::alert{header="Prerequisites" type="info"}
Before proceeding, ensure you have completed the previous sections:
- Kiro CLI Authentication Setup
:::

## Understanding Custom Agents

Kiro CLI custom agents are specialized AI assistants that can be configured with specific prompts, tools, and capabilities. The security agent we'll create is designed to:

- Conduct security investigations using AWS security services
- Perform Well-Architected Framework security reviews
- Provide SMB-focused security recommendations
- Generate comprehensive security reports and remediation guidance

## MCP Servers for Security Analysis

Model Context Protocol (MCP) servers are standardized interfaces that allow AI assistants to connect to external tools, data sources, and services in a secure and controlled manner. They enable Kiro CLI to access real-time AWS security data, perform complex analysis across multiple services, and execute specialized security operations that would otherwise require manual integration and custom tooling.

The configuration includes several MCP servers that enhance Kiro CLI's security capabilities:

- **[AWS Well-Architected Security Assessment Tool](https://awslabs.github.io/mcp/servers/well-architected-security-mcp-server/)**: AWS security best practices and framework compliance
- **[AWS Documentation](https://awslabs.github.io/mcp/servers/aws-documentation-mcp-server)**: MCP server that provides AWS documentation, search for content, and get recommendations.
- **[AWS Knowledge Server](https://awslabs.github.io/mcp/servers/aws-knowledge-mcp-server/)**: MCP server that provides up-to-date documentation, code samples, and other official AWS content.
- **[AWS Core Server](https://awslabs.github.io/mcp/servers/core-mcp-server/)**: MCP server that provides a starting point for using AWS MCP servers through a dynamic proxy server strategy based on role-based environment variables.
- **[AWS API Server](https://awslabs.github.io/mcp/servers/aws-api-mcp-server/)**: MCP Server enables AI assistants to interact with AWS services and resources through AWS CLI commands. It provides programmatic access to manage your AWS infrastructure while maintaining proper security controls.
- **[AWS IAM Server](https://awslabs.github.io/mcp/servers/iam-mcp-server/)**: Specialized IAM policy analysis and privilege management
- **[AWS CloudTrail Server](https://awslabs.github.io/mcp/servers/cloudtrail-mcp-server/)**: Security event analysis and audit trail investigation
- **[AWS CloudFormation Server](https://awslabs.github.io/mcp/servers/cfn-mcp-server/)**: Infrastructure-as-Code security analysis

## Create Custom Agent Directory

First, we'll create the directory structure for each adventure: and set up the working environment.

1. Create the working directory structure for each adventure:

:::code{showCopyAction=true showLinenumbers=false language=bash}
mkdir -p /home/ec2-user/SMB306/infrastructure-protection
mkdir -p /home/ec2-user/SMB306/identity-access-management
mkdir -p /home/ec2-user/SMB306/application-security
mkdir -p /home/ec2-user/SMB306/data-security
:::

2. Verify the directories were created:

:::code{showCopyAction=true showLinenumbers=false language=bash}
ls -la /home/ec2-user/SMB306/
:::

## Create the Security Agent Configuration

Now we'll create the security agent configuration file with specialized security capabilities.

1. Create the security agent configuration file:

:::code{showCopyAction=true showLinenumbers=false language=bash}
cat > ~/.kiro/agents/security-agent.json << 'EOF' 
{
  "name": "security-agent",
  "description": "Security agent for investigations, compliance reviews, and security architectural guidance using read-only operations",
  "prompt": "You are a security agent that conducts security investigations, compliance reviews, and provides security architectural guidance using MCP tools and read-only AWS operations. Your role is to analyze security posture, investigate incidents, and document findings with specific focus on small and medium-sized business environments.\n\nWORKING DIRECTORY: Always store all created files, reports, and artifacts in /home/ec2-user/SMB306/ directory using the adventure-specific subdirectories:\n- /home/ec2-user/SMB306/infrastructure-protection/ - Infrastructure security analysis and findings\n- /home/ec2-user/SMB306/identity-access-management/ - IAM policies, roles, and access reviews\n- /home/ec2-user/SMB306/application-security/ - Application security assessments and reports\n- /home/ec2-user/SMB306/data-security/ - Data protection and encryption analysis\n\nREMEDIATION POLICY:\n- ONLY provide remediation guidance when explicitly asked (e.g., 'How do I fix this?', 'What's the remediation?', 'How should I resolve this?')\n- Focus on analysis, investigation, and documentation by default\n- Present security findings objectively without unsolicited remediation steps\n- When remediation IS requested, tailor guidance for SMB environments with limited resources\n- Consider implementation costs, team size, and technical capabilities when providing remediation\n- Prioritize managed services and solutions that provide maximum security benefit with minimal operational overhead\n\nYour core responsibilities:\n1. Use MCP tools for security investigations and well-architected reviews\n2. Conduct security investigations using CloudTrail and security service findings\n3. Use Security Hub/CSPM as primary source of truth when enabled, fallback to individual services\n4. Analyze and document security posture with factual findings\n5. Generate comprehensive reports documenting security findings\n6. Identify security gaps and compliance issues\n7. Document security architectural analysis\n\nSecurity Investigation Process:\n- Use Security Hub for centralized findings when available\n- Query individual services (Inspector, GuardDuty, Macie) when Security Hub unavailable\n- Analyze CloudTrail logs for incident investigation and compliance auditing\n- Correlate findings across multiple security services for comprehensive analysis\n- Research security best practices using AWS knowledge tools\n- Document findings clearly and objectively\n\nWell-Architected Security Reviews:\n- Use well-architected MCP server to conduct security pillar assessments\n- Evaluate current architecture against security best practices\n- Identify security gaps and document findings with supporting evidence\n- Only provide remediation steps when explicitly requested\n\nDeliverable Requirements:\n- Create detailed markdown reports with findings and analysis in the appropriate adventure subdirectory\n- Generate shell scripts for remediation ONLY when remediation is explicitly requested\n- Provide step-by-step remediation guidance ONLY when asked\n- Include compliance mapping and risk assessments as factual analysis\n- Store all artifacts in /home/ec2-user/SMB306/ using the adventure-specific subdirectories\n- When remediation is requested, include estimated implementation costs and resource requirements\n\nIMPORTANT BOUNDARIES:\n- Use ONLY read-only AWS operations and MCP tools\n- DO NOT modify AWS resources, configurations, or security settings\n- Focus on analysis, investigation, and documentation by default\n- Provide remediation guidance ONLY when explicitly requested\n- Present findings objectively without assuming the user wants remediation steps\n- ALWAYS store files in /home/ec2-user/SMB306/ directory structure using adventure-specific subdirectories\n- Ask clarifying questions if the user's intent regarding remediation is unclear",
  "mcpServers": {
    "well-architected-security-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.well-architected-security-mcp-server@latest"],
      "env": {},
      "timeout": 120000,
      "disabled": false
    },
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "AWS_DOCUMENTATION_PARTITION": "aws",
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "timeout": 120000,
      "disabled": false
    },
    "aws-knowledge-mcp-server": {
      "command": "uvx",
      "args": [
        "mcp-proxy",
        "--transport",
        "streamablehttp",
        "https://knowledge-mcp.global.api.aws"
      ]
    },
    "awslabs.core-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.core-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "timeout": 120000,
      "disabled": false
    },
    "awslabs.aws-api-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-api-mcp-server@latest"],
      "env": {
        "AWS_REGION": "us-east-1"
      },
      "timeout": 120000,
      "disabled": false
    },
    "awslabs.cfn-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cfn-mcp-server@latest"],
      "env": {},
      "disabled": false,
      "autoApprove": []
    },
    "awslabs.iam-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "awslabs.cloudtrail-mcp-server": {
      "autoApprove": [],
      "disabled": false,
      "command": "uvx",
      "args": ["awslabs.cloudtrail-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "transportType": "stdio"
    }
  },
  "tools": [
    "*"
  ],
  "toolAliases": {},
  "allowedTools": [
    "fs_read",
    "fs_write",
    "execute_bash",
    "@well-architected-security-mcp-server",
    "@awslabs.aws-documentation-mcp-server",
    "@aws-knowledge-mcp-server",
    "@awslabs.core-mcp-server",
    "@awslabs.aws-api-mcp-server",
    "@awslabs.cfn-mcp-server",
    "@awslabs.iam-mcp-server",
    "@awslabs.cloudtrail-mcp-server"
  ],
  "resources": [
    "file:///home/ec2-user/environment/README.md",
    "file:///home/ec2-user/environment/.amazonq/rules/**/*.md"
  ],
  "hooks": {
    "agentSpawn": [],
    "userPromptSubmit": []
  },
  "toolsSettings": {
    "execute_bash": {
      "alwaysAllow": [
        {
          "preset": "readOnly"
        }
      ]
    }
  },
  "useLegacyMcpJson": false
}
EOF
:::

2. Verify the configuration file was created correctly:

:::code{showCopyAction=true showLinenumbers=false language=bash}
cat ~/.kiro/agents/security-agent.json | python3 -m json.tool
:::

This command will validate the JSON syntax and display the formatted configuration.

## Understanding the Security Agent Prompt

The agent's prompt configures it as a specialized security analyst that:

- Conducts security investigations and compliance reviews
- Focuses on analysis and documentation of security findings
- Uses read-only operations for safe analysis
- Provides remediation guidance only when explicitly requested
- Generates comprehensive security reports and documentation


## Test the Security Agent

Now let's test that the security agent is properly configured and accessible.

1. List available Kiro CLI agents:

:::code{showCopyAction=true showLinenumbers=false language=bash}
kiro-cli agent list
:::

You should see the `security-agent` listed among available agents.

2. Start the security agent:

:::code{showCopyAction=true showLinenumbers=false language=bash}
kiro-cli --agent security-agent
:::

As the security agent loads, you might notice that some mcp servers are loading in the background! This is because of our configuration file that we setup earlier. You might see some similar to the following:

![](/static/images/kiro-agent-mcp-load.png)

3. Verifying all MCP servers have uploaded

After a minute or two you should be able to run the following command indicating all the mcp servers have loaded correctly

:::code{showCopyAction=true showLinenumbers=false language=bash}
/mcp
:::

![](/static/images/q-agent-mcp-load-complete.png)

4. Test the Security Agent

:::code{showCopyAction=true showLinenumbers=false language=bash}
What capabilities do you have?
:::

You should see an answer similar to the follwing:

![](/static/images/security-agent-initial-prompt.png)

## Understanding the Security Agent Configuration

The security agent is configured with several key components:

### **Specialized Security Prompt**
The agent is programmed with expertise in:
- Security investigations and incident response
- AWS Well-Architected Framework security pillar
- Security analysis and documentation
- Compliance and audit support
- Remediation guidance delivery only when explicitly requested

### **Working Directory Configuration**
The agent is configured to store all files in `/home/ec2-user/SMB306/` with organized subdirectories for each adventure as described above.

## Wrap Up

You have successfully created a custom security agent for Kiro CLI. In the next sections, you will use this security agent to analyze real AWS environments and identify security improvements.