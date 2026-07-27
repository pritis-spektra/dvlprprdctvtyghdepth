# Lab 8: Improving Code Quality and Maintainability with GitHub Copilot

### Estimated Duration: 90 Minutes

## Overview

In this lab, you will inherit a poorly written order processing module and use GitHub Copilot to understand, refactor, test, and document it. You will maintain a code-review mindset throughout, validating every refactor against a characterization test safety net.

You've just been assigned to the **"OrderFlow"** team at a mid-size e-commerce company. The previous developer has left abruptly. You've inherited **a working but poorly written order processing module** — **order_processor.py**. The code runs, the business depends on it, and **you are now the owner**.

Your tech lead's message reads:

*"The order processor works, but it's a mess. No tests, no docs, cryptic names everywhere, copy-paste logic all over the place. We need you to clean it up before we can add the new discount feature next sprint. Don't break anything."*

In this lab, you'll use GitHub Copilot as your AI pair programmer to **understand, refactor, test, and document** inherited code while maintaining a **code review mindset** at every step.

## Objectives

In this lab, you will complete the following tasks:

   - Task 1: Understand the Problem — Human Reasoning First
   - Task 2: Use GitHub Copilot to Analyze and Explain the Code
   - Task 3: Ask Copilot to Identify Code Smells
   - Task 4: Ask Copilot to Compare the Two Functions
   - Task 5: Refactor the Code Using Copilot — Incremental, Validated Changes

### Task 1: Understand the Problem — Human Reasoning First

In this task, you will clone the inherited order_processor.py codebase and manually read through it without Copilot's help. You will list out code smells you spot on your own to build a code-review mindset first.

1. Create a folder in your `C:/Lab08` and open it in Visual Studio Code and sign in with your GitHub account with Copilot license. Open terminal -> GitBash.

   ![Image](./media/image1.png)

1. Run the below command and clone the repo:

   ```
   git clone https://github.com/technofocus-pte/GitHub-Copilot-orderflow-cleanup
   ```

   ![Image](./media/image2.png)

