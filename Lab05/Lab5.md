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

1. Navigate to **lab-05-agents\java\src\main\java\com\example\demo** and open the file **EmployeeController.java**.

   ![Image](./media/image1.png)

1. Scroll to an empty line inside the class. Type the following comment. Press **Enter** and pause for a moment.

   ```
   //Method to check if a number is prime
   ```

   ![Image](./media/image2.png)

1. Observe Copilot's inline suggestion. Press **Tab** to accept the suggestion.

   ![Image](./media/image3.png)

1. Try another function, for instance. Accept the suggested method. Edit the generated code and see if more relevant suggestions appear.

   ```
   //Method to compute factorial of a number
   ```

   ![Image](./media/image4.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex5-task1-lab05-autocomplete" />

### Task 2: Copilot Chat

In this task, you will use Copilot Chat's Ask mode to get explanations and code examples for common Java questions. You will intentionally introduce a NullPointerException and use /explain and /terminalfix to diagnose and resolve it.

1. Open Copilot Chat (you can also press Ctrl+Shift+' in VS Code).

1. Ask (select ask mode) the following question and read the response carefully. Copilot should provide clear explanations and example code.
   ```
   How can I inject a service into this controller?
   ```

   ![Image](./media/image5.png)

1. Ask (select ask mode) the following question and read the response carefully. Copilot should provide clear explanations and example code.

   ```
   Why am I getting a NullPointerException on line 25?
   ```

   ![Image](./media/image6.png)

1. Ask (select ask mode) the following question and read the response carefully. Copilot should provide clear explanations and example code.

   ```
   Show me how to read a file line by line in Java.
   ```

   ![Image](./media/image7.png)

1. Open **EmployeeService.java** under the **service** folder and **set an object to null** and then try to use it. This causes a **NullPointerException**, a very common Java runtime error. Modify the **getAllEmployees()** method and save the file (Note: we are introducing a bug and checking how Copilot helps us to fix it):

   ```
   public List<Employee> getAllEmployees() {
   EmployeeRepository repo = null;   // intentionally introduced bug
   return repo.findAll();             // this will cause NullPointerException
   }
   ```

   ![Image](./media/image8.png)

   > **Note:** We created a variable (repo) and explicitly set it to null. When we try to call a method on it, this will fail.

1. Open terminal -> Git Bash and run the app with the below commands. Application will run:

   ```
   cd github-copilot-workshops-labs-java/lab-05-agents/java/
   ```

   ```
   mvn spring-boot:run
   ```

   ![Image](./media/image9.png)

1. Open a browser and navigate to the below link. You can see a **500 Internal Server Error** in the browser.

   ```
   http://localhost:8080/api/employees. 
   ```

   ![Image](./media/image10.png)

1. Switch back to the terminal and the **stack trace** in the terminal contains NullPointerException.

   ![Image](./media/image11.png)

1. Select the error message in the terminal or select the buggy method in EmployeeService.java and open Copilot Chat, enter `/explain`.

   ![Image](./media/image12.png)

1. Copilot explains — what a NullPointerException is, which object is null, why repo.findAll() fails, and where the issue originates.

1. Select the agent mode and ask Copilot to fix it with the command `/terminalfix`.

   ![Image](./media/image13.png)

1. Review and accept the fix and then re-run the application to check the fix (stop the server with Ctrl+C in the terminal):

   ```
   mvn spring-boot:run
   ```

   ![Image](./media/image14.png)

1. Open a browser and navigate to `http://localhost:8080/api/employees`.

   ![Image](./media/image15.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex5-task2-lab05-copilot-chat" />

### Task 3: Copilot Agent Mode

In this task, you will use Copilot Agent mode to refactor controllers for constructor dependency injection across multiple files. You will review the proposed plan and generate unit tests for the service package.

1. Open the Command Palette: Ctrl + Shift + P. Search for **"Copilot Agent"** and activate it (You can also select Agent mode directly).

   ![Image](./media/image16.png)

   ![Image](./media/image17.png)

1. Assign a high-level task, for example. Review the **plan** proposed by the Agent. Approve the plan to let Copilot apply changes across files.

   ```
   Refactor all controllers to use constructor dependency injection.
   ```

   ![Image](./media/image18.png)

1. Ask follow-up questions such as:

   ```
   Explain why this refactor was necessary.
   ```

   ![Image](./media/image19.png)

1. Try another Agent task:

   ```
   Generate unit tests for all public methods in the service package.
   ```

   ![Image](./media/image20.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex5-task3-lab05-agent-mode" />

### Task 4: Custom Prompt Files/Instructions

In this task, you will create reusable *.prompt.md files for explaining, reviewing, and generating tests for Java code. You will invoke these custom slash commands in Copilot Chat and use one to generate and run a DivideTest.java test class.

1. Navigate to **lab-05-agents/java/.github** and create a folder `prompts`.

   ![Image](./media/image21.png)

1. Create a new file with the name `explain-java.prompt.md` and paste the below prompt:

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

1. Open Copilot Chat and type - `/explain-java` and Copilot will recognize it as a custom prompt:

   ![Image](./media/image23.png)

1. Paste this example code when asked and set the audience to **"beginner"**:

   ```
   public int fibonacci(int n) {
   return n <= 1 ? n : fibonacci(n-1) + fibonacci(n-2);
   }
   ```

   ![Image](./media/image24.png)

1. Create another file named `review-java.prompt.md` and paste the below content:

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

1. Go back to Copilot Chat and run: `/review-java`.

   ![Image](./media/image26.png)

   ![Image](./media/image27.png)

1. Paste this code snippet (which has a common Java issue). Copilot should flag potential issues like modifying the input parameter and suggest best practices:

   ```
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

1. Create a file called `generate-tests-java.prompt.md` in **.github/prompts/generate-tests-java.prompt.md** and paste the below content:

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

1. Enter in Copilot Chat Agent mode: `/generate-tests-java`.

   ![Image](./media/image30.png)

1. Add the below method:

   ```
   public double divide(double a, double b) {
   return a / b;
   }
   ```

   ![Image](./media/image31.png)

1. Enter test matrix – `zero division, negative numbers, large numbers`.

   ![Image](./media/image32.png)

1. Create a `DivideTest.java` in **src/test/java** and save the above results.

   ![Image](./media/image33.png)

1. Open terminal and run tests with below commands:

   ```
   cd lab-05-agents\java
   ```

   ```
   mvn test
   ```

   ![Image](./media/image34.png)

1. If you see any compilation errors, ask Copilot to fix with `/terminalfix` command. Review and accept the fix:

   ![Image](./media/image35.png)

1. Save the file and run the test again `mvn test`. Tests will run successfully.

   ![Image](./media/image36.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex5-task4-lab05-custom-prompts" />

## Review

In this lab, you have completed the following:

   - Used Copilot Autocomplete (inline ghost text) driven by natural language comments
   - Used Copilot Chat to ask questions, understand errors, and debug a NullPointerException
   - Used Copilot Agent mode for multi-file refactoring and test generation
   - Created reusable custom prompt files for explaining, reviewing, and testing Java code

### You have successfully completed the lab!
### In the Lab Guide section, click the **Next >>** button to proceed to Lab 6.

![](./media/nx.png) 
