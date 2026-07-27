# Accelerating day-to-day development tasks with GitHub Copilot

### Overall Estimated Duration: 17 Hours 15 Minutes

## Overview

In this hands-on lab series, you will use **GitHub Copilot** as an AI pair programmer to accelerate real-world, day-to-day development tasks across Java, Python, and Node.js codebases. You will progress from understanding and testing an existing Java Spring Boot API, through debugging, documentation, and refactoring exercises, to building full-stack prototypes from scratch, fixing buggy REST APIs, and resolving a simulated production incident under time pressure. Throughout, you will practice using Copilot's Autocomplete, Chat, Agent, and custom prompt modes, and building your own MCP (Model Context Protocol) server — all while retaining full developer judgment over what Copilot suggests, refines, or rejects.

## Objectives

In this lab, you will:

- **Lab 1:** Use GitHub Copilot to understand an existing Java Spring Boot REST API and write unit tests across the repository, service, and controller layers, then extend the application with a new feature.
- **Lab 2:** Use GitHub Copilot as a debugging partner to interpret compile-time and runtime errors in a Java Spring Boot application, identify root causes, and apply validated fixes.
- **Lab 3:** Use GitHub Copilot to generate and refine functional and technical documentation for a Java Spring Boot API, including Swagger/OpenAPI support and JavaDoc.
- **Lab 4:** Use GitHub Copilot to migrate Markdown documentation into working code, then refactor methods, add error handling, extract reusable logic, and generate documentation.
- **Lab 5 (Optional):** Explore GitHub Copilot's Autocomplete, Chat, and Agent modes, and create reusable custom prompt files for explaining, reviewing, and testing code.
- **Lab 6 (Optional):** Build a custom Java MCP server that exposes tools GitHub Copilot can discover and invoke, and build an MCP client to connect to it.
- **Lab 7:** Build a Python Flask Task Management REST API from an empty repository using GitHub Copilot to accelerate scaffolding, feature development, and testing.
- **Lab 8:** Inherit a poorly written order processing module and use GitHub Copilot to understand, refactor, test, and document it with a code-review mindset.
- **Lab 9:** Use GitHub Copilot to rapidly scaffold a demo-ready Flask customer health scoring dashboard from a vague business request.
- **Lab 10:** Use GitHub Copilot to systematically identify, understand, and fix bugs, security vulnerabilities, and missing validation in a buggy Node.js Express REST API.
- **Lab 11 (Optional):** Respond to a simulated production incident at a fintech company, using human triage first and GitHub Copilot's Agent mode to diagnose and fix critical bugs under time pressure.
- **Lab 12 (Optional):** Use GitHub Copilot to scaffold a simple, server-rendered Flask customer health dashboard with mock data and Bootstrap 5 styling.

## Pre-requisites

Participants should have:

- Visual Studio Code (or another Copilot-supported IDE).
- GitHub Copilot extension installed and authenticated, with an active or trial license.
- A GitHub account with Copilot access.
- Java Development Kit (JDK) 17 or higher, and Apache Maven (or the Maven Wrapper `mvnw`).
- Python 3.x with `pip`.
- Node.js and `npm`.
- Basic familiarity with Java/Spring Boot, Python/Flask, and Node.js/Express concepts.
- A modern web browser such as Microsoft Edge or Google Chrome.

## Accessing Your Lab Environment

Once you're ready to dive in, your virtual machine and **Guide** will be right at your fingertips within your web browser.

![Image](media/gs1.png)

## Virtual Machine & Lab Guide

Your virtual machine is your workhorse throughout the workshop. The guide is your roadmap to success.

## Lab Guide Zoom In/Zoom Out

To adjust the zoom level for the environment page, click the **A↕ : 100%** icon located next to the timer in the lab environment.

![Image](media/gs2.png)

## Exploring Your Lab Resources

To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.

![Image](media/gs3.png)

## Managing Your Virtual Machine

Feel free to **start, restart, or stop (2)** your virtual machine as needed from the **Resources (1)** tab. Your experience is in your hands!

![Image](./media/gs4.png)

## Utilizing the Split Window Feature

For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the top right corner.

![Image](./media/gs5.png)

## Resize the Virtual Machine View

Use the **slider (three vertical dots)** located between the **Virtual Machine** and the **Lab Guide** panes to adjust the display size, allowing you to customize the layout based on your preference.

![](./media/gs6.png)

### Login to GitHub

1. In the **Lab VM**, open the **Microsoft Edge** browser from the desktop.

   ![](./media/gg1.png)

1. Navigate to the **GitHub login** page by copying and pasting the following URL into the address bar:

   ```
   https://github.com/login
   ```

1. On the **Sign in to GitHub** tab, enter the provided **GitHub username** in the input field, and click on **Sign in with your identity provider** **(2)**.

    - **Email/Username:** <inject key="GitHub User Name" enableCopy="true"/> **(1)**

      ![](./media/gg2.png)

