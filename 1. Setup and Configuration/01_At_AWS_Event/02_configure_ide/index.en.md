---
title: 'Configure the IDE environment'
weight: 13
---

To get to the hands-on portion of the workshop as quickly as possible, we will be using [Visual Studio Code Server](https://code.visualstudio.com/docs/remote/vscode-server)
(VS Code Server), hosted on an Amazon EC2 instance.

## Accessing Visual Studio Code Server

1. Open the [Workshop Studio event dashboard](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US)

2. Navigate to the **Event Outputs** pane at the bottom of the page.

3. Copy **Password**. You will need this in the next step.

4. Select the **URL** to open Visual Studio Code Server.

5. In the **Welcome to code-server** dialog, paste the password that you copied earlier, and choose **Submit**.

![](/static/images/submit.png)

6. You should now see the Visual Studio Code IDE, as shown below.

![](/static/images/codeserver.png)

::alert[**Congratulations!** You have successfully accessed Visual Studio Code IDE.]{type="success"}

## Wrap up

The following items have already been installed on your code-server instance:

[AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
[Amazon Kiro CLI](https://kiro.dev/cli/)
[Git](https://github.com/git-guides/install-git/)

You have successfully accessed your VS code-server IDE, which has been pre-configured for the workshop. In the next section, you will set up authentication for Kiro CLI.