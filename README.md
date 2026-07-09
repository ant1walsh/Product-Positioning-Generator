# Product Positioning Generator

The **Product Positioning Generator** automatically produces publication-ready product taglines, positioning statements, and unique value propositions — or feature release notes — from product details and optional supporting documents. Product Marketing and Product Management professionals can use it to translate how a new product or feature delivers customer value. Built on **n8n**, the latest **Gemini Pro** and **Gemini Flash** models, and **Gmail**.

---

## Core Features

- **Dynamic Input Selection:** users choose between a New Product Launch or a Feature Release at the start of the workflow.
- **Hybrid Data Extraction:** the agent extracts core value drivers from two sources:
  - **Form Fills:** targeted answers about the product or feature, its audience, competitors, and differentiators.
  - **PDF Analysis:** the Information Extractor uses the latest Gemini Pro model to parse uploaded PRDs or RFCs and pull structured attributes — problem statement, functionality, anticipated results, ICP, persona, competitors, and differentiators.
- **Strict Content Governance:** a custom LLM prompt enforces a "banned words" list (e.g. avoiding "seamless," "end-to-end," "best-in-class," "game-changing," "empower," "robust," "turnkey") to keep output authoritative, human-centric, and clear.
- **Structured Output:** the final copy is formatted into a Markdown file and delivered to your inbox for review.

---

## Workflow Architecture

### New Product Launch

1. **Product Enhancement Evaluation** *(Form Trigger)* — user selects "Launching a new product."
2. **Product Launch Details** *(Form)* — collects product name, category, problem statement, how it works, primary features, anticipated results, ICP, target persona, competitors, and differentiators. Optionally accepts a PDF (PRD or RFC).
3. **Uploaded PRD/RFC** *(IF)* — checks whether a PDF was uploaded and routes accordingly.
4. **Extract from File → Information Extractor** — if a PDF was uploaded, extracts text and pulls structured attributes (latest Gemini Pro).
5. **Only Form Field** *(IF)* — if no PDF was uploaded, routes the form data onward.
6. **Basic LLM Chain** — drafts the tagline, positioning statement, and unique value proposition (latest Gemini Flash). The PDF and form-only branches converge directly here, prioritizing extracted PDF data over form data where both exist.
7. **Structured Output** — parses the LLM output into `tagline`, `positioning_statement`, and `unique_value_proposition`.
8. **Format Text → Convert to File** — formats the output as Markdown and saves it as `product_positioning_statements.md`.
9. **Review Copy** *(Gmail)* — sends the file as an email attachment for review.

### Feature Release

1. **Product Enhancement Evaluation** *(Form Trigger)* — user selects "Launching a new feature."
2. **Feature Release Details** *(Form)* — collects feature name, parent product, user persona, problem statement, how the feature works, benefits, comparable alternatives, and differentiators. Optionally accepts a PDF (PRD or RFC).
3. **Uploaded PRD/RFC1** *(IF)* — checks whether a PDF was uploaded and routes accordingly.
4. **Extract from File1 → Information Extractor1** — if a PDF was uploaded, extracts text and pulls structured feature attributes (latest Gemini Pro).
5. **Only Form Field1** *(IF)* — if no PDF was uploaded, routes the form data onward.
6. **Basic LLM Chain1** — drafts a 2–3 sentence feature release note (latest Gemini Flash), encouraging adoption among current customers and engaged prospects. The PDF and form-only branches converge directly here.
7. **Convert to File1** — saves the output as `feature_release_note.md`.
8. **Review Copy1** *(Gmail)* — sends the file as an email attachment for review.

---

## Technical Specifications

| | |
| --- | --- |
| **Platform** | n8n (Cloud or self-hosted) |
| **Models** | `gemini-pro-latest` (extraction), `gemini-flash-latest` (drafting) |
| **Input formats** | Form fields; optional `.pdf` (PRD/RFC) |
| **Output format** | Markdown (`.md`) |
| **Integrations** | Google Gemini (PaLM) API, Gmail OAuth2 |

