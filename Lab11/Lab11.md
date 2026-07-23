## Exercise 11: Fixing a Production Incident Using GitHub Copilot Agent Mode (Optional)

### Estimated Duration: 90 Minutes

## Overview

It's **9:47 AM on a Monday.** You're an on-call engineer at **ZAVA PayStream Inc.**, a fintech company processing merchant payouts. The #incident-critical Slack channel just fired:

**[SEV-1] Payout Processing Service — Multiple Failures**

- Merchants report: payouts not arriving

- Monitoring: 37% of payout requests returning 500 errors

- Logs: TypeError, KeyError, and malformed amounts in database

- QA: 4 of 9 unit tests now failing after Friday's deploy

- Security team flagged: one endpoint may accept negative payout amounts

- **Business impact: $42K/hour in delayed merchant settlements**

Your TIM says: *"We need this fixed in the next 90 minutes. Use every tool you have."*

You open VS Code. You have GitHub Copilot with Agent Mode.

## Objectives

In this exercise, you will complete the following tasks:

   - Exercise 1, Task 1: TRIAGE — Human Reasoning: Read the Incident Log First
   - Exercise 1, Task 2: Use Copilot Chat to Confirm the Diagnosis
   - Exercise 1, Task 3: Use /explain on the Most Dangerous Code
   - Exercise 2, Task 1: Agent Mode — Fix the Critical Security Vulnerability
   - Exercise 2, Task 2: Agent Mode — Fix the Fee Calculation Bug
   - Exercise 2, Task 3: Agent Mode — Fix the Key Mismatch Bug
   - Exercise 3, Task 1: Fix the Missing Fields Test

### Task 1: TRIAGE — Human Reasoning: Read the Incident Log First

1. Open Visual Studio Code -> Terminal -> Git Bash and run the below command to clone the repo:

   +++git clone https://github.com/technofocus-pte/paystream-incident.git+++

1. Open **incident_log.txt** and read every line. Before touching Copilot, create a triage list.

   Categorize the issues by severity:

   **CRITICAL (fix first — financial loss):**

   - Negative payout amounts accepted → credits merchant instead of debiting

   - calculate_fee returns None for GBP → TypeError crashes processing

   **HIGH (fix next — service availability):**

   - POST /payouts crashes with KeyError when fields are missing

   - GET /merchants/\<id\>/payouts crashes — 'amount' vs 'amt' key mismatch

   **MEDIUM (fix after — test reliability):**

   - 4 failing tests

   - Missing test coverage for GBP fees and duplicate processing

   **LOW (fix last — code quality):**

   - Inconsistent key naming ('amt' vs 'amount')

   - No idempotency on process_payout

   ![Image](./media/image1.png)

   > **Note:** This triage step is **mandatory and Copilot-free**. In real incidents, developers must think before acting. Copilot's agentic capabilities don't replace your judgment in these situations — they amplify it. The triage order (critical → high → medium → low) mirrors real incident response.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex11-task1-lab11-triage" />

### Task 2: Use Copilot Chat to Confirm the Diagnosis

1. Open Copilot Chat and enter the below prompt in Agent mode:

   **Prompt:**

   ```
   @workspace I'm investigating a production incident. Read incident_log.txt and cross-reference it with payout_models.py and payout_api.py.
   For each error in the log, identify:
   1. The exact line of code causing the error
   2. The root cause
   3. Suggested severity (critical/high/medium/low)
   Present as a table.
   ```

   ![Image](./media/image3.png)

1. Copilot should produce a table mapping each log entry to specific code lines and root causes.

   **Developer Action:**

   - Compare Copilot's table against your manual triage.

   - Does Copilot correctly identify the **negative amount security issue** as critical?

   - Does Copilot catch the **'amt' vs 'amount' inconsistency** across files?

   ![Image](./media/image4.png)

   ![Image](./media/image5.png)

> **Note:** "Once you've identified the problem area, you can turn to GitHub Copilot and ask, 'I'm giving this input but getting this output — what's wrong?' That's where GitHub Copilot really shines." The key learning: Copilot confirms and enriches your diagnosis — but the triage *priority* is a human decision based on business impact.

### Task 3: Use /explain on the Most Dangerous Code

1. Select the **calculate_fee** function in **payout_models.py.** Type in Copilot Chat:

   **Prompt:**
   +++/explain What happens when an unsupported currency like "GBP" is passed to this function? Trace the downstream impact.+++

   ![Image](./media/image6.png)

