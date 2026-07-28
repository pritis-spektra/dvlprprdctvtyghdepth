# Lab 5: Exploring GitHub Copilot Modes - Autocomplete, Chat, Agent, and Custom Prompts (Optional)

### Estimated Duration: 75 Minutes

## Overview

In this lab, you will learn how to use **GitHub Copilot** effectively by working through a series of hands-on challenges. You will practice using different Copilot modes and features to:

- Write code using inline suggestions (Autocomplete)

- Ask questions and debug code using Copilot Chat

- Perform multi-file changes using Copilot Agent mode

- Create reusable prompt files for explaining code, reviewing code, and generating tests

This lab focuses on **developer productivity** and **learning how to guide Copilot**, not just accepting suggestions.

## Objectives

In this lab, you will complete the following tasks:

   - Task 1: Basic Copilot Autocomplete
   - Task 2: Copilot Chat
   - Task 3: Copilot Agent Mode
   - Task 4: Custom Prompt Files/Instructions

**Prerequisites**

Before starting this lab, make sure you have:

- Visual Studio Code installed

- GitHub Copilot extension installed and signed in

- Java 17 or higher installed

- Maven installed

- Basic understanding of Java and Spring Boot concepts

### Task 1: Basic Copilot Autocomplete

In this task, you will write comments describing simple functions and observe Copilot's inline autocomplete suggestions. You will accept and edit the generated code to see how suggestions adapt.

1. Navigate to **lab-05-agents/java/src/main/java/com/example/demo** and open the file **EmployeeController.java**.

   ![Image](./media/image1.png)

1. Scroll to an empty line inside the class. Type the following comment. Press **Enter** and pause for a moment to let Copilot generate a suggestion.

   ```
   //Method to check if a number is prime
   ```

   ![Image](./media/image2.png)

1. Observe Copilot's inline suggestion (shown as dimmed ghost text). Press **Tab** to accept the suggestion.

   ![Image](./media/image3.png)

1. Try another function by typing the comment below. Press **Enter** and pause. Accept the suggested method. Edit the generated code and observe how Copilot updates its suggestions accordingly.

   ```
   //Method to compute factorial of a number
   ```

   ![Image](./media/image4.png)

### Task 2: Copilot Chat

In this task, you will use Copilot Chat's Ask mode to get explanations and code examples for common Java questions. You will intentionally introduce a NullPointerException and use /explain and /terminalfix to diagnose and resolve it.

1. Open Copilot Chat (you can also press **Ctrl+Shift+I** in VS Code).

1. In the Copilot Chat panel, ensure **Ask** mode is selected from the dropdown. Enter the following question and read the response carefully. Copilot should provide clear explanations and example code.

   ```
   How can I inject a service into this controller?
   ```

   ![Image](./media/image5.png)

1. Ask the following question in **Ask** mode and read the response carefully:

   ```
   Why am I getting a NullPointerException on line 25?
   ```

   ![Image](./media/image6.png)

1. Ask the following question in **Ask** mode and read the response carefully:

   ```
   Show me how to read a file line by line in Java.
   ```

   ![Image](./media/image7.png)

1. Open **EmployeeService.java** under the **service** folder. Modify the **getAllEmployees()** method to intentionally introduce a NullPointerException bug and save the file:

   ```java
   public List<Employee> getAllEmployees() {
   EmployeeRepository repo = null;   // intentionally introduced bug
   return repo.findAll();             // this will cause NullPointerException
   }
   ```

   ![Image](./media/image8.png)

   > **Note:** We are creating a variable (`repo`) and explicitly setting it to null. When we try to call a method on it, it will fail at runtime. This simulates a common real-world mistake.

1. Open **Terminal → New Terminal** in VS Code, click the dropdown and select **Git Bash**. Navigate to the lab folder:

   ```
   cd github-copilot-workshops-labs-java/lab-05-agents/java/
   ```

1. Set up Maven for this terminal session:

   ```
   export MAVEN_HOME="/c/Users/Admin/Documents/maven-mvnd-1.0.5-windows-amd64/maven-mvnd-1.0.5-windows-amd64"
   ```

   ```
   export PATH="$MAVEN_HOME/bin:$PATH"
   ```

1. Run the app:

   ```
   mvn spring-boot:run
   ```

   ![Image](./media/image9.png)

1. Open a browser and navigate to the link below. You will see a **500 Internal Server Error** in the browser:

   ```
   http://localhost:8080/api/employees
   ```

   ![Image](./media/image10.png)

1. Switch back to the terminal — the stack trace in the terminal contains a NullPointerException.

   ![Image](./media/image11.png)

1. Select the error message in the terminal, or select the buggy method in **EmployeeService.java**, and open **Copilot Chat**. Enter `/explain`:

   ![Image](./media/image12.png)

1. Copilot explains what a NullPointerException is, which object is null, why `repo.findAll()` fails, and where the issue originates.

1. Switch to **Agent** mode in the Copilot Chat dropdown and ask Copilot to fix it with the command `/terminalfix`.

   ![Image](./media/image13.png)

1. Review and accept the fix. Then stop the server in the terminal by pressing **Ctrl+C**, and re-run the application to confirm the fix works:

   ```
   mvn spring-boot:run
   ```

   ![Image](./media/image14.png)

1. Open a browser and navigate to `http://localhost:8080/api/employees`. The application should now return a valid response.

   ![Image](./media/image15.png)

### Task 3: Copilot Agent Mode

In this task, you will use Copilot Agent mode to refactor controllers for constructor dependency injection across multiple files. You will review the proposed plan and generate unit tests for the service package.

