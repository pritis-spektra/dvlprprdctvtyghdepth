## Lab 3: Creating and Improving Documentation with GitHub Copilot

### Estimated Duration: 75 Minutes

## Overview

In this lab, you will focus on **documentation as a core developer skill**, using GitHub Copilot to assist with creating, reviewing, and improving documentation for a Java Spring Boot application.

Good documentation is essential for:

- Understanding APIs

- Onboarding new developers

- Maintaining code over time

- Reducing defects caused by incorrect assumptions

Rather than writing documentation manually from scratch, this lab demonstrates how **GitHub Copilot can accelerate documentation tasks** while developers remain responsible for accuracy, clarity, and correctness.

## Objectives

In this lab, you will complete the following tasks:

   - Task 1: Understand and Document the API
   - Task 2: Add Swagger (OpenAPI) Documentation
   - Task 3: Add Code Documentation to Classes with GitHub Copilot Help
   - Task 4: Update API with Functional and Technical Documentation Structuring
   - Task 5: Testing Employee API with curl

**Prerequisites**

Before starting this lab, ensure the following:

- Visual Studio Code (or another Copilot-supported IDE)

- GitHub Copilot extension installed and authenticated

- Java Development Kit (JDK) 17 or higher

- Apache Maven (or Maven Wrapper mvnw)

- Project successfully builds and runs (from previous exercises)

### Task 1: Understand and Document the API

Before generating documentation, you must understand **what the API does**. This task reinforces API comprehension using **GitHub Copilot as a code understanding assistant**, not a replacement for developer reasoning.

1. In the Explorer panel, navigate to and open the following file:

   `lab-03-documentation → java → src → main → java → com → example → demo → controller → EmployeeController.java`

   ![Image](./media/image1.png)

1. Select the entire contents of **EmployeeController.java**. Open **Copilot Chat** in **Agent** mode and enter the following prompt to generate the `api-functional.md` documentation file:

   ```
   Using the EmployeeController.java file in this project, create a new file called api-functional.md in the lab-03-documentation/java folder.

   For each endpoint in the controller, document:
   - Base URL
   - Endpoint descriptions
   - HTTP methods
   - Sample curl commands
   - Expected responses

   Format the file as clean Markdown with a heading for each endpoint.
   ```

   > **Note:** Similar to how you created `api-docs.md` in Lab 1 and `api-doc.md` in Lab 2, Agent mode will automatically generate and create the `api-functional.md` file. Click **Keep** to accept the file.

   ![Image](./media/image2.png)

### Task 2: Add Swagger (OpenAPI) Documentation

Swagger (OpenAPI) provides **interactive API documentation**, making it easier to explore and test endpoints without external tools.

This task demonstrates how Copilot assists in **framework-specific documentation setup**.

1. Open **Copilot Chat** in **Agent** mode with the **Claude Sonnet 4.5** model selected. Enter the following prompt to add Swagger/OpenAPI support to the project:

   ```
   Add Swagger/OpenAPI support to this Spring Boot project using springdoc-openapi
   ```

   ![Image](./media/image3.png)

1. Copilot will propose changes to `pom.xml`. Review the dependency it suggests, then click **Keep** to accept it. If Copilot doesn't apply the change automatically, add the dependency manually to `pom.xml` and save the file.

   ![Image](./media/image4.png)

   ![Image](./media/image5.png)

1. Open **Terminal → Git Bash** in VS Code and navigate to the lab folder:

   ```
   cd github-copilot-workshops-labs-java/lab-03-documentation/java/
   ```

   ![Image](./media/image6.png)

1. Reload Maven to pick up the new dependency by running:

   ```
   mvn clean compile
   ```

   ![Image](./media/image7.png)

   ![Image](./media/image8.png)

1. Start the application:

   ```
   mvn spring-boot:run
   ```

   ![Image](./media/image9.png)

1. If port 8080 is already in use, open a **Command Prompt as Administrator** and run the following command to find the process using the port:

   ```
   netstat -ano | findstr :8080
   ```

   Then terminate the process using its PID:

   ```
   taskkill /F /PID <PID>
   ```

   Replace `<PID>` with the actual process ID shown in the output.

1. Open a browser and navigate to:

   ```
   http://localhost:8080/swagger-ui.html
   ```

   The Swagger UI will display all available endpoints interactively.

   ![Image](./media/image10.png)

> **Note:** GitHub Copilot suggests changes but does not automatically apply them. Always verify files such as pom.xml and accept or apply changes explicitly.

### Task 3: Add Code Documentation to Classes with GitHub Copilot Help

Code documentation explains **how the system works internally**, not just what the API does.

In real projects:

- APIs are used by consumers

- Code is maintained by developers

- Test code explains expected behavior

This task shows how **GitHub Copilot helps generate high-quality JavaDoc**, while developers validate accuracy.