1. Open **order_processor.py** and create a list of code smells on paper or in a scratch comment block at the bottom of the file.

   > **Note:** Spend 5 minutes and identify as many issues as you can.

   ```
   # === CODE REVIEW NOTES ===
   # 1. Variable names: d, o, t, p, q, r, s, tt — all cryptic, single-letter
   # 2. proc() and proc_batch() contain nearly identical logic (copy-paste duplication)
   # 3. Discount rates are hardcoded magic numbers (0.1, 0.2, 0.25)
   # 4. Tax rate (0.08) is a magic number buried in logic
   # 5. No docstrings, no type hints, no meaningful comments
   # 6. Empty order check happens AFTER calculation (wrong order of operations)
   # 7. gen_report() uses string concatenation instead of f-strings or templates
   # 8. No unit tests exist
   # 9. No README or module-level documentation
   ```

   ![Image](./media/image3.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex8-task1-lab08-code-review" />

### Task 2: Use GitHub Copilot to Analyze and Explain the Code

In this task, you will select the proc() function and use Copilot's /explain command to get a plain-English breakdown of its behavior. You will check whether Copilot catches the empty-order ordering bug, learning that /explain describes current behavior, not correct behavior.

1. Select the **entire proc() function** in the editor. Open Copilot Chat (Ctrl+Shift+I / Cmd+Shift+I) and type:

   ```
   /explain
   ```

   ![Image](./media/image4.png)

1. Copilot should provide a plain-English explanation of what the function does: processes an order by calculating subtotal, applying customer-type discounts, adding tax, and returning a result dictionary.

   Before you modify existing code, make sure you understand its purpose and how it currently works. Copilot can help you with this.

1. Read the explanation carefully. Does Copilot identify the **empty-order bug** (the empty check happens after calculation)? If not, note this — Copilot explained what the code *does*, not what it *should* do. **This is a critical distinction.**

   > **Note:** /explain describes *current behavior*, not *intended behavior*. The developer must judge correctness.

### Task 3: Ask Copilot to Identify Code Smells

In this task, you will use Copilot Chat in Agent mode to review order_processor.py and list code smells ranked by severity. You will compare Copilot's findings against your own notes to see what each of you caught that the other missed.

1. With **order_processor.py** open, type in Copilot Chat in Agent mode with Claude Sonnet 4.6 model:

   **Prompt:**
   ```
   @workspace Review order_processor.py for code quality issues.
   List all code smells including: poor naming, duplication, magic numbers,
   missing error handling, structural problems, and maintainability
   concerns.
   Rank them by severity.
   ```

   ![Image](./media/image6.png)

1. Copilot should identify most (or all) of the issues you noted in Step 1, possibly organized by category and severity.

1. Compare Copilot's list against your own notes from Step 1:

   - Did Copilot find anything you missed? (Possibly: the lack of input validation on `o["id"]`, or no error handling if `items` key is missing.)

   - Did you find anything Copilot missed? (Possibly: the **logical ordering bug** — the empty check happens too late.)

      ![Image](./media/image7.png)

      ![Image](./media/image8.png)

      > **Note:** This comparison exercise demonstrates the **human-AI collaboration** model. Neither alone catches everything. Together, coverage is far more complete.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex8-task3-lab08-code-smells" />

### Task 4: Ask Copilot to Compare the Two Functions

In this task, you will ask Copilot to compare proc() and proc_batch() and identify their duplicated logic. You will confirm Copilot's suggestion that proc_batch() should simply call proc() for each order.

1. Enter the below prompt to compare the two functions and understand the response:

   **Prompt:**
   ```
   Compare the proc() and proc_batch() functions in order_processor.py.
   Identify duplicated logic and explain how they could be consolidated.
   ```

   ![Image](./media/image9.png)

1. Copilot should note that proc_batch() contains an **exact copy** of proc()'s logic inside a loop, and suggest that proc_batch() should simply call proc() for each order.

1. This is a correct architectural observation. Copilot's refactoring direction is sound. We will act on it in the next task.

   ![Image](./media/image10.png)

### Task 5: Refactor the Code Using Copilot — Incremental, Validated Changes

In this task, you will build a characterization test safety net, then refactor the code in incremental passes renaming variables, extracting magic numbers into constants, deduplicating proc_batch(), and extracting the discount logic into a helper. You will re-run the test suite after every change to confirm behavior stays identical.

We'll refactor in **five incremental passes**, creating a test safety net first, then improving one dimension at a time.

> Never refactor without tests. We'll write a "characterization test" first — a test that locks in the *current* behavior, even if it's imperfect.

#### **Create a Characterization Test (Safety Net)**

1. Create a new file: `test_order_processor.py` in the root folder. Type the following comment at the top and let Copilot help:

   ```
   # Characterization tests for order_processor.py
   ```

   ```
   # These tests capture the CURRENT behavior of the code before refactoring
   ```

   ```
   # Purpose: ensure refactoring does not change observable behavior
   ```

   ![Image](./media/image11.png)

   ![Image](./media/image12.png)

1. Now open Copilot Chat, and type:

   **Prompt:**
   ```
   @workspace Generate characterization tests for the proc() function
   in order_processor.py. These tests should capture the current behavior,
   not the ideal behavior. Include tests for:
   1. A standard order with 2 items and no discount
   2. A VIP order (customer type "VIP")
   3. An EMPLOYEE order
   4. An order with an empty items list
   5. An order where item quantity is negative
   Use pytest. Calculate the expected values manually to match current code
   behavior.
   ```

   ![Image](./media/image13.png)

1. Copilot should generate 5 test functions with hardcoded expected values that match the current proc() output.

   ![Image](./media/image14.png)

1. Manually verify at least 2 expected values with a calculator. For example, for a VIP order with items `[{"p": 100, "q": 2}]`:

   - Subtotal: 200

   - VIP discount (10%): 200 - 20 = 180

   - Tax (8%): 180 × 0.08 = 14.40

   - Total: 194.40

   - If Copilot's expected values don't match your manual calculation, **reject and correct**.

   - Verify the "st" (status) field assertions match the threshold logic.

1. **Open Terminal and run the tests:**

   ```
   cd GitHub-Copilot-orderflow-cleanup/
   ```

   ```
   pytest test_order_processor.py -v
   ```

   ![Image](./media/image15.png)

1. All 5 tests pass. If any fail, the expected values need correction — fix them now. These tests are your safety net.

   > **Note:** Review suggestions carefully. This is especially important for characterization tests — wrong expected values mean your safety net has holes.

#### **Refactor 1: Fix Variable Names**

1. Open order_processor.py file and select the **entire proc() function**. Open **Copilot inline chat** (Ctrl+I / Cmd+I) and type:

   **Prompt:**
   ```
   Improve all variable names in this function to be descriptive and
   readable.
   Rename: d→discount_rate, o→order, t→subtotal, p→price, q→quantity,
   r→result, i→item. Keep the exact same logic and behavior.
   ```

   ![Image](./media/image16.png)

1. It should rename all single-letter variables to descriptive names while preserving exact logic.

1. VS Code will show you an inline diff. Check:

   - Are **all** single-letter variables renamed?

   - Is the logic **identical** — same calculations, same conditionals?

   - Did Copilot accidentally change any computation?

   - Did Copilot rename the function itself?

1. **Accept** the suggestion if the logic is preserved.

   ![Image](./media/image17.png)

1. Then immediately **run tests:**

   ```
   pytest test_order_processor.py -v
   ```

   ![Image](./media/image18.png)

1. All tests still pass. If any fail, **reject the refactor**, undo (Ctrl+Z), and re-prompt with more specificity.

#### **Refactor 2: Extract Magic Numbers into Constants**

1. At the top of order_processor.py, type the following comment and let Copilot suggest:

   ```
   # Constants for discount rates and tax
   # VIP customers get 10% discount
   # Employee customers get 20% discount
   # Wholesale customers get 25% discount
   # Tax rate is 8%
   # Orders over $1000 require review
   ```

   ![Image](./media/image19.png)

1. Copilot should suggest:

   ```
   VIP_DISCOUNT_RATE = 0.10
   EMPLOYEE_DISCOUNT_RATE = 0.20
   WHOLESALE_DISCOUNT_RATE = 0.25
   TAX_RATE = 0.08
   REVIEW_THRESHOLD = 1000
   ```

   ![Image](./media/image20.png)

1. Accept this suggestion — it's clean and follows Python conventions (UPPER_SNAKE_CASE).

1. Now use inline chat to ask Copilot to help replace magic numbers in proc(). Select the proc() function:

   **Prompt:**
   ```
   Replace all magic numbers in this function with the constants defined
   at the top of the file: VIP_DISCOUNT_RATE, EMPLOYEE_DISCOUNT_RATE,
   WHOLESALE_DISCOUNT_RATE, TAX_RATE, REVIEW_THRESHOLD.
   Do not change any logic.
   ```

   ![Image](./media/image21.png)

   ![Image](./media/image22.png)

1. **Review and accept.** Then immediately run:

   ```
   pytest test_order_processor.py -v
   ```

   ![Image](./media/image23.png)

1. All tests pass.

#### **Refactor 3: Eliminate Duplication Between proc() and proc_batch()**

1. This is the most impactful refactor. Enter the below prompt in the chat:

   **Prompt:**
   ```
   @workspace The proc_batch() function in order_processor.py contains
   duplicated logic from proc(). Refactor proc_batch() so it simply calls
   proc() for each order in the list. Keep the return type the same (list
   of results).
   Do not modify proc() itself.
   ```

   ![Image](./media/image24.png)

1. Copilot should suggest:

   ```
   def proc_batch(orders):
       return [proc(order) for order in orders]
   ```

   ![Image](./media/image25.png)

1. Accept the suggestion. But we need a **test for proc_batch()** too. In Copilot Chat:

   **Prompt:**
   ```
   /tests Generate a pytest test for proc_batch() that processes 3 orders
   (one standard, one VIP, one EMPLOYEE) and verifies that the result is a
   list of 3 results with correct order_ids.
   ```

   ![Image](./media/image26.png)

1. Add the generated test to test_order_processor.py, review it, and run:

   ```
   pytest test_order_processor.py -v
   ```

   ![Image](./media/image27.png)

   All tests pass, including the new batch test.

#### **Refactor 4: Extract Discount Calculation into a Helper**

1. The discount logic inside proc() uses an if/elif chain that will grow as new customer types are added. Let's extract it.

1. Select the discount if/elif block inside proc() and open inline chat:

   **Prompt:**
   ```
   Extract the discount calculation into a separate function called
   calculate_discount(subtotal, customer_type) that returns the discounted
   subtotal. Use a dictionary mapping instead of if/elif chains.
   ```

   ![Image](./media/image28.png)

1. Copilot suggests using a dictionary to map customer types to their corresponding discount rates.

   ![Image](./media/image29.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex8-task5-lab08-refactoring" />

## Review

In this lab, you have completed the following:

   - Analyzed inherited code for code smells before using Copilot
   - Used Copilot /explain to understand existing behavior
   - Identified and ranked code quality issues using Copilot Chat
   - Created characterization tests as a safety net before refactoring
   - Applied incremental refactoring passes: variable naming, constants, deduplication, and helper extraction
   - Validated all changes with a test-then-refactor-then-test cycle

### You have successfully completed the lab!
### In the Lab Guide section, click the **Next >>** button to proceed to Lab 9.

![](media/up4.png)
