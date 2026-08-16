# AI-Assisted-Mini-Lean-Canvas-Miro
transforms one product idea into a structured 5-part AI canvas, then turns that into review and paste-ready outputs for workshop use in Miro.

Cells 1-2 are instructions

Cell 4 initializes runtime
Imports Python libs and LangChain dependencies.
Creates artifacts folder if missing.
Loads environment variables from .env.
Sets USE_LLM flag to live/offline mode.

Cell 6 configures model client
Reads LLM_PROVIDER.
Branches:
foundry: validates FOUNDRY_ENDPOINT, FOUNDRY_API_KEY, FOUNDRY_MODEL; normalizes endpoint to OpenAI-compatible base URL; builds ChatOpenAI client.
azure: validates AZURE_* vars; builds AzureChatOpenAI client.
openai: validates OPENAI_* vars; builds ChatOpenAI client.
If USE_LLM is false, sets llm=None.

Cell 13 converts dict to review table
Maps internal keys to display names.
Builds rows list:
section title
ai_draft bullet text
default review_label=assumption
class_notes blank
Creates canvas_df DataFrame for class review.

Cell 8 defines product idea
Stores one business idea text string (clinic no-show reduction).

Cell 10 builds generation prompt
Creates strict JSON prompt with exact schema:
user_customer
problem
ai_solution_mvp
success_metric
biggest_assumption_next_test

Cell 11 generates and parses canvas
Defines sample_canvas fallback (offline mode).
Defines extract_json_object(text):
try json.loads on full response
if that fails, regex extracts first {...} block
parses extracted JSON
If USE_LLM:
sends SystemMessage + HumanMessage to llm.invoke(...)
stores raw model text
parses to dict canvas
Else:
uses sample_canvas
Result: canvas is always a Python dict with 5 sections.
Cell 13 converts dict to review table
Maps internal keys to display names.
Builds rows list:
section title
ai_draft bullet text
default review_label=assumption
class_notes blank
Creates canvas_df DataFrame for class review.

Cell 15 adds initial instructor labels
Updates specific rows in canvas_df:
User/Customer -> assumption
Success Metric -> evidence_needed
Biggest Assumption/Next Test -> next_test

Cell 17 creates Miro paste text
Iterates DataFrame rows and renders section blocks.
Joins blocks with separators into miro_text.
Prints final text for copy/paste into Miro stickies.

Cell 18 exports artifacts
Writes:
artifacts/mini_lean_canvas_for_miro.csv
artifacts/mini_lean_canvas_for_miro.md
artifacts/miro_paste_text.txt
This is the code section you’re currently on.
