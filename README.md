# ChatBox – Public document Q&A chatbot

Public visitors can ask questions without creating an account. Only the administrator can upload or delete shared knowledge-base documents.

## Features
- Public question-and-answer interface; no visitor login required.
- One central document library shared by all visitors.
- Admin-only document upload, paste, listing, and deletion.
- OpenAI Responses API called only from the server; API key is never sent to the browser.
- Retrieval selects relevant overlapping text passages and returns source document names with answers.
- Chat history is saved in the visitor's own browser using localStorage; it is not a server-side visitor account or cross-device history.
- Suggested question prompts and a clear-history control.
- Vietnamese and English response language follows the visitor's question.
- Basic rate limiting for login and public chat.

## Run locally
Requires Node.js 20 or later.

1. Run `npm install`.
2. Copy `.env.example` to `.env`.
3. Set `OPENAI_API_KEY` and a strong `ADMIN_PASSWORD`.
4. Run `npm start`.
5. Open `http://localhost:3000`.

## Deploy on Render
The repository includes `render.yaml` for a Node web service.
- Build: `npm install`
- Start: `npm start`
- Persistent disk mount: `/var/data`
- Set secret environment variables `OPENAI_API_KEY` and `ADMIN_PASSWORD` in Render.
- Keep `DATA_DIR=/var/data` so the shared knowledge base survives redeploys.

Render deploys new commits from the connected branch automatically when auto-deploy is enabled. Review the deploy event/logs after each sync.

## Data and limitations
- Documents are stored as extracted text in `DATA_DIR/documents.json`; original PDFs are not retained.
- Text-layer PDFs, TXT, MD, and CSV are supported. Scanned PDFs require OCR before upload.
- Chat history is local to each visitor's browser and can be cleared with the chat's clear-history button.
- The current Admin uses one shared password. Admin sessions are held in memory and expire after 8 hours; a server restart requires Admin to log in again.
- The public chat endpoint is rate-limited per IP. For high traffic, add a durable shared rate-limit store and consider a database-backed document library.
- Do not commit `.env` or expose secrets in frontend code.


## Admin analytics and document management
- Admin dashboard shows total successful questions, document count, stored text size, a 14-day daily activity summary, popular questions, frequently referenced documents, and recent questions.
- Analytics are stored in `DATA_DIR/analytics.json` on the Render persistent disk.
- The analytics log stores each successful question trimmed to 160 characters, timestamp, and up to five source document names. It does not store visitor names, email addresses, or IP addresses in the analytics file.
- Admin can assign a category and up to 10 tags when uploading a document, search by document name/category/tags, filter by category, edit document name/category/tags, and delete documents.
- Existing documents remain compatible; documents without metadata are shown as `Chưa phân loại`.
- Analytics are aggregate operational metrics, not unique visitor counts. Daily counts are retained for 45 days; the recent-question list keeps 20 entries.