1. Open the Command Palette: **Ctrl+Shift+P**. Search for **"Copilot Agent"** and activate it (you can also select **Agent** mode directly from the Copilot Chat dropdown).

   ![Image](./media/image16.png)

   ![Image](./media/image17.png)

1. Enter the following high-level task. Review the **plan** proposed by the Agent before approving it. Approve the plan to let Copilot apply changes across files:

   ```
   Refactor all controllers to use constructor dependency injection.
   ```

   ![Image](./media/image18.png)

1. Ask a follow-up question to understand the change:

   ```
   Explain why this refactor was necessary.
   ```

   ![Image](./media/image19.png)

1. Try another Agent task:

   ```
   Generate unit tests for all public methods in the service package.
   ```

   ![Image](./media/image20.png)

### Task 4: Custom Prompt Files/Instructions

In this task, you will create reusable *.prompt.md files for explaining, reviewing, and generating tests for Java code. You will invoke these custom slash commands in Copilot Chat and use one to generate and run a DivideTest.java test class.

1. Navigate to **lab-05-agents/java/.github** in the Explorer panel and create a folder named `prompts`.

   ![Image](./media/image21.png)

1. Create a new file named `explain-java.prompt.md` inside the `prompts` folder and paste the below content:

   ```
   ---
   mode: 'agent'
   description: 'Explain a Java method in a simple and structured way'
   ---
   Please explain the following Java code clearly for the selected audience.
   **Java code to explain**:
   ${input:code:Paste the Java code here}
   **Target audience**:
   ${input:audience:Who is this for? (beginner/intermediate/advanced)}
   Your explanation must include:
   - A short summary of what the code does
   - A step-by-step breakdown
   - Explanation of key Java concepts involved
   - One simple usage example
   - Common pitfalls or edge cases
   ```

   ![Image](./media/image22.png)

1. Open Copilot Chat and type `/explain-java` — Copilot will recognize it as a custom prompt:

   ![Image](./media/image23.png)

1. When prompted, paste the following example code and set the audience to **"beginner"**:

   ```java
   public int fibonacci(int n) {
   return n <= 1 ? n : fibonacci(n-1) + fibonacci(n-2);
   }
   ```

   ![Image](./media/image24.png)

1. Create another file named `review-java.prompt.md` in the `prompts` folder and paste the below content:

   ```
   ---
   mode: 'agent'
   description: 'Perform a structured code review for Java code'
   ---
   Perform a technical review of the following Java code.
   **Code to review**:
   ${input:code:Paste your Java code here}
   **Focus areas**:
   ${input:criteria:readability, performance, security, maintainability, testing}
   Please respond with:
   - Findings grouped by each selected area
   - Potential risks and how to fix them
   - Recommended refactors or Java best practice improvements
   - Quick wins with priority levels (high/medium/low)
   ```

   ![Image](./media/image25.png)

1. Go back to Copilot Chat and run `/review-java`.

   ![Image](./media/image26.png)

   ![Image](./media/image27.png)

1. Paste the following code snippet (which has a common Java issue). Copilot should flag potential issues like modifying the input parameter and suggest best practices:

   ```java
   public List<Integer> addItems(List<Integer> items) {
   if (items == null) {
       items = new ArrayList<>();
   }
   for (int i = 0; i < 10; i++) {
       items.add(i);
   }
   return items;
   }
   ```

   ![Image](./media/image28.png)

1. Create a file called `generate-tests-java.prompt.md` in **.github/prompts/** and paste the below content:

   ```
   ---
   description: 'Generate JUnit unit tests for a given Java method'
   ---
   Write a JUnit test suite for the following Java code.
   **Code under test**:
   ${input:code:Paste the Java method or class here}
   **Test strategy**:
   ${input:matrix:Describe the edge cases, invalid inputs, and expected failures}
   Requirements:
   - Use JUnit 5 conventions
   - Cover success, edge, and failure scenarios
   - Use clear and descriptive test names
   - Include minimal setup and teardown (@BeforeEach, @AfterEach if needed)
   - Highlight any missing test coverage areas
   ```

   ![Image](./media/image29.png)

1. In Copilot Chat in **Agent** mode, type `/generate-tests-java`.

   ![Image](./media/image30.png)

1. When prompted for the code to test, add the following method:

   ```java
   public double divide(double a, double b) {
   return a / b;
   }
   ```

   ![Image](./media/image31.png)

1. When prompted for the test matrix, enter: `zero division, negative numbers, large numbers`.

   ![Image](./media/image32.png)

1. Create a `DivideTest.java` file in **src/test/java** and paste the generated test code. Save the file.

   ![Image](./media/image33.png)

1. Open the terminal and run the tests:

   ```
   cd lab-05-agents/java
   ```

   ```
   mvn test
   ```

   ![Image](./media/image34.png)

1. If you see any compilation errors, select the error in the terminal and ask Copilot to fix it with `/terminalfix`. Review and accept the fix.

   ![Image](./media/image35.png)

1. Save the file and run the tests again. Tests will run successfully:

   ```
   mvn test
   ```

   ![Image](./media/image36.png)

## Review

In this lab, you have completed the following:

   - Used Copilot Autocomplete (inline ghost text) driven by natural language comments
   - Used Copilot Chat to ask questions, understand errors, and debug a NullPointerException
   - Used Copilot Agent mode for multi-file refactoring and test generation
   - Created reusable custom prompt files for explaining, reviewing, and testing Java code

### You have successfully completed the lab!
### In the Lab Guide section, click the **Next >>** button to proceed to Lab 6.

![](./media/nx.png) 
