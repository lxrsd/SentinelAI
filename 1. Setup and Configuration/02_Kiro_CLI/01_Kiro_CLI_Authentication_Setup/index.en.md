---
title: Kiro CLI Authentication Setup
weight: 16

---

In this section, you will set up authentication for the new Kiro CLI. Kiro CLI uses AWS Builder ID for authentication, which allows access to certain developer tools and services from AWS without requiring an AWS account. AWS Builder IDs are free to use. Users only pay for the AWS resources used in their accounts.

## Using Kiro CLI in This Workshop

:::alert{header="Kiro CLI" type="info"}
**Note:** Kiro CLI was announced on November 17, 2025, as the next generation of Amazon Q Developer CLI. This workshop has been updated to use Kiro CLI, which provides enhanced AI-powered development capabilities with improved integration for security workflows and MCP server support while maintaining the same authentication model using AWS Builder ID.
:::

Kiro CLI offers command-line access that provides direct terminal access to Kiro's AI capabilities through commands like `kiro-cli` for security discussions and troubleshooting. This terminal-focused approach is pre-configured with AWS Builder ID authentication and ready for immediate use in your security workflows.

## Set up Kiro CLI with AWS Builder ID

Kiro CLI provides a command-line interface to interact with Kiro, allowing you to leverage AI assistance directly from your terminal.

## Logging into Kiro CLI

1. Open the VSCode terminal by navigating to the upper menu, selecting **Terminal** and **New Terminal** in the drop down bar.

2. In your terminal, navigate to your project directory and run:

:::code{showCopyAction=true showLinenumbers=false language=bash}
kiro-cli login --use-device-flow
:::

3. You will be presented with login options:

![](/static/images/kiro-cli-login-options.png)

4. Select **Use with Builder ID** using the arrow keys and press Enter.

5. You will be prompted to open a URL. Open the URL in a new tab:

![](/static/images/kiro-cli-login-credentials.png)

6. Utilize the login information provided to you from your AWS employee (applicable for re\:Invent 2025)

![](/static/images/kiro-cli-login-credentials-email.png)

7. A browser tab will open displaying the **Authorization requested** page. The confirmation code should already be populated. Choose **Confirm and continue**.

![](/static/images/kiro-cli-login-credentials-one-time-password.png)

8. Select **Allow access** on the next screen:

![](/static/images/kiro-cli-login-access.png)

9. After successful authentication, you will see a **Request approved** message:

![](/static/images/kiro-cli-login-approved.png)

10. Return to your terminal, which should now show that you are successfully logged in.

## Verifying Your Kiro CLI Installation

To verify that Kiro CLI is working correctly, try a simple command:

:::code{showCopyAction=true showLinenumbers=false language=bash}
kiro-cli chat "Hello, Kiro!"
:::

The chat mode will begin, and you should receive a friendly response from Kiro, confirming that the CLI is properly set up, and instructions on getting started.

You can exit Kiro after successful introductions

:::code{showCopyAction=true showLinenumbers=false language=bash}
/exit
:::

## Using Kiro CLI

Throughout this workshop, you will have opportunities to use both the IDE integration and the CLI version of Kiro. The CLI is particularly useful for:

- Quick queries without switching context in your IDE
- Batch operations or scripting
- Working in terminal-based environments
- Generating code snippets that can be piped to files

## Wrap Up

You have now configured your CLI, authenticated using your AWS Builder ID, providing you with a seamless experience across different interfaces.