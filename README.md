# Kiescy MCP servers

Let your AI coding assistant fix the privacy, accessibility and security issues on your live website, and write your legal pages, with your approval at every step.

[Kiescy](https://kiescy.com) checks your live site in a real browser: which cookies and trackers load before the visitor agrees, what still loads after they click **Reject**, accessibility (WCAG A/AA) and security headers. These hosted MCP servers give Claude Code, Cursor, VS Code and any other MCP client what Kiescy found, plus exact instructions for fixing it in *your* code.

| Server | URL | What it does |
| --- | --- | --- |
| **Kiescy** (audit and fix) | `https://mcp.kiescy.com/mcp` | Reads your site's open issues, explains how to fix each one in your codebase, and re-checks the live site after you deploy |
| **Kiescy Documents** | `https://mcp.kiescy.com/mcp/documents` | Asks about your business, then drafts or imports your privacy policy, terms, cookie policy and 8 other documents, applies a design, and publishes only after you say yes |

Both are remote servers (Streamable HTTP). Nothing to install.

## Get an API key

1. Create a free account at [kiescy.com](https://kiescy.com) (free during early access, every feature, no card).
2. Add your website and run the checks.
3. Open the website, then **Fix with AI**, and copy its API key. Each website has its own key.

## Connect

### Claude Code

```bash
claude mcp add --transport http kiescy https://mcp.kiescy.com/mcp --header "Authorization: Bearer YOUR_API_KEY"
claude mcp add --transport http kiescy-documents https://mcp.kiescy.com/mcp/documents --header "Authorization: Bearer YOUR_API_KEY"
```

### Cursor (`.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "kiescy": {
      "url": "https://mcp.kiescy.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

### VS Code (`.vscode/mcp.json`)

```json
{
  "servers": {
    "kiescy": {
      "type": "http",
      "url": "https://mcp.kiescy.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

Any other client: point it at the URL and send the key as `Authorization: Bearer YOUR_API_KEY` (or `X-API-Key: YOUR_API_KEY`). Keep the key in user settings or a git-ignored file, never in a public repo.

## Try it

- "Fix the Kiescy issues on this site." The assistant reads the issues, searches your code, shows you a plan and waits for your OK before editing anything.
- "Which scripts still load after a visitor clicks Reject, and how do I stop them?"
- "I'm adding Google Analytics. Make it wait for consent." (`get_consent_api` explains the tagging and hooks.)
- "Set up my legal documents with Kiescy." (Documents server, or the `setup_legal_documents` prompt.)

## Tools

**Kiescy** (`/mcp`)

| Tool | What it does |
| --- | --- |
| `get_site` | The website this key belongs to, its banner script tag, health score and open issues by severity |
| `list_issues` | Issues found on the live site, most severe first |
| `get_issue` | One issue: what Kiescy saw, where, and the suggested fix |
| `get_fix_prompt` | Step-by-step fix instructions for this codebase |
| `get_consent_api` | How the site's code should wait for the visitor's consent (script tags, `pira.when`, `piraQueue`) |
| `run_check` | Re-runs a check against the live site after you deploy |
| `get_check` | The result of that check: found, opened and fixed |

Prompt: `fix_issues`. This server also includes the document tools below, for connections made before the Documents server existed.

**Kiescy Documents** (`/mcp/documents`)

`get_document_setup`, `update_business_profile`, `list_document_templates`, `list_documents`, `get_document`, `create_document`, `update_document`, `import_document`, `apply_import`, `analyze_document`, `check_documents`, `list_versions`, `compare_versions`, `preview_document`, `publish_document`, `rollback_document`, `mark_document_reviewed`, `save_translation`, `get_embed_code`, `update_document_style`

Prompts: `setup_legal_documents`, `review_legal_documents`, `design_my_documents`.

## Safety by design

- **One website per key.** The assistant sees only that website's issues and documents. It can't change your banner or account settings, or read consent records.
- **Plan first.** The server tells the assistant to show you a plan and wait for your approval before it edits any file.
- **Publishing needs your yes.** `publish_document` returns a summary and a one-time token. The assistant must show you the summary and get your explicit yes in the conversation before it publishes.
- **Asks, never guesses.** Answers that change people's rights (where customers are, what data is collected, refunds, children) need your confirmation.
- Revoke a key at any time in Kiescy (Settings → API key); the old key stops working at once.

Kiescy's templates and checks help you comply. They are not legal advice and don't guarantee compliance.

## Links

- Setup guide: https://kiescy.com/product/fix-with-ai
- Documents MCP: https://kiescy.com/product/ai-documents
- Free privacy check, no sign-up: https://kiescy.com/check
- Support: hello@kiescy.com

`server.json` and `server.documents.json` are the entries for the [official MCP Registry](https://registry.modelcontextprotocol.io) (`com.kiescy/kiescy`, `com.kiescy/documents`).

## License

The documentation and config examples in this repository are MIT licensed. The Kiescy service itself is a hosted product; see [kiescy.com/terms](https://kiescy.com/terms).