1. Open **EmployeeController.java**, select the **entire class**, then open **Copilot Chat** in **Agent** mode and enter the following prompt:

   ```
   Generate JavaDoc for this controller class and all its public methods. Explain the purpose of each endpoint, parameters, and return values.
   ```

   ![Image](./media/image11.png)

1. Copilot will add a class-level JavaDoc comment and method-level documentation describing each endpoint in developer-friendly language.

   ![Image](./media/image12.png)

1. Before accepting, validate the generated JavaDoc against the following checklist. Manually correct anything that doesn't match:

   - Does the JavaDoc accurately describe what the endpoint does?
   - Are all parameter names correct and consistent with the method signature?
   - Does it avoid claiming behavior that doesn't exist (e.g., 404 handling that isn't implemented)?

   ![Image](./media/image12.png)

1. Repeat the process for the following classes. For each, select the entire class, open **Copilot Chat** in **Agent** mode, and enter the prompt below:

   - **EmployeeService.java**
   - **Employee.java**
   - **EmployeeRepository.java**

   ```
   Add clear JavaDoc explaining the responsibility of this class and its methods. Keep the documentation technical and concise.
   ```

   Review and click **Keep** to accept the documentation for each file.

   ![Image](./media/image13.png)

   ![Image](./media/image14.png)

   ![Image](./media/image15.png)

1. Repeat the process for the test classes. For each, select the entire class, open **Copilot Chat** in **Agent** mode, and enter the prompt below:

   - **EmployeeControllerTest.java**
   - **EmployeeRepositoryTest.java**
   - **EmployeeServiceTest.java**

   ```
   Generate JavaDoc for this test class. Explain what behavior is being validated and why.
   ```

   Review and click **Keep** to accept the documentation for each file.

   ![Image](./media/image16.png)

   ![Image](./media/image17.png)

   ![Image](./media/image18.png)

### Task 4: Update API with Functional and Technical Documentation Structuring

Update the API documentation with the new information. Generate two different markdown files — one with the functional documentation and another with the technical documentation.

1. In the root of the project, create a `docs/` folder and inside it create two new Markdown files:
   - `api-functional.md`
   - `api-technical.md`

   ![Image](./media/image19.png)

1. Open **api-functional.md**. Select all existing content in the file, then open **Copilot Chat** in **Agent** mode and enter the following prompt. Review the response and click **Keep** to accept:

   ```
   Convert the existing API documentation into functional documentation. Focus only on endpoints, requests, responses, and usage examples.
   ```

   ![Image](./media/image20.png)

1. Open **api-technical.md**. Open **Copilot Chat** in **Agent** mode and enter the following prompt. Review the response and click **Keep** to accept:

   ```
   Generate technical documentation describing the internal architecture of this project. Explain the responsibility of each layer and how components interact.
   ```

   ![Image](./media/image21.png)

### Task 5: Testing Employee API with curl

1. Open **Terminal → Git Bash** in VS Code and start the application. If you encounter any errors, use Copilot's `/fix` command to resolve them:

   ```
   mvn spring-boot:run
   ```

   ![Image](./media/image22.png)

1. In VS Code, go to **File → Duplicate Workspace** to open a second terminal window. In the new window, open **Git Bash** and run the following command to retrieve all employees:

   ```
   curl -X GET http://localhost:8080/api/employees
   ```

   ![Image](./media/image23.png)

1. Run the following command to add a new employee:

   ```
   curl -X POST http://localhost:8080/api/employees -H "Content-Type: application/json" -d '{
   "name": "John",
   "surname": "Doe",
   "email": "john.doe@example.com"
   }'
   ```

   ![Image](./media/image24.png)

1. Run the following command to retrieve an employee by ID. Replace `{id}` with the actual employee ID returned from the previous step:

   ```
   curl -X GET http://localhost:8080/api/employees/{id}
   ```

   ![Image](./media/image25.png)

1. Run the following command to update an employee record. Replace `{id}` with the actual employee ID:

   ```
   curl -X PUT http://localhost:8080/api/employees/{id} -H "Content-Type: application/json" -d '{
   "name": "Jane",
   "surname": "Doe",
   "email": "jane.doe@example.com"
   }'
   ```

   ![Image](./media/image26.png)

1. Run the following command to delete an employee. Replace `{id}` with the actual employee ID:

   ```
   curl -X DELETE http://localhost:8080/api/employees/{id}
   ```

   ![Image](./media/image27.png)

1. Once you have finished testing, close all open files in VS Code.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex3-task5-lab03-curl-testing" />

## Review

In this lab, you have completed the following:

   - Used GitHub Copilot to understand the REST API and generate functional API documentation
   - Added Swagger (OpenAPI) support to the Spring Boot project
   - Added JavaDoc documentation to controllers, services, models, repositories, and test classes
   - Structured documentation into functional and technical views for different audiences
   - Validated API behavior using curl commands

### You have successfully completed the lab!
### In the Lab Guide section, click the **Next >>** button to proceed to Lab 4.

![](media/up4.png)
