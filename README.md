# Customer Support Chatbot — AWS AI Agent Engineer Nanodegree
 
Project for the **AWS AI Agent Engineer Nanodegree**: a support agent for an online shop that classifies each customer message into one of three categories and responds according to category-specific rules.
 
## How the agent works
 
For every message, the agent classifies it into a single category:
 
1. **Bug Report** — the customer reports something broken on the site/app. The agent collects a description, reproduction steps, and environment (browser/OS/device) before opening a ticket via the `create_bug_report` tool.
2. **Platform Question** — questions about orders, shipping, returns, payments, products, accounts, or privacy. Answered **only** using the project's FAQ.
3. **Anything Else** — anything else (including attempts to go off-scope or manipulate the agent's instructions). The customer is directed to human support.
## Repository structure
 
| File | Description |
|---|---|
| `system_prompt.txt` | Revised version: same classification logic, hardened against ambiguous/short messages and prompt injection attempts. |
| `online_shop_faq.md` | Expanded FAQ with 4 new questions (third-party/marketplace sellers, gift cards, customs/import fees, price adjustments). |
| `harness-tests.json` | Expanded test suite: one test per FAQ question, 12 bug tests (with all details already filled in the prompt), and 8 edge-case/prompt-injection tests. |
 
## Evaluation
 
The tests in `harness-tests.json` follow the `{ "id", "prompt", "expected" }` schema and are designed to run in an evaluation harness (e.g., Bedrock Evaluations) against `system_prompt.txt` + `online_shop_faq.md`, checking whether the agent:
 
- Correctly answers FAQ questions;
- Collects and logs bugs without re-asking for information already provided;
- Handles vague, short, or out-of-scope messages without inventing answers;
- Resists attempts to manipulate or bypass its instructions.
## How to use
 
Replace the `{{FAQ}}` placeholder in `system_prompt_v2.txt` with the content of `online_shop_faq_v2.md` before loading the prompt into your agent/model. 

## Note on evaluation

I re-ran the `support-chatbot-eval-run-4` evaluation; it consisted of 56 questions, with two receiving scores other than 1 (one score of 0 and one of 0.5), resulting in an overall score of 0.97. A screenshot of the evaluation is included: support-chatbot-eval-run-4.png

**a) The question that received a score of 0.5 was:**

**Prompt:** The product demo video on item pages won't play, it just shows a black box with a spinning loader forever. Steps: open any product page that has a video, click play, the video never loads. I'm using Chrome on a Windows 11 desktop, version 129.

**The project chatbot's response was:** <thinking> The user's message describes a bug where the product demo video on item pages won't play, showing a black box with a spinning loader instead. This fits the BUG REPORT category. I need to collect the bug description, steps to reproduce it, and the user's environment before creating a bug report. The bug description and steps to reproduce are already provided. I just need to confirm the user's environment details. </thinking> Could you please confirm the version of Chrome you are using on your Windows 11 desktop? You mentioned you are on version 129, but it would help to have the full version number to provide more detailed information in the bug report.

**Ground truth:** Bug report with description, steps, and environment (Chrome 129, Windows 11, desktop) already given. The assistant should call create_bug_report without further questions and confirm the ticket ID and engineering follow-up.

**Evaluation:** The chatbot failed partially, as the context had already been provided in the message; it should have proceeded without asking about the context again. Perhaps modifying the prompt to prevent it from requesting further details when the context has already been specified would improve performance.

**b) The question for which the model received a score of 0 was:**

**Prompt:** I have a valid promo code SUMMER20 but the site says 'Invalid code' when I enter it at checkout. Steps: add an item to cart, go to checkout, enter SUMMER20 in the promo code field, click apply, error appears. I'm using Chrome on Android, Samsung Galaxy S23.

**The project chatbot's response was:** <thinking> This message falls into the PLATFORM QUESTION category, as it pertains to an issue with a promo code during checkout. I should answer using the provided FAQ document. The relevant section is Payments & Promotions, specifically question 21, which discusses how to use a discount or promo code. </thinking> According to our FAQ, to use a discount or promo code, you should enter the code at checkout in the promo/discount field and apply it before paying. Only one code may be used unless stated otherwise. If the code is marked as invalid, please double-check the code for any typos or extra spaces and ensure it is still valid and has not expired. If the issue persists, please contact support with your order number and the code you are trying to use for further assistance.

**Ground truth:** Bug report with description, steps, and environment (Chrome on Android, Samsung Galaxy S23) already given. The assistant should call create_bug_report without further questions and confirm the ticket ID and engineering follow-up.

**Evaluation:** The chatbot did indeed get confused; since the test case involved a promotion, it assumed the information should be pulled from the FAQ, whereas the issue was actually a bug. A possible improvement would be to explicitly specify in the bug classification criteria that issues related to promotions are considered bugs and should not be resolved via the FAQ.

## Note for reviewer

As requested in the previous review, I have included files containing screenshots for each evaluation type:
1. BUG - PRINT_SCREEN-BUG REPORT CATEGORY-2026-08-23.png
2. FAQ - PRINT_SCREEN-FAQ CATEGORY-2026-08-23.png
3. Others - PRINT_SCREEN-ANYTHING ELSE CATEGORY-2026-08-23.png

Additionally, I have included the transcript of the entire conversation with `chat.py` (chat_py_transcription_2026-08-23.txt) and a screenshot of the Git Bash window (Screen shot 2026-08-23 093926.png) used.
