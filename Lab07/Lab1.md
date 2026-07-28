# Lab 1: Building a Task Management REST API with GitHub Copilot as an AI Pair Programmer

### Estimated Duration: 120 Minutes

## Overview

In this lab, you will build a Python Flask Task Management REST API from an empty repository, using GitHub Copilot to accelerate scaffolding, feature development, and testing. You will practice deciding when to accept, refine, or reject Copilot's suggestions while owning the quality of every line shipped.

You have just joined **ZavaTask Inc.**, a mid-size SaaS company. Your team lead has assigned you to build a **Task Management REST API** from scratch — a backend microservice that lets users create, retrieve, update, and delete tasks. The codebase is an **empty repository** with no business logic. You are expected to go from zero to a working, tested, and documented service in a single sprint.

Your manager says: *"Use GitHub Copilot to accelerate, but you own the quality. Every line of code ships under your name."*

This lab simulates that real-world scenario. You will scaffold, build, test, and document the service — using GitHub Copilot as your AI pair programmer at every step — while learning **when to accept, when to refine, and when to reject** its suggestions.

## Objectives

In this lab, you will complete the following tasks:

   - Task 1: Understand the Problem (Human Reasoning — No Copilot Yet)
   - Task 2: Scaffold the Project Using Natural Language Comments
   - Task 3: Add a Helper Function via Comment Prompt
   - Task 4: Build Application Features Using Intent-Driven Prompts
   - Task 5: Get All Tasks Endpoint (Copilot Chat)
   - Task 6: Get Single Task Endpoint (Inline Comment)
   - Task 7: Update Task Endpoint
   - Task 8: Delete Task Endpoint
   - Task 9: Add the Application Entry Point
   - Task 10: Review and Refine Copilot Output — Refactoring
   - Task 11: Use /fix for Any Issues
   - Task 12: Generate Unit Tests with Copilot
   - Task 13: Generate Tests Using Copilot Chat /tests
   - Task 14: Generate Remaining Tests

#### **Pre-Lab Setup Steps**

