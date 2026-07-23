## Exercise 1: Using GitHub Copilot to Build and Improve Automated Tests for a Java REST API

### Estimated Duration: 90 Minutes

## Overview

This exercise helps developers learn how to **use GitHub Copilot effectively and responsibly** while working with an existing Java Spring Boot application. Rather than generating code blindly, learners will practice using Copilot to understand unfamiliar APIs, create unit tests across multiple layers, extend functionality, and improve test quality - while retaining full developer judgment and control.

## Objectives

In this exercise, you will complete the following tasks:

   - Task 0: Activate and Configure GitHub Copilot Subscription
   - Task 1: Understand the API
   - Task 2: Create Repository Layer Unit Tests
   - Task 3: Create Service Layer Unit Tests
   - Task 4: Create Controller Layer Unit Tests
   - Task 5: Add a New API Operation

**Prerequisites**

Before starting this exercise, ensure the following are installed and configured:

- Visual Studio Code (or another Copilot-supported IDE)

- GitHub Copilot extension (authenticated)

- Java Development Kit (JDK) 17 or higher

- Apache Maven

- GitHub account with Copilot access

**Lab Environment**

- Operating System: Any

- Programming Language: Java

- Framework: Spring Boot

- Build Tool: Maven

- Testing Libraries: JUnit, Mockito

### Task 0: Activate and Configure GitHub Copilot Subscription

In this task, you will activate GitHub Copilot and configure it within Visual Studio Code to enable AI-assisted software development. You will sign in with your GitHub account, authenticate your access, and install the GitHub Copilot Chat extension in VS Code.

**If you still do not have an active Copilot license, a 30-day trial can be requested with the following steps. Make sure to cancel your license before the trial ends to avoid getting billed**.

> **Note:** This task is intended only for users who have not yet activated or configured GitHub Copilot. If your setup is already complete, you may skip these steps.

1. Open a new tab in your browser and go to Signup to GitHub Copilot - `https://github.com/github-copilot/signup`

1. Click on the **"Get access to GitHub Copilot"** button.

   ![Image](./media/image42.png)

1. Enter billing information with your personal credit card and then click on **Save**.

   ![Image](./media/image43.png)

1. Enter your billing information and then click on the **Save payment information** button.

   ![Image](./media/image44.png)

   **IMPORTANT:** Make sure to deactivate the account after you complete the labs to avoid billing for usage.

1. Open Visual Studio Code from the Windows Start menu. Click on **Accounts > Backup and Sync Settings** and select **Sign in.**

   ![Image](./media/image45.png)

1. Select the **Sign in with GitHub** option.

   ![Image](./media/image46.png)

1. Select the Browser and Sign in with your Copilot enabled Github account.

   ![Image](./media/image47.png)

   ![Image](./media/image48.png)

1. Authenticate and verify with the code to complete Two-factor authentication.

   ![Image](./media/image49.png)

1. Click on Visual Studio Code.

   ![Image](./media/image50.png)

