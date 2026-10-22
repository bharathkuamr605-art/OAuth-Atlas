# OAuth Atlas

An evidence-first OAuth setup guide prototype built for the Composio APM Integrations take-home. It includes a searchable library of four curated provider guides and a live research endpoint for unfamiliar apps.

## Run locally

Requirements: Node.js 18+.

```bash
npm start
```

Open http://localhost:3000. The curated guide library works without an API key.

### Enable live guide generation

Set an OpenAI API key in your environment before starting the server. The server uses the Responses API with web search, and the key is never sent to the browser.

macOS/Linux:

```bash
export OPENAI_API_KEY="your-key"
npm start
```

Windows PowerShell:

```powershell
$env:OPENAI_API_KEY="your-key"
npm start
```

Optional: set `OPENAI_MODEL` to a model available to your account. Do not commit secrets or place API keys in frontend files.

## Product decisions

- **Show effort and waiting separately.** Time to first test is not the same as provider review time. Approval estimates are qualified and marked unknown when provider timelines are not public.
- **Evidence is a first-class part of the guide.** Sources, confidence, uncertainty, blockers, and human handoff points are visible in the guide.
- **Start with four varied apps.** GitHub represents a relatively direct registration flow; Google illustrates scope-dependent verification; Slack distinguishes testing from Marketplace distribution; Notion surfaces content access and workspace sharing nuances.
- **Check managed OAuth first.** Composio-managed OAuth can remove the need to register a custom app for supported toolkit/auth combinations. Developers may still need custom credentials for branding, scopes, or other requirements.
- **Agents prepare; developers retain control.** The prototype explicitly separates research and drafting from account access, credential handling, legal attestations, provider submissions, and real authorization tests.
- **Shareable guide routes.** Curated guides can be opened by hash and copied as links. Live-generated guides receive a short share ID and can be opened by other visitors while the server process is running. Print/export uses the browser print dialog.

## Live research behavior

The server asks the model to use web search and return a structured guide. It prioritizes official documentation and asks the model to distinguish estimates from verified facts. The app displays source links and uncertainties returned by the research agent. This prototype does **not** claim that OAuth registration, authorization, or provider approval has been executed. The agent's citations should be opened and checked before production use.

## Validation performed

- Seed-guide claims were grounded in official provider documentation links at authoring time.
- GitHub OAuth App creation steps were checked against GitHub's official OAuth App creation documentation.
- Google's verification notes were checked against the Google OAuth App Verification Help Center, including the important distinction that verification depends on requested scopes.
- Composio managed-vs-custom auth framing was checked against Composio's managed OAuth documentation.
- No live provider account setup, token exchange, or approval submission was performed. Time estimates are planning estimates, not measured benchmarks. Provider UIs and policies can change.

## Limitations / next steps

- Four seed guides, not a complete 100-app directory.
- Live generation requires an OpenAI API key and a model with Responses API web-search support.
- The prototype stores generated guides in server memory so share links work while the same server process remains running. A restart or redeploy clears them. For durable cross-device sharing, persist generated guide JSON behind stable IDs in a database.
- Source links are displayed but are not automatically independently verified. Add URL fetching, canonical-domain validation, citation extraction, and a reviewer feedback loop before production.
- No provider credentials are requested or stored.
- Add automated tests, rate limits, request timeouts, and output schema validation before public deployment.

## Deployment

Deploy as a Node.js web service on a host that supports Node 18+ and environment variables. Set `OPENAI_API_KEY` in the host's secret manager. Never commit `.env` or API keys. For a public demo, add rate limiting and abuse protection before exposing live generation.