1. Create a new empty folder named **Lab01** in your `C:\` drive.

   ![Image](./media/image1.png)

1. Open the folder in Visual Studio Code. Sign in with your GitHub Copilot-enabled account.

   ![Image](./media/image2.png)

1. Open **Terminal → New Terminal** in VS Code and click the dropdown to select **Git Bash**. Run the below command to create the project folder:

   ```
   mkdir cloudtask-api && cd cloudtask-api
   ```

   ![Image](./media/image3.png)

1. **Initialize a Git repository:**

   ```
   git init
   ```

   ![Image](./media/image4.png)

1. **Create a Python virtual environment and activate it:**

   ```
   python -m venv venv
   ```

   ```
   source venv/Scripts/activate
   ```

   ![Image](./media/image5.png)

   > **Note:** The `source venv/Scripts/activate` command is the correct syntax for activating a virtual environment in Git Bash on Windows. You should see `(venv)` prepended to your terminal prompt after activation.

1. Make sure GitHub Copilot is enabled and signed in.

### Task 1: Understand the Problem (Human Reasoning — No Copilot Yet)

In this task, you will reason through the API's architecture without Copilot, answering questions about endpoints, data fields, framework, and testing tools. You will decide on Flask and pytest before writing any code.

1. What endpoints does a Task Management API need?

   - GET /tasks — List all tasks

   - POST /tasks — Create a new task

   - GET /tasks/\<id\> — Get a single task

   - PUT /tasks/\<id\> — Update a task

   - DELETE /tasks/\<id\> — Delete a task

1. What data fields does a "Task" have?

   - id (auto-generated)

   - title (string, required)

   - description (string, optional)

   - status (string: "pending", "in-progress", "completed")

   - created_at (datetime)

1. What framework will you use?

   - **Flask** (lightweight, well-known)

1. What testing framework?

   - **pytest**

      > **Note:** This task is intentionally Copilot-free. The goal is to establish that developers must *think first*. A prompt is a request that you make to GitHub Copilot. In addition to your prompt, Copilot uses additional context, like the code in your current file and the chat history, to generate a response. The *quality* of that context starts with the developer's understanding of the problem.

### Task 2: Scaffold the Project Using Natural Language Comments

In this task, you will use Copilot Agent mode to generate a requirements.txt for a production-ready Flask REST API. You will review the suggested packages, remove anything unnecessary, and install the dependencies.

1. Open **Copilot Chat** (**Ctrl+Shift+I** / **Cmd+Shift+I**), select **Agent** mode and the **Claude Sonnet 4.6** model, then type the following prompt:

   ```
   I'm building a Task Management REST API using Python and Flask. I need a
   requirements.txt file with Flask, pytest, and any essential dependencies
   for a production-ready Flask REST API.
   ```

   ![Image](./media/image6.png)

1. Copilot should suggest a requirements.txt with packages such as flask, pytest, flask-cors, and possibly python-dotenv.

   ![Image](./media/image7.png)

1. Review the list. Accept packages you recognize. **Remove** any package you don't understand or don't need yet. Create the file `requirements.txt` and paste the accepted content, or if using Agent mode, Copilot will create the file automatically.

   ![Image](./media/image7.png)

1. In the terminal, install the required packages:

   ```
   pip install -r requirements.txt
   ```

   ![Image](./media/image8.png)

   > **Note:** Follow good coding practices. If you aren't getting the responses you want when you ask Copilot for suggestions or explanations in your codebase, make sure that your existing code follows best practices and is easy to read. Starting with a clean requirements.txt sets the foundation.

### Task 3: Add a Helper Function via Comment Prompt

In this task, you will write a natural-language comment to prompt Copilot into generating a find_task helper function. You will review the suggestion and refine your comment if Copilot produces redundant logic.

1. Create a new file `app.py` in the `cloudtask-api` folder. Below any data store or imports, type the following comments and wait for Copilot's inline suggestion. Press **Tab** to accept each suggestion:

   ```
   # Helper function to find a task by its ID from the tasks list
   ```

   ```
   # Returns the task dict if found, or None if not found
   ```

   ![Image](./media/image14.png)

   ![Image](./media/image15.png)

   ![Image](./media/image16.png)

1. Copilot should suggest a function like:

   ```python
   def find_task(task_id):
       return next((task for task in tasks if task['id'] == task_id), None)
   ```

   ![Image](./media/image16.png)

1. Review the suggestion. Does it use a for loop or `next()` with a generator? Either is acceptable. Imports should always be at the top of the file. Move them up if needed.

   ![Image](./media/image17.png)

1. If Copilot suggests both a loop **and** `next()` redundantly, **reject** the suggestion and retype a cleaner comment:

   ```
   # Find and return a task by ID using next() with a generator expression, return None if not found
   ```

   > **Note:** If you don't get the result that you want, iterate on your prompt and try again. Refining the comment gives better results.

### Task 4: Build Application Features Using Intent-Driven Prompts

In this task, you will use inline comment prompts to have Copilot generate the POST /tasks create-task endpoint with validation and auto-generated fields. You will review the code for correctness and manually add any missing imports.

#### **Create Task Endpoint (Inline Comment)**

1. In **app.py**, below the helper function, type the following inline comments manually to create the task endpoint:

   ```
   # POST /tasks - Create a new task
   # Accept JSON body with 'title' (required) and 'description' (optional)
   # Auto-generate 'id' using uuid4, set 'status' to 'pending', set 'created_at' to current UTC time
   # Return the created task with 201 status code
   # If 'title' is missing, return a 400 error with a message
   ```

   ![Image](./media/image18.png)

   ![Image](./media/image19.png)

1. Copilot should generate a full `@app.route('/tasks', methods=['POST'])` function.

1. Review the generated code line-by-line:

   - Does it import uuid and datetime (or use them)?

   - Does it validate that title is present?

   - Does it return 201 status code?

   - If it doesn't validate title, **reject** and add: `# Validate that 'title' is present in the request body`

   - Accept the function if it meets requirements.

      > **Note:** Copilot may generate code using uuid4 and datetime but may **not** add the import statements at the top of the file. **This is intentional.**

1. Add the missing imports manually at the top of the file:

   ```python
   from uuid import uuid4
   from datetime import datetime, timezone
   ```

### Task 5: Get All Tasks Endpoint (Copilot Chat)

In this task, you will use Copilot Chat's @workspace context to generate a GET /tasks endpoint that returns all tasks as JSON. You will verify it matches the style and patterns of the existing code.

1. Open **Copilot Chat** in **Agent** mode and enter the below prompt:

   ```
   @workspace Open [app.py] and add a GET /tasks endpoint that returns all tasks as JSON with HTTP 200. Follow the existing style and patterns already used in the file. Keep changes minimal and only update [app.py]. Use Flask route style consistent with current code, and return jsonify(tasks), 200.
   ```

   ![Image](./media/image20.png)

   ![Image](./media/t5s1.png)

1. Copilot should generate a GET /tasks route that returns `jsonify(tasks)`.

1. Review the suggestion in the Chat panel. Click **Keep** to accept the change.

   ![Image](./media/image22.png)

1. Verify it follows the same style (decorator pattern, response format) as the POST endpoint.

   > **Note:** Using @workspace allows Copilot to have a broader context window to analyze your full project. This helps Copilot match existing patterns — both for feature code and for tests.

