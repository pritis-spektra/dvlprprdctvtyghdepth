## Lab 6: Building and Using MCP Servers with GitHub Copilot (Optional)

### Estimated Duration: 60 Minutes

## Overview

In this lab, you will learn how to:

- Understand the **Model Context Protocol (MCP)**

- Build a **custom MCP server** using Java

- Expose tools that GitHub Copilot can discover and invoke

- Use **GitHub Copilot Agent Mode** with MCP servers

- Consume both **custom MCPs** and **prebuilt MCPs** (Playwright, Microsoft Learn)

## Objectives

In this lab, you will complete the following tasks:

   - Task 1: Create Your First MCP Server using GitHub Copilot
   - Task 2: Create MCPClient.java

### Task 1: Create Your First MCP Server using GitHub Copilot

Create a minimal MCP server that Copilot can connect to.

1. Navigate to **lab-06-MCP** and then open GitHub Copilot Chat and ask Copilot the below prompt in Agent mode + Claude Sonnet 4.6 model:

   ```
   @workspace I need to create a Maven project structure for lab-06-mcp.
   The project should be a Model Context Protocol (MCP) server in Java.
   Please create:
   - src/main/java/com/example/mcp directory structure
   - src/test/java/com/example/mcp directory structure
   - Basic McpServer.java main class
   - Sample tool implementation
   ```

1. Copilot will provide terminal commands or file creation instructions.

1. In Visual Studio Code, navigate to **Lab-06-mcp-java-src/main/java/com/example/mcp/** and create a file +++MCPServer.java+++.

   ![Image](./media/image1.png)

1. Enter the below prompt in Agent mode of GitHub Copilot Chat:

   ```
   Create a minimal MCP server using the MCP Java SDK (0.16.0).
   Use STDIO transport.
   Set server name to demo-mcp-server and version 1.0.0.
   Extend MCPServer.java to register the following tools:
   add, subtract, multiply, divide.
   Each tool:
   - Accepts two numbers
   - Returns the result
   - Handles division by zero as an error
   ```

   ![Image](./media/image2.png)

1. Keep allowing the response request as Copilot performs:

   - Analyzed MCP SDK structure

   - Adjusted JSON mapper usage

   - Updated pom.xml

   - Created MCPServer.java

   - Removed .gitkeep placeholders

   ![Image](./media/image3.png)

   ![Image](./media/image4.png)

1. Review the response and then allow Copilot to create the below tools:

   - add - adds two numbers

   - subtract - subtracts two numbers

   - multiply - multiplies two numbers

   - divide - divides two numbers (with error handling for division by zero)

   ![Image](./media/image5.png)

1. Open the **Terminal -> Git Bash** and run the below command to run the server. Make sure the server is up and running:

   +++cd github-copilot-workshops-labs-java/lab-06-mcp/java/+++

   ```
   mvn exec:java
   ```

   ![Image](./media/image6.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex6-task1-lab06-mcp-server" />

### Task 2: Create MCPClient.java

1. Enter the below prompt in Copilot:

   ```
   Create a Java MCP client in package com.example.mcp that:
   - Connects to the MCP server via STDIO
   - Lists available tools
   - Calls each math tool
   - Demonstrates division by zero error handling
   ```

   ![Image](./media/image7.png)

1. Allow tool results to create:

   ![Image](./media/image8.png)

   ![Image](./media/image9.png)

   ![Image](./media/image10.png)

1. MCPClient got created. Allow Copilot to compile:

   ![Image](./media/image11.png)

1. Now run the client:

   ```
   mvn package -q; mvn exec:java '-Dexec.mainClass=com.example.mcp.MCPClient'
   ```

   ![Image](./media/image12.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex6-task2-lab06-mcp-client" />

## Review

In this lab, you have completed the following:

   - Understood MCP concepts and architecture
   - Used GitHub Copilot Agent Mode to create a custom Java-based MCP server
   - Exposed executable math tools (add, subtract, multiply, divide) discoverable by Copilot
   - Created an MCP client to connect to the server, list available tools, and invoke them programmatically
   - Handled error scenarios such as division by zero

### You have successfully completed the lab!
### In the Lab Guide section, click the **Next >>** button to proceed to Lab 7.

![](media/up4.png)
