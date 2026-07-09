# Product Positioning Generator (template)

An n8n workflow that generates positioning and messaging for enterprise software. Two paths: a **new product** path that outputs a tagline, positioning statement, and unique value proposition, and a **new feature** path that outputs a concise feature release note. Both accept structured form input, optionally read a PRD/RFC PDF, and email the result as a Markdown file. Drafting and extraction run on Google Gemini.

This is a **templatized** version: all instance-specific values (recipient email, credential IDs, workflow/instance IDs) have been replaced with placeholders so it's safe to share and import into any n8n instance.

## What it does

1. You choose whether you're launching a new product or a new feature.
2. You fill in a detailed form and optionally upload a PRD/RFC PDF.
3. If a PDF is provided, its text is extracted and an Information Extractor pulls structured attributes from it.
4. Form data and any extracted attributes feed the drafting chain (Gemini), which produces the copy under strict tone rules and a banned-buzzword list.
5. The result is converted to `.md` and emailed for review.

## Workflow structure

The entry form routes to one of two branches via an IF node.

**Product path:** `Product Launch Details → Uploaded PRD/RFC (IF) → [PDF] Extract from File → Information Extractor  ·  [no PDF] Only Form Field → Basic LLM Chain → Format text → Convert to File → Review copy`

**Feature path:** `Feature Release Details → Uploaded PRD/RFC1 (IF) → [PDF] Extract from File1 → Information Extractor1  ·  [no PDF] Only Form Field1 → Basic LLM Chain1 → Convert to File1 → Review copy1`

The PDF and form-only branches converge directly into the drafting chains, so neither path can stall waiting on the other.

## Form inputs

- **Entry:** "Are you launching a new product or feature?" (required).
- **Product:** name, category, problem solved, how it works, primary features, results, ICP, target persona, competitors, differentiators, optional PDF.
- **Feature:** feature name, parent product, user persona, problem solved, how it works, benefits, comparable competitor features, differentiation, optional PDF.

## Output

- **Product** — `product_positioning_statements.md`: a Product Tagline, a 2–3 sentence Positioning Statement, and a 4–6 bullet Unique Value Proposition.
- **Feature** — `feature_release_note.md`: a 2–3 sentence release note framed around benefits, usage, and differentiation.

The prompts enforce a strict style (no "it," personas instead of "users," no leading prepositional fragments) and ban a long list of buzzwords to keep copy concrete.

## Requirements

- An **n8n** instance (Cloud or self-hosted) with the LangChain nodes package (`@n8n/n8n-nodes-langchain`).
- A **Google Gemini API key** (information extraction and drafting).
- A **Gmail account** (to send the review emails).

## Setting up credentials

This template ships with placeholder credentials, so after import n8n will show the Gemini and Gmail nodes as needing a credential. Set them up once, then select them on each node.

### Google Gemini

Nodes that use it: **Google Gemini Chat Model**, **Google Gemini Chat Model1**, **Google Gemini Chat Model2**, **Google Gemini Chat Model3**. (These chat-model nodes power both the drafting chains and the Information Extractors.)

1. Get an API key: go to [Google AI Studio](https://aistudio.google.com/app/apikey), sign in, and click **Get API key → Create API key**. Copy it.
2. In n8n: **Credentials → Add credential → search "Google Gemini(PaLM) API"**.
3. Paste the key into the **API Key** field and **Save** (name it e.g. "Google Gemini API account").
4. Open each of the four Gemini chat-model nodes above and select the credential in the **Credential to connect with** dropdown.

### Gmail

Nodes that use it: **Review copy**, **Review copy1**.

1. In n8n: **Credentials → Add credential → search "Gmail OAuth2 API"**.
2. Connect your account:
   - **n8n Cloud:** click **Sign in with Google** and authorize the Gmail account to send from.
   - **Self-hosted:** create a Google Cloud OAuth client — in [Google Cloud Console](https://console.cloud.google.com/), pick/create a project → enable the **Gmail API** → **APIs & Services → Credentials → Create OAuth client ID (Web application)** → add n8n's **OAuth Redirect URL** (shown on the credential screen) to the authorized redirect URIs → paste the **Client ID** and **Client Secret** into n8n → **Sign in with Google** and authorize.
3. **Save**, then select this credential on **Review copy** and **Review copy1**.

## Set the recipient email

Both Gmail nodes ship with `sendTo = your-email@example.com`. Open **Review copy** and **Review copy1** and change the **To** field to your address.

## Import and activate

1. In n8n: **Workflows → Import from File →** choose `Product Positioning Generator (template).json`.
2. Set the Gemini and Gmail credentials and the recipient email.
3. The workflow imports **inactive**. Open the **Product Enhancement Evaluation** form trigger for the form URL, test both a product and a feature run, then toggle **Active**.

## Notes

- PDF uploads must be `.pdf`. The workflow works from form fields alone or enriches them with a PRD/RFC; when both exist, extracted attributes take priority.
- Models: `gemini-pro-latest` for extraction, `gemini-flash-latest` for drafting.
- Keep the Gemini API key private; don't commit a filled-in copy to a public repo.