### Task 6: Get Single Task Endpoint (Inline Comment)

In this task, you will write inline comments to prompt Copilot into generating a GET /tasks/\<task_id\> endpoint using the find_task helper. You will accept the generated function if it correctly handles the not-found case.

1. Type the below comments manually in `app.py`:

   ```
   # GET /tasks/<task_id> - Get a single task by ID
   # Use the find_task helper function
   # Return 404 with error message if task not found
   ```

   ![Image](./media/image23.png)

   ![Image](./media/image24.png)

   ![Image](./media/image25.png)

   ![Image](./media/image26.png)

1. Let Copilot generate the function. Accept it if it correctly uses `find_task` and returns 404 when the task is not found.

### Task 7: Update Task Endpoint

In this task, you will use Copilot Chat to generate a PUT /tasks/\<task_id\> endpoint that validates status values and updates only provided fields. You will critically review the validation logic and refactor hardcoded statuses into a constant.

1. Enter the below prompt in **Copilot Chat** in **Agent** mode:

   ```
   @workspace Open app.py and add a PUT /tasks/<task_id> endpoint following the same style and patterns already in the file. Keep changes minimal and only edit app.py.
   ```

   ![Image](./media/image27.png)

1. The endpoint should:

   - Accept JSON with optional fields: title, description, status

   - Only update fields that are provided in the request body

   - Validate that 'status' is one of: pending, in-progress, completed

   - Return 404 if task not found

   - Return 400 if invalid status value

   - Return the updated task with 200 status

   Follow the coding patterns already in the file.

   ![Image](./media/image28.png)

1. A complete PUT route with field-level updates and validation. **Critically review** the status validation logic. Does Copilot use an if check? A set? An enum?

   - If Copilot hardcodes the valid statuses as a list inside the function, consider refactoring them into a constant at the top of the file:

   - `VALID_STATUSES = {"pending", "in-progress", "completed"}`

     ![Image](./media/image29.png)

1. Move this constant to the top of the file. This is **your** architectural decision, not Copilot's.

   > **Note:** This is the "Balancing speed, ownership, and code quality" objective in action. Start general, then get specific. When writing a prompt for Copilot, first give Copilot a broad description of the goal or scenario. Then list any specific requirements. Notice how the detailed prompt produced a more accurate result than a vague one would.

### Task 8: Delete Task Endpoint

In this task, you will write your own comment-driven prompt to have Copilot generate a DELETE /tasks/\<task_id\> endpoint. You will verify it removes the task, returns 404 when not found, and returns 204 on success.

1. Write your own comment-driven prompt in `app.py` for a DELETE /tasks/\<task_id\> endpoint.

   Requirements:

   - Remove the task from the in-memory list

   - Return 404 if not found

   - Return 204 (No Content) on success

   Write your own comment block in `app.py` and let Copilot generate the code. Review before accepting.

   **Hint — Example Comment:**

   ```
   # DELETE /tasks/<task_id> - Delete a task by ID
   # Use the find_task helper to locate the task
   # Remove the task from the tasks list
   # Return 404 if task not found
   # Return 204 No Content on successful deletion
   ```

### Task 9: Add the Application Entry Point

In this task, you will prompt Copilot to generate the `if __name__ == '__main__':` block to run the Flask app in debug mode on port 5000. You will start the app and test it with curl to confirm the endpoints work.

1. At the bottom of `app.py`, type the following comment and press **Enter**. Accept Copilot's suggestion for the `if __name__ == '__main__':` block:

   ```
   # Run the Flask app in debug mode on port 5000
   ```

   ![Image](./media/image30.png)

   ![Image](./media/image31.png)

1. Review the generated code. If the POST endpoint returns status code 200 instead of 201, update it to return 201 (the correct HTTP status for resource creation).

1. Save the file and run the app in the terminal:

   ```
   cd cloudtask-api/
   ```

   ```
   python app.py
   ```

   ![Image](./media/image32.png)

1. Open a second terminal (or duplicate workspace) and run the below commands to test the API:

   ```
   curl -X POST http://localhost:5000/tasks     \
   -H "Content-Type: application/json"     \
   -d '{"title": "My first task", "description": "Testing the API"}'
   curl http://localhost:5000/tasks
   ```

   ![Image](./media/image33.png)

   ![Image](./media/image34.png)

### Task 10: Review and Refine Copilot Output — Refactoring

In this task, you will ask Copilot to review the full app.py for code quality and suggest refactors like shared error-response helpers. You will accept improvements that reduce duplication while rejecting any that change the API contract.

