# Product Positioning Generator
The **Product Positioning Generator** automatically generates publication-ready product taglines, positioning statements, and unique value propositions or feature release notes based upon product documentation and details provided. Product Marketing and Product Management professionals can now translate how a new product or translates to customer value. 

## Core Functions
This AI automation was developed using **N8N**, **Google Gemini 3.1 Pro**, and **Gmail**. It includes the following features:
- **Dynamic Input Selection:** Users choose between a New Product Launch or a Feature Release.
- **Hybrid Data Extraction:** The agent extracts core value drivers from two sources:
  - **Form Fills:** Generates responsive answers to targeted questions about the product suite, its audience, and competitors.
  - **PDF Analysis:** The Information Extractor analyzes uploaded PRDs or RFCs to determine competitive advantages and anticipated results.
- **Strict Content Governance:** A custom-engineered LLM prompt enforces a "Banned Words" list (e.g., avoiding "seamless," "end-to-end," and "groundbreaking") to ensure the output remains authoritative, human-centric, and clear.
- **Structured Output:** The final copy is formatted into a Markdown file and emailed directly to you for review.

## ⚙️ Workflow Architecture
- **Form Triggers:** Collects initial launch context and persona data.
- **Information Extractor:** Uses multimodal **Gemini 3.1 Pro** to parse uploaded PDFs for technical specifications and problem statements.
- **Data Logic:** Prioritizes "Extracted" data over "Manual" data to ensure the highest fidelity.
- **Structure & Stylistic Guidelines:** Applies strict PMM-standard rules to draft the tagline, positioning statement, and value prop.
- **Gmail Node:** Automates the final delivery of the copy as an .md attachment.

## Workflow Outputs
- **Product Tagline:** A unique and memorable phrase that captures the essence of the product and its primary benefit. 
- **Positioning Statement:** 2-3 sentences that clearly define what the product is, what it does, who it's for, the impact it delivers, and its competitive advantages.
- **Unique Value Proposition:** 4-6 substantiating claims as bulletpoints. Explain how the product delivers value for its users and how it's superior to its alternatives.
- **Feature Release Note:** 2-3 sentence feature release note, helping users understand the benefits of new product enhancements and how to use it. 

## Getting Started
- **Configure Credentials:** Ensure your Google Gemini (PaLM) and Gmail OAuth2 credentials are active in n8n.
- **Submit the Form:** Open the Form Trigger URL.
- **Upload & Execute:** Upload your latest PRD (PDF format). 
- **Review the Outputs:** The agent will process the file, draft the copy, and send it to your inbox