1. Click on Extension from the left navigation menu, search for `GitHub Copilot` chat, select it and click on **Install**.

   ![Image](./media/image51.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex1-task0-lab01-activate-copilot" />

### Task 1: Understand the API

In this task, you will learn what the application does before writing any tests. This task focuses on using **GitHub Copilot as a comprehension assistant** to analyze an existing REST API, identify endpoints, and document expected behavior.

1. Open **Visual Studio Code** from the desktop.

1. Sign in to GitHub via GitHub Enterprise:

   1. In VS Code, click the **Accounts** icon at the bottom-left of the Activity Bar.

   1. Click **Sign in with GitHub** (or **Sign in to GitHub Enterprise**).

   1. In the browser window that opens, sign in using the **GitHub Enterprise** credentials provided in the **Environment Details** tab of your lab guide:

      - **GitHub Enterprise URL:** `https://github.com`
      - **Username:** <inject key="GitHubUsername"></inject>
      - **Password:** <inject key="GitHubPassword"></inject>

   1. If prompted for **two-factor authentication**, check your registered email or authenticator app and enter the code.

   1. Click **Authorize Visual-Studio-Code** when prompted to grant VS Code access to your GitHub account.

   1. Return to VS Code - you should see your GitHub username appear in the **Accounts** section at the bottom-left, confirming successful authentication.

   > **Note:** If you already see your GitHub username in VS Code's Accounts menu, you are already signed in and can skip the sign-in steps above.

1. Click on **File -> Open Folder**, navigate to `C:\Labfiles`, and select the folder **github-copilot-workshops-labs-java**.

   > **Note:** The lab files have already been extracted to `C:\Labfiles` by the lab setup script - no manual extraction is required.

   ![Image](./media/image1.png)

1. Open the `01-testing -> java -> src -> main -> controller -> EmployeeController.java` api.

   ![Image](./media/image2.png)

1. Read the controller classes (example - @RestController, @RequestMapping, getMapping, etc.) of the API and identify:

   - **Base URL** (common path prefix used by all APIs in this controller)

   - **HTTP methods** (HTTP methods describe what you want to do with the resource (Employee))

   - **Request Body** (The Request Body is the data sent by the client to the server, usually in JSON format)

   - **Response** (The Response is what the API sends back to the client)

   ![Image](./media/image3.png)

1. Below are the base URL, HTTP methods, Request body and response from EmployeeController.java api:

   Base URL: **/api/employees**

   Http Methods:

   | **HTTP Method** | **Purpose** | **Endpoint** |
   |--|--|--|
   | GET | Retrieve data | /api/employees |
   | GET | Retrieve one record | /api/employees/{id} |
   | POST | Create new record | /api/employees |
   | PUT | Update existing record | /api/employees/{id} |
   | DELETE | Delete a record | /api/employees/{id} |

   Retrieve all employees - `public List<Employee> getAllEmployees()`

   Create new employee (PostMapping) - `public Employee createEmployee(@RequestBody Employee employee)`

   - **Request body:** `public Employee createEmployee(@RequestBody Employee employee)` – (PostMapping)
   - **Response by Endpoint:** Get all employees - `public List<Employee> getAllEmployees()` - (@GetMapping)

1. Select the entire EmployeeController.java file. Open **Copilot Chat in Ask mode with Claude Sonnet 4.5 model selected**. Paste the following prompt:

   ```
   Explain this Spring Boot REST controller
   Identify:
   - Base URL
   - All endpoints
   - HTTP methods
   - Request bodies
   - Response payloads
   Explain it as if I am preparing to write tests.
   ```

   ![Image](./media/image4.png)

1. You can see a response similar to the one below. Read the Copilot response and evaluate it:

   - **Base URL:** Employee Model (Request/Response shape) - All endpoints consume and produce Employee objects serialized as JSON

   | **Field** | **Type** | **Notes** |
   |--|--|--|
   | id | Long | Auto-generated (DB identity), not sent on create |
   | name | string | Required for meaningful data |
   | Surname | String | Required for meaningful data |
   | email | String | Required for meaningful data |

   - **Get All Employees**

   | **Method** | **GET** |
   |--|--|
   | URL | /api/employees |
   | Request body | None |
   | Response | 200 OK + JSON array of Employee objects (empty array [] if none exist) |

   - Similarly prepare for all other endpoints as shown in the image.

   ![Image](./media/image5.png)

1. Now you will use Copilot **Agent mode** to automatically generate and create an `api-docs.md` file documenting all endpoints with curl commands and expected behavior.

   1. In the Copilot Chat panel, switch the mode dropdown to **Agent**.

   1. Type the following prompt and press **Enter**:

      ```
      Using the EmployeeController.java file in this project, create a new file called api-docs.md in the lab-01-testing/java folder.

      For each endpoint in the controller, document:
      1. The HTTP method and endpoint URL
      2. A sample curl command showing the full HTTP request with example data
      3. The expected HTTP response code
      4. A brief description of what the endpoint does

      Format the file as clean Markdown with a heading for each endpoint.
      ```

      ![Image](./media/image6.png)

   1. Agent mode will analyze `EmployeeController.java`, generate the documentation, and **automatically create the `api-docs.md` file** in your project. You will see it appear in the Explorer panel on the left.

      ![Image](./media/image7.png)

   1. Once Agent mode finishes, click **Keep** to accept the created file. Open `api-docs.md` from the Explorer to verify it contains documentation for all 5 endpoints: GET all, GET by ID, POST (create), PUT (update), and DELETE.

      ![Image](./media/image10.png)

   > **Note:** Agent mode creates the file for you - no manual copy-pasting needed. If the file appears in the wrong location, you can drag it to the correct folder in Explorer.

### Task 2: Create Repository Layer Unit Tests

The repository layer is responsible for data persistence. This task focuses on testing data access logic in isolation, without involving business logic or REST endpoints.

1. Navigate to `src/test/java/com/example/demo` and create the file `EmployeeRepositoryTest.java`.

   ![Image](./media/image11.png)

   ![Image](./media/image12.png)

1. At the top of **EmployeeRepositoryTest.java**, add the below comment manually. Stop typing and wait - Copilot will start suggesting:

   - @DataJpaTest
   - Autowired repository
   - Sample save and find tests

   ```
   // Write JUnit tests for EmployeeRepository using @DataJpaTest.
   ```

   ![Image](./media/image13.png)

1. Open chat, select agent mode and Claude Sonnet 4.5 model, then enter the below prompt:

   ```
   Create JUnit 5 tests for EmployeeRepository
   Requirements:
   - Use @DataJpaTest
   - Test basic CRUD operations (save, findAll, findById, delete)
   - Use an in-memory database
   - Follow Spring Boot testing best practices
   ```

   ![Image](./media/image14.png)

1. Copilot will generate and update your test file.

   ![Image](./media/image15.png)

1. Review unit tests and click on **Keep** to accept tests.

   ![Image](./media/image16.png)

   ![Image](./media/image17.png)

1. Open a **Terminal -> Git Bash** from VS Code (go to **Terminal → New Terminal**, then switch to **Git Bash** from the dropdown in the terminal panel).

   ![Image](./media/image18a.png)

   ![Image](./media/image18b.png)

1. Navigate to the lab project folder:

    ```
    cd lab-01-testing/java
    ```
    > **Note:** This command navigates into the Maven project directory for Lab 01. All subsequent Maven commands must be run from this folder.

1. Set up Maven for this terminal session. Maven is pre-installed on the lab VM at the path below - run both commands to configure it:

    ```
    export MAVEN_HOME="/c/Users/Admin/Documents/maven-mvnd-1.0.5-windows-amd64/maven-mvnd-1.0.5-windows-amd64"
    ```

    ```
    export PATH="$MAVEN_HOME/bin:$PATH"
    ```

      > **Note:** These two commands only need to be run once per terminal session. If you open a new terminal later, run them again before using `mvn`.

1. Run the tests with Maven:

    ```
    mvn test
    ```

   ![Image](./media/image18.png)

   ![Image](./media/image19.png)

### Task 3: Create Service Layer Unit Tests

The service layer contains business logic and coordinates interactions with the repository. This task teaches how to test logic independently of infrastructure by mocking dependencies.

1. Navigate to **src/test/java/com/example/demo** and create a file with the name `EmployeeServiceTest.java` and enter the below prompt in Copilot Chat Agent mode:

   ```
   Create unit tests for EmployeeService
   Requirements:
   - Use Mockito
   - Mock EmployeeRepository
   - Use @Mock and @InjectMocks
   - Include positive and negative scenarios
   - Follow JUnit 5 best practices
   ```

   ![Image](./media/image20.png)

   ![Image](./media/image22.png)

1. Review and validate the generated tests and click on **Keep**:

   ```
   When GitHub Copilot generates unit tests
   - Always verify package declarations
   - Always verify import statements
   - Ensure imported classes exist in src/main/java
   - Fix any mismatches before running tests
   ```

   Copilot suggestions must be reviewed before execution.

   ![Image](./media/image23.png)

   ![Image](./media/image24.png)

1. Open the terminal and navigate to the path suggested by Copilot and run `mvn test`.

   ![Image](./media/image25.png)

   ![Image](./media/image26.png)

### Task 4: Create Controller Layer Unit Tests

The controller layer exposes the REST API. This task focuses on verifying HTTP behavior without starting the full application.

1. Navigate to `src/test/java/com/example/demo` and create a test class named `EmployeeControllerTest.java`.

   ![Image](./media/image27.png)

1. Open Copilot and enter the below prompt:

   ```
   Create unit tests for EmployeeController
   Use:
   - @WebMvcTest
   - MockMvc
   - Mock EmployeeService
   Test:
   - GET /api/employees
   - GET /api/employees/{id}
   - POST /api/employees
   Validate:
   - HTTP status codes
   - JSON response content
   ```

   ![Image](./media/image28.png)

1. Review the tests and click on **Keep** to add the controller class.

   ![Image](./media/image29.png)

1. When GitHub Copilot generates controller tests:

   1. Verify the test's `package` declaration
   1. It MUST match the main application package

   For example:
   - Main application: `com.example.demo`
   - Test class MUST also be in: `com.example.demo`

   If packages do not match, Spring Boot will fail to locate `@SpringBootApplication` and tests will not start.

   ![Image](./media/image30.png)

1. Run below command to test – `mvn test` (it will fail if package declaration is not matching).

   ![Image](./media/image31.png)

1. Select the test class and ask Copilot in Agent mode to fix the error with the command - `/fix`.

   ![Image](./media/image32.png)

1. GitHub Copilot may suggest improvements beyond fixing test failures, such as recommending better REST semantics (e.g., returning 404 instead of 200). These suggestions are advisory. Only apply them if the lab explicitly asks for API refactoring.

   ![Image](./media/image33.png)

1. Change the package from **package com.example;** to `package com.example.demo;` and then run `mvn test`.

   ![Image](./media/image34.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex1-task4-lab01-controller-tests" />

### Task 5: Add a New API Operation

This task simulates a real development scenario: extending an existing application with a new feature. You will add a new operation to find an employee by email and ensure it is properly tested at every layer.

1. Open Copilot Chat and enter the below prompt:

   ```
   Add a new feature to find an employee by email
   Requirements:
   - Add a repository method to find employee by email
   - Add a corresponding service method
   - Add a REST endpoint to fetch employee by email
   - Generate unit tests for repository, service and controller layers
   - Follow existing coding style
   ```

   ![Image](./media/image35.png)

1. Review the response and accept by clicking on **Keep**.

   ![Image](./media/image36.png)

1. **Review and accept the tests**.

   ![Image](./media/image37.png)

1. Review and accept the code changes to repository and service classes.

   ![Image](./media/image38.png)

   ![Image](./media/image39.png)

1. Now run the command `mvn clean test` to clean the build.

   ![Image](./media/image40.png)

   ![Image](./media/image41.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex1-task5-lab01-new-api-operation" />

## Review

In this exercise, you have completed the following:

   - Used Copilot to understand and document existing APIs
   - Written unit tests for repository, service, and controller layers
   - Used mocking to isolate logic and avoid brittle tests
   - Extended an application with new functionality using Copilot assistance
   - Analyzed and improved test quality and coverage

### You have successfully completed the exercise!
### In the Lab Guide section, click the **Next >>** button to proceed to Exercise 2.

![](media/up4.png)
