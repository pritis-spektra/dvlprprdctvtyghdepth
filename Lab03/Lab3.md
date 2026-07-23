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

1. Open Visual Studio Code and navigate to the folder **lab-03-documentation/java/src/main/java/com/example/demo/controller**, and open **EmployeeController.java** class.

   ![Image](./media/image1.png)

1. Ask Copilot to explain with the **/explain** command and create an `api-functional.md` file with the below values (refer to Exercise 1/Exercise 2 for similar guidance on how to prepare the md file):

   - **Base URL**
   - **Endpoint descriptions**
   - **HTTP methods**
   - **Sample curl commands**
   - **Expected responses**

   ![Image](./media/image2.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex3-task1-lab03-document-api" />

### Task 2: Add Swagger (OpenAPI) Documentation

Swagger (OpenAPI) provides **interactive API documentation**, making it easier to explore and test endpoints without external tools.

This task demonstrates how Copilot assists in **framework-specific documentation setup**.

1. Open Copilot Chat and enter the below prompt in Agent mode with Claude Sonnet 4.5 model:

   +++Add Swagger/OpenAPI support to this Spring Boot project using springdoc-openapi+++

   ![Image](./media/image3.png)

1. Copilot will edit the pom.xml. Review the changes and click on **Keep** to accept the dependency, or manually add it to pom.xml and save the file.

   ![Image](./media/image4.png)

   ![Image](./media/image5.png)

1. Open the Terminal -> Git Bash and run the below command to navigate to the folder:

   +++cd github-copilot-workshops-labs-java/lab-03-documentation/java/+++

   ![Image](./media/image6.png)

1. After making changes to the pom.xml, reload Maven with the command:

   +++mvn clean compile+++

   ![Image](./media/image7.png)

   ![Image](./media/image8.png)

1. Now run the application:

   +++mvn spring-boot:run+++

   ![Image](./media/image9.png)

1. Open cmd as administrator and run +++netstat -ano | findstr :8080+++ to check if port 8080 is busy and kill the process with +++taskkill /F /PID XXX+++.

1. Open the browser and enter - +++http://localhost:8080/swagger-ui.html+++.

   ![Image](./media/image10.png)

> **Note:** GitHub Copilot suggests changes but does not automatically apply them. Always verify files such as pom.xml and accept or apply changes explicitly.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex3-task2-lab03-swagger-docs" />

### Task 3: Add Code Documentation to Classes with GitHub Copilot Help

Code documentation explains **how the system works internally**, not just what the API does.

In real projects:

- APIs are used by consumers

- Code is maintained by developers

- Test code explains expected behavior

This task shows how **GitHub Copilot helps generate high-quality JavaDoc**, while developers validate accuracy.

1. Open the file **EmployeeController.java** and select the **entire class** and open **GitHub Copilot Chat**:

   +++Generate JavaDoc for this controller class and all its public methods. Explain the purpose of each endpoint, parameters, and return values.+++

   ![Image](./media/image11.png)

1. Copilot will add a class-level JavaDoc, add method-level documentation, and describe endpoints in developer language.

   ![Image](./media/image12.png)

1. Before accepting, check the below checklist and accept or make changes manually if required:

   - Does the JavaDoc match the actual endpoint?
   - Are parameter names correct?
   - Does it avoid claiming behavior that doesn't exist (e.g., 404 handling)?

   ![Image](./media/image12.png)

1. Repeat the above step for other classes as well:

   - **EmployeeService.java**

   - **Employee.java**

   - **EmployeeRepository.java**

   Prompt:

   +++Add clear JavaDoc explaining the responsibility of this class and its methods. Keep the documentation technical and concise.+++

   ![Image](./media/image13.png)

   ![Image](./media/image14.png)

   ![Image](./media/image15.png)

1. Repeat the above steps to add documentation to the test classes with the prompt:

   - EmployeeControllerTest.java
   - EmployeeRepositoryTest.java
   - EmployeeServiceTest.java

   +++Generate JavaDoc for this test class. Explain what behavior is being validated and why?+++

   ![Image](./media/image16.png)

   ![Image](./media/image17.png)

   ![Image](./media/image18.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex3-task3-lab03-code-documentation" />

### Task 4: Update API with Functional and Technical Documentation Structuring

Update the API documentation with the new information. Generate two different markdown files — one with the functional documentation and another with the technical documentation.

1. Go to the root folder and create a folder - +++docs/+++ and create two md files - +++api-technical.md+++ and +++api-functional.md+++.

   ![Image](./media/image19.png)

1. Open **api-functional.md**, select the content of the file and ask Copilot. Review and accept the changes:

   +++Convert the existing API documentation into functional documentation. Focus only on endpoints, requests, responses, and usage examples.+++

   ![Image](./media/image20.png)

1. Open **api-technical.md** file and ask Copilot. Review the response and accept the changes:

   +++Generate technical documentation describing the internal architecture of this project. Explain the responsibility of each layer and how components interact.+++

   ![Image](./media/image21.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex3-task4-lab03-functional-technical-docs" />

### Task 5: Testing Employee API with curl

1. Open terminal and run the below command to run the app (use /fix and fix if you encounter any errors):

   +++mvn spring-boot:run+++

   ![Image](./media/image22.png)

1. Duplicate the workspace (File -> Duplicate Workspace) and run below command in Git Bash:

   +++curl -X GET http://localhost:8080/api/employees+++

   ![Image](./media/image23.png)

1. **Run below curl command to add a new Employee:**

   ```
   curl -X POST http://localhost:8080/api/employees -H "Content-Type: application/json" -d '{
   "name": "John",
   "surname": "Doe",
   "email": "john.doe@example.com"
   }'
   ```

   ![Image](./media/image24.png)

1. Run below command to get Employee by ID. Replace {id} with the actual employee ID:

   ```
   curl -X GET http://localhost:8080/api/employees/{id}
   ```

   ![Image](./media/image25.png)

1. Run below command to update the employee record. Replace {id} with the actual employee ID:

   ```
   curl -X PUT http://localhost:8080/api/employees/{id} -H "Content-Type: application/json" -d '{
   "name": "Jane",
   "surname": "Doe",
   "email": "jane.doe@example.com"
   }'
   ```

   ![Image](./media/image26.png)

1. Run below curl command to delete employee. Replace {id} with the actual employee ID:

   ```
   curl -X DELETE http://localhost:8080/api/employees/{id}
   ```

   ![Image](./media/image27.png)

1. Close all the open files.

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