---

## Requirements

- **n8n** (Cloud or self-hosted) with the LangChain nodes package (`@n8n/n8n-nodes-langchain`)
- **Google Gemini API key** — for information extraction and drafting
- **Gmail OAuth2 credentials** — for automated email delivery

---

## Inputs

- **Entry:** "Are you launching a new product or feature?" (required).
- **Product:** name, category, problem solved, how it works, primary features, results, ICP, target persona, competitors, differentiators, optional PDF.
- **Feature:** feature name, parent product, user persona, problem solved, how it works, benefits, comparable competitor features, differentiation, optional PDF.

The workflow runs from form fields alone or enriches them with a PRD/RFC; when both exist, extracted attributes take priority.

---

## Outputs

- **Product Tagline** — a unique, memorable phrase capturing the product's essence and primary benefit.
- **Positioning Statement** — 2–3 sentences defining what the product is, what it does, who it's for, the impact it delivers, and its competitive advantages.
- **Unique Value Proposition** — 4–6 substantiating bullet points on how the product delivers value and bests its alternatives.
- **Feature Release Note** — a 2–3 sentence note helping users understand a new enhancement's benefits and how to use it.

Delivered as `product_positioning_statements.md` (product) or `feature_release_note.md` (feature).

---

## Setting Up Credentials

After import, n8n will show the Gemini and Gmail nodes as needing a credential. Set each up once, then select it on the relevant nodes.

### Google Gemini

Nodes that use it: **Google Gemini Chat Model**, **Google Gemini Chat Model1**, **Google Gemini Chat Model2**, **Google Gemini Chat Model3**. (These chat-model nodes power both the drafting chains and the Information Extractors.)

1. Get an API key from [Google AI Studio](https://aistudio.google.com/app/apikey): sign in and click **Get API key → Create API key**. Copy it.
2. In n8n: **Credentials → Add credential → search "Google Gemini(PaLM) API"**.
3. Paste the key into the **API Key** field and **Save** (e.g. name it "Google Gemini API account").
4. Open each of the four Gemini chat-model nodes above and select the credential in the **Credential to connect with** dropdown.

### Gmail

Nodes that use it: **Review copy**, **Review copy1**.

1. In n8n: **Credentials → Add credential → search "Gmail OAuth2 API"**.
2. Connect your account:
   - **n8n Cloud:** click **Sign in with Google** and authorize the Gmail account to send from.
   - **Self-hosted:** create a Google Cloud OAuth client — in [Google Cloud Console](https://console.cloud.google.com/), pick/create a project → enable the **Gmail API** → **APIs & Services → Credentials → Create OAuth client ID (Web application)** → add n8n's **OAuth Redirect URL** (shown on the credential screen) to the authorized redirect URIs → paste the **Client ID** and **Client Secret** into n8n → **Sign in with Google** and authorize.
3. **Save**, then select this credential on **Review copy** and **Review copy1**.

---

## Set the Recipient Email

Both Gmail nodes ship with `sendTo = your-email@example.com`. Open **Review copy** and **Review copy1** and change the **To** field to your address.

---

## Import and Run

1. In n8n: **Workflows → Import from File →** choose `Product Positioning Generator.json`.
2. Configure the Gemini and Gmail credentials (above) and set the recipient email.
3. The workflow imports **inactive**. Open the **Product Enhancement Evaluation** form trigger for the form URL, test both a product and a feature run, then toggle the workflow **Active**.

---

## Notes

- PDF uploads must be `.pdf`. The workflow works from form fields alone or enriches them with a PRD/RFC; when both exist, extracted attributes take priority.
- Models: the latest **Gemini Pro** (`gemini-pro-latest`) for extraction and the latest **Gemini Flash** (`gemini-flash-latest`) for drafting.
- Keep your Gemini API key private; don't commit a filled-in copy of this workflow to a public repo.