1. Click on **Continue** on the **Single sign-on to CloudLabs Organizations** page to proceed.

   ![](./media/gg3.png)

1. You'll see the **Sign in** tab. Here, enter your Azure Entra credentials and click **Next (2)**.

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject> **(1)**

     ![](./media/gg4.png)

1. Next, provide your Temporary Password and click on **Sign in (2)**

   - **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject> **(1)**

     ![](./media/gg5.png)

1. On the **Stay Signed in?** pop-up, click on No.

   ![](./media/gg6.png)

1. You are now successfully logged in to **GitHub** and have been redirected to the **GitHub homepage**.

   ![](./media/gg7.png)

   >**Note** : If **Start using Copilot** pop-up appears, click on **X** to close it
    ![](./media/gg8.png)

### GitHub Copilot is moving to usage-based billing

Instead of counting premium requests, every Copilot plan will include a monthly allotment of **GitHub AI Credits**, with the option for paid plans to purchase additional usage. Usage will be calculated based on token consumption, including input, output, and cached tokens, using the listed API rates for each model.​

​**What's changing**

- Starting June 1, GitHub will replace Premium Request Units with GitHub AI Credits. ​
- Credits will be consumed based on actual AI token usage. ​
- Base pricing for all GitHub Copilot plans will remain unchanged. ​
- Code completions and Next Edit Suggestions will continue to be included at no extra cost. ​
- The fallback experience to lower-cost AI models will no longer be available after credits are exhausted. ​
- Copilot Code Review will also consume GitHub Actions minutes in addition to GitHub AI Credits.​

### GitHub Copilot Plan updates and AI Credit changes​

| Plan | Monthly Price | Included Monthly AI Credits | Key Update |
|:---|:---|:---|:---|
| **Copilot Pro** | $10/month | $10 AI Credits | Migrates to usage-based billing from June 1 |
| **Copilot Pro+** | $39/month | $39 AI Credits | Includes higher AI credit allocation |
| **Copilot Business** | $19/user/month | $19 AI Credits | Includes pooled organizational credits |
| **Copilot Enterprise** | $39/user/month | $39 AI Credits | Adds advanced budget and spending controls |

### Managing roles and governance via enterprise teams:

GitHub Enterprise Cloud has introduced new enterprise-level governance and management capabilities to help enterprises manage access, security, and policies at scale.

As of today, enterprise owners can use GitHub’s API or the enterprise settings UI to:

- Assign enterprise teams to organizations.
- Create and assign custom enterprise roles.
- Assign enterprise roles to both enterprise teams and users, including the new predefined Enterprise Security Manager role.
- Empower organization and repository owners to assign roles to enterprise teams within their scope.
- Assign enterprise teams and roles to ruleset bypass lists.

### Copilot Insights:

The Copilot usage metrics dashboard gives enterprise administrators and billing managers clear visibility into Copilot adoption and usage under the Insights tab.

These metrics help you understand:

- **Overall usage and adoption:** Review indicators like weekly usage provide a broad view of Copilot adoption across your enterprise.
- **Specific model, feature, and language usage:** See which AI models and programming languages are most utilized by your teams, highlighting areas for even greater value.
- **Agent adoption percentage:** Track how many developers are using Copilot for advanced tasks like refactoring, debugging, and complex problem solving. High agent adoption signals a shift toward truly transformative coding.

![](../../media/copilot-insights.png)

## Setting up IDE

1. Open the **Visual Studio Code** shortcut from the desktop of your **Lab VM**.

   ![](./media/gg9.png)

1. To sign in to **GitHub Copilot**, follow the steps below:

   - In Visual Studio Code, click on the **Icon (1)** in the GitHub Copilot Chat panel located at the bottom-right corner of the window, and select **Use AI Features (2)**.

     ![](./media/gg14.png)

   - On the *Sign in to use GitHub Copilot* screen, select **Continue with GitHub** to sign in.
  
     ![](./media/gg10.png)

   - Now, in the browser, click on **Continue** to Authorize Visual Studio Code. 

     ![](./media/gg11.png)

   - On the next window, click on **Authrize Visual-Studio-Code**.

     ![](./media/gg12.png)

   - You will see a pop-up asking **This site is trying to open Visual Studio Code**. Enable the **CheckBox (1)** and then click on **Open (2)**. It will take you to VS Code. 

     ![](./media/gg13.png)

## Summary

In this lab, you successfully set up your development environment, logged into GitHub, created a new repository, and configured Visual Studio Code with GitHub Copilot.  

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: cloudlabs-support@spektrasystems.com
- Live Chat Support: https://cloudlabs.ai/labs-support

#### You have successfully completed the lab. Click on **Next >>** to continue to the next lab.

![](./media/nx.png)