1. Copilot should explain that the function returns None, which then causes `p["amt"] - None` to raise a TypeError in process_payout(). Confirm this matches log entry #4. Root cause confirmed. Now we fix.

   ![Image](./media/image7.png)

   ![Image](./media/image8.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex11-task3-lab11-explain-dangerous-code" />

### Task 4: Agent Mode — Fix the Critical Security Vulnerability

1. Open the Copilot Chat panel. Select **"Agent"** from the dropdown and enter the below prompt:

   **Prompt:**
   ```
   INCIDENT FIX - CRITICAL PRIORITY
   In payout_models.py, the create_payout function accepts negative
   amounts.
   This is a security vulnerability — negative payouts credit merchants
   instead of debiting them (see incident_log.txt).
   Fix this by:
   1. Adding input validation in create_payout() to reject amounts <= 0
   2. Adding input validation to reject empty or None merchant_id
   3. Raising a ValueError with clear messages for invalid inputs
   4. In payout_api.py, catching ValueError in the POST /payouts endpoint and returning a 400 response with the error message
   5. Do NOT change any existing test expectations
   6. After making changes, run: pytest test_payouts.py -v
   ```

   ![Image](./media/image9.png)

1. Watch Agent Mode's process carefully. Agent mode autonomously uses various tools to get to the end result. After it runs commands and applies edits, Agent mode works to detect syntax errors, terminal output, test results, and build errors. Based on the results, it then determines how to correct.

1. Review Agent's Work: Document in a notepad what Agent got right vs. what needed correction.

   ![Image](./media/image10.png)

1. Allow Copilot to run.

   ![Image](./media/image11.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex11-task4-lab11-fix-security-vuln" />

### Task 5: Agent Mode — Fix the Fee Calculation Bug

1. Run the below Agent mode prompt:

   ```
   INCIDENT FIX - CRITICAL
   The calculate_fee function in payout_models.py returns None for
   unsupported currencies (like GBP). This causes a TypeError downstream in
   process_payout().
   Fix this by:
   1. Adding a default fee calculation for unsupported currencies (4% + 1.00)
   2. OR raising a ValueError for unsupported currencies
   3. Handling this error gracefully in process_payout()
   4. Update the POST /payouts/<id>/process endpoint in payout_api.py
   to return a 400 error if the fee calculation fails
   ```

   ![Image](./media/image12.png)

1. Run tests after changes.

   ![Image](./media/image13.png)

1. Accept or Refine: Agent Mode may choose Option 1 (default fee) or Option 2 (raise ValueError). **Which is correct?**

   ![Image](./media/image14.png)

1. **If Agent chose the default fee:** Override and ask it to use ValueError instead. In fintech, **correctness > availability** for financial calculations.

   **Follow-up Prompt:**
   ```
   Actually, for a financial system, it's safer to reject unknown currencies with a ValueError rather than applying a default fee. Please change the
   approach to raise ValueError for unsupported currencies and handle it in the API layer with a 400 response.
   ```

1. Run tests:

   +++pytest test_payouts.py -v+++

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex11-task5-lab11-fix-fee-calculation" />

### Task 6: Agent Mode — Fix the Key Mismatch Bug (amt vs amount)

1. This is the bug crashing the merchant payouts endpoint. Type in Agent Mode:

   **Agent Mode Prompt:**

   ```
   INCIDENT FIX - HIGH PRIORITY
   There is a key naming inconsistency across the codebase:
   - payout_models.py stores the amount as "amt"
   - payout_api.py references "amount" in the merchant_payouts endpoint
   - This causes a KeyError crash on GET /merchants/<id>/payouts
   Fix this across ALL files consistently. The canonical key should be
   "amount" (not "amt") because it is more readable. Update:
   1. payout_models.py - change "amt" to "amount" everywhere
   2. payout_api.py - verify all references use "amount"
   3. test_payouts.py - update any test assertions referencing "amt"
   4. Run all tests after changes.
   ```

   ![Image](./media/image15.png)

   ![Image](./media/image16.png)

1. Agent Mode should:

   1. Read all 3 files to understand the scope

   1. Rename "amt" → "amount" in payout_models.py

   1. Update process_payout() where it references `p["amt"]`

   1. Update test assertions (e.g., `assert p["amt"]` → `assert p["amount"]`)

   1. Run tests

1. test_api_merchant_payouts should now pass.

   ![Image](./media/image17.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex11-task6-lab11-fix-key-mismatch" />

### Task 7: Fix the Missing Fields Test

1. The test test_api_missing_fields expects a 400 response when required fields are missing, but the original code returned 500 (unhandled KeyError). After Phase 2's validation fixes, this should now work.

   Run:

   +++pytest test_payouts.py::test_api_missing_fields -v+++

   ![Image](./media/image18.png)

   ![Image](./media/image19.png)

1. If it still fails, select the test and the relevant API code, then:

   **Prompt:**

   ```
   /fix This test expects a 400 status code when 'currency' is missing from
   the POST /payouts request body. The endpoint should validate required
   fields and return 400 with a descriptive error. Fix either the test or the endpoint as needed.
   ```

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ex11-task7-lab11-fix-missing-fields-test" />

## Review

In this exercise, you have completed the following:

   - Applied a triage-first approach to prioritize fixes by business impact
   - Used Copilot Chat to confirm the diagnosis by cross-referencing incident logs with source code
   - Used /explain to trace the downstream impact of dangerous code paths
   - Used Agent Mode to fix a critical security vulnerability (negative payout amounts)
   - Fixed a financial calculation bug (GBP currency support) with human override of Agent's approach
   - Fixed a key naming inconsistency across multiple files using Agent Mode's multi-file awareness
   - Restored failing unit tests after all fixes were applied

### You have successfully completed the exercise!
### In the Lab Guide section, click the **Next >>** button to proceed to Exercise 12.

![](media/up4.png)