1. Select all the code in `app.py`, then open **Copilot Chat** and enter the below prompt:

   ```
   Review this Flask application for code quality improvements.
   Suggest refactoring for:
   1. Extracting repeated validation logic into helper functions
   2. Consistent error response formatting
   3. Improving readability and maintainability
   - Do not change the API contract or endpoints.
   ```

   ![Image](./media/image36.png)

1. Copilot should suggest:

   - A `make_error_response(message, status_code)` helper

   - Possibly extracting validation into a `validate_task_data()` function

   - Consistent JSON error format like `{"error": "message"}`

      ![Image](./media/image37.png)

1. Accept helpers that **reduce duplication**. **Reject** any suggestion that changes the API contract (e.g., changing route paths or HTTP methods).

1. Apply changes incrementally — one refactor at a time.

### Task 11: Use /fix for Any Issues

In this task, you will highlight any buggy or warning-flagged code and use Copilot's /fix command to get a proposed correction. You will review each fix carefully before applying it, since Copilot may alter intentional behavior.

1. If you notice any linter warnings or bugs, highlight the code and type in **Copilot Chat**:

   ```
   /fix
   ```

   > **Note:** The `/fix` command tells Copilot to analyze the selected code for bugs and propose corrections. Always review the fix before applying — Copilot may "fix" something that was intentional.

### Task 12: Generate Unit Tests with Copilot

In this task, you will create test_app.py and use comment prompts to have Copilot generate pytest-based unit tests for the CRUD operations and edge cases. You will accept the tests incrementally rather than all at once.

1. Create a new file `test_app.py` in the root `cloudtask-api` folder. Type the following comments manually at the top and pause to observe Copilot's suggestions. It may try to generate all tests at once:

   ```
   # Unit tests for the Task Management REST API
   ```

   ```
   # Using pytest and Flask's test client
   ```

   ```
   # Test all CRUD operations: create, read, update, delete
   ```

   ```
   # Include edge cases: missing title, invalid status, task not found
   ```

   ![Image](./media/image38.png)

   ![Image](./media/image39.png)

   ![Image](./media/image40.png)

   ![Image](./media/image41.png)

1. Do **NOT** accept the full block. Instead, proceed with one test at a time in the following tasks.

### Task 13: Generate Tests Using Copilot Chat /tests

In this task, you will select the create_task function and use Copilot Chat's /tests command to generate targeted pytest cases for it. You will review each test's assertions before copying the accepted ones into test_app.py.

1. Open `app.py`, select the **Create Task** endpoint function. Then in **Copilot Chat**, enter the following prompt:

   ```
   /tests Generate pytest unit tests for the selected create_task function.
   Include tests for:
   - Successfully creating a task with title and description
   - Creating a task with only a title (no description)
   - Attempting to create a task without a title (expect 400)
   Use Flask's test client.
   ```

   ![Image](./media/image42.png)

1. Copilot should generate a test file with:

   - A client fixture using `app.test_client()`

   - Three test functions following the Arrange-Act-Assert pattern

     ![Image](./media/image43.png)

1. Review each test:

   - Does the fixture correctly import app from app.py?

   - Are assertions checking the right status codes (201, 400)?

   - Are response bodies validated (not just status codes)?

   - Copy accepted tests into `test_app.py`.

     ![Image](./media/image44.png)

     > **Note:** Review suggestions carefully. Just like with human-generated code, never trust any tests Copilot generates without going through your normal review process.

### Task 14: Generate Remaining Tests

In this task, you will use Copilot Chat's /tests command to generate tests for the remaining endpoints, such as GET /tasks. You will run the full test suite with pytest and use /fix to resolve any failures.

1. Use **Copilot Chat** to generate tests for additional endpoints using prompts like:

   ```
   /tests Generate pytest tests for the GET /tasks endpoint. Include a test
   for an empty task list and a test after creating two tasks.
   ```

   ![Image](./media/image45.png)

1. Run tests with the command below. All tests should pass:

   ```
   pytest test_app.py -v
   ```

   ![Image](./media/image46.png)

   **If tests fail:**

   - Read the error message.

   - Highlight the failing test in VS Code.

   - Ask Copilot Chat:

   - `/fix This test is failing with the following error: [paste error]. The test is for the create_task endpoint. Fix the test.`

   - Review the fix, apply it, re-run.

## Review

In this lab, you have completed the following:

   - Reasoned about architecture before using Copilot (human-first approach)
   - Scaffolded a Flask REST API using natural language comments and Copilot Chat
   - Built all CRUD endpoints using inline comment prompts and Copilot Chat prompts
   - Refactored the application to improve code quality and consistency
   - Generated unit tests using Copilot's /tests command and inline suggestions

### You have successfully completed the lab!
### In the Lab Guide section, click the **Next >>** button to proceed to Lab 2.

![](./media/nx.png) 
