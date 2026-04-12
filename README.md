# Product Positioning Generator
The **Product Positioning Generator** automatically generates publication-ready product taglines, positioning statements, and unique value propositions or feature release notes based on product documentation and details provided. Product Marketing and Product Management professionals can use this tool to translate how a new product or feature delivers customer value.

## Core Functions
This AI automation was developed using **n8n**, **Google Gemini 3.1 Pro**, and **Gmail**. It includes the following features:
- **Dynamic Input Selection:** Users choose between a New Product Launch or a Feature Release at the start of the workflow.
- **Hybrid Data Extraction:** The agent extracts core value drivers from two sources:
  - **Form Fills:** Collects targeted answers about the product or feature, its audience, competitors, and differentiators.
  - **PDF Analysis:** The Information Extractor uses Gemini 3.1 Pro to parse uploaded PRDs or RFCs and extract structured product attributes — including the problem statement, feature functionality, anticipated results, ICP, persona, competitors, and differentiators.
- **Strict Content Governance:** A custom-engineered LLM prompt enforces a "Banned Words" list (e.g., avoiding "seamless," "end-to-end," and "groundbreaking") to ensure the output remains authoritative, human-centric, and clear.
- **Structured Output:** The final copy is formatted into a Markdown file and delivered to your inbox for review.

## ⚙️ Workflow Architecture

### New Product Launch
1. **Product Enhancement Evaluation** *(Form Trigger)* — User selects "Launching a new product."
2. **Product Launch Details** *(Form)* — Collects the product name, category, problem statement, how it works, primary features, anticipated results, ICP, target persona, competitors, and competitive differentiators. Optionally accepts a PDF upload (PRD or RFC).
3. **Uploaded PRD/RFC** *(If Node)* — Checks whether a PDF was uploaded and routes accordingly.
4. **Extract from File → Information Extractor** — If a PDF was uploaded, extracts text and uses Gemini 3.1 Pro to pull structured attributes from the document.
5. **Only Form Field** *(If Node)* — If no PDF was uploaded, routes the form data directly to the merge step.
6. **Merge** — Combines extracted PDF data and form data into a single input.
7. **Basic LLM Chain** — Drafts the product tagline, positioning statement, and unique value proposition using Gemini 3.1 Pro, prioritizing extracted PDF data over form data where both are available.
8. **Structured Output** — Parses the LLM output into three structured fields: `tagline`, `positioning_statement`, and `unique_value_proposition`.
9. **Format Text → Convert to File** — Formats the output as Markdown and saves it as `product_positioning_statements.md`.
10. **Review Copy** *(Gmail)* — Sends the file as an email attachment for review.

### Feature Release
1. **Product Enhancement Evaluation** *(Form Trigger)* — User selects "Launching a new feature."
2. **Feature Release Details** *(Form)* — Collects the feature name, parent product, user persona, problem statement, how the feature works, its benefits, comparable alternatives, and competitive differentiators. Optionally accepts a PDF upload (PRD or RFC).
3. **Uploaded PRD/RFC1** *(If Node)* — Checks whether a PDF was uploaded and routes accordingly.
4. **Extract from File1 → Information Extractor1** — If a PDF was uploaded, extracts text and uses Gemini 3.1 Pro to pull structured feature attributes.
5. **Only Form Field1** *(If Node)* — If no PDF was uploaded, routes the form data directly to the merge step.
6. **Merge1** — Combines extracted PDF data and form data into a single input.
7. **Basic LLM Chain1** — Drafts a 2–3 sentence feature release note using Gemini 3.1 Pro, encouraging adoption among current customers and engaged prospects.
8. **Convert to File1** — Saves the output as `feature_release_note.md`.
9. **Review Copy1** *(Gmail)* — Sends the file as an email attachment for review.

## Workflow Outputs
- **Product Tagline:** A unique and memorable phrase that captures the essence of the product and its primary benefit.
- **Positioning Statement:** 2–3 sentences that clearly define what the product is, what it does, who it's for, the impact it delivers, and its competitive advantages.
- **Unique Value Proposition:** 4–6 substantiating claims as bullet points explaining how the product delivers value and how it's superior to its alternatives.
- **Feature Release Note:** A 2–3 sentence release note helping users understand the benefits of a new product enhancement and how to use it.

## Getting Started
1. **Configure Credentials:** Ensure your Google Gemini (PaLM) and Gmail OAuth2 credentials are active in n8n.
2. **Submit the Form:** Open the Form Trigger URL and select whether you are launching a new product or a new feature.
3. **Fill in Details:** Complete the relevant form fields for your product or feature.
4. **Upload a Document *(optional)*:** Upload a PRD or RFC in PDF format to supplement or replace manual form inputs.
5. **Review the Output:** The agent will process your inputs, draft the copy, and send it to your inbox as a Markdown file.
