# WebMCP Skill

[![skills.sh](https://skills.sh/b/Blackie360/webmcp-skill)](https://skills.sh/Blackie360/webmcp-skill)

Agent skill for implementing **WebMCP** (Web Model Context Protocol) on websites — expose your app's features as structured, agent-callable tools via `document.modelContext.registerTool()`.

> **Status**: WebMCP is a [draft W3C spec](https://webmachinelearning.github.io/webmcp/). The API may change. Chrome 149+ origin trial and local dev flag available as of mid-2026.

Make your website agent-ready **without DOM scraping**. WebMCP lets browser agents call your existing JavaScript functions with JSON Schema inputs instead of guessing which button to click.

## Install

```bash
npx skills add Blackie360/webmcp-skill -g
```

Install to the current project (omit `-g`):

```bash
npx skills add Blackie360/webmcp-skill
```

After publishing, find the skill at [skills.sh](https://skills.sh).

## What This Skill Covers

- Imperative `document.modelContext.registerTool()` implementation
- JSON Schema tool inputs and structured return values
- Security annotations (`readOnlyHint`, `untrustedContentHint`)
- Origin isolation and permissions policy requirements
- Chrome local flag and origin trial setup
- React/vanilla integration patterns
- Testing with the Model Context Tool Inspector extension

## What This Skill Does NOT Cover

- **Server-side MCP** — use an `mcp-builder` skill from [skills.sh](https://skills.sh) instead
- **Browser DOM automation** — use browser MCP / automation skills for scraping-based flows

## Quick Start (Human)

1. Enable Chrome flag: `chrome://flags/#enable-webmcp-testing`
2. Register a tool in your app:

```typescript
const controller = new AbortController()

await document.modelContext.registerTool(
  {
    name: 'search_products',
    title: 'Search products',
    description: 'Searches the catalog by keyword. Read-only.',
    inputSchema: {
      type: 'object',
      properties: { query: { type: 'string' } },
      required: ['query']
    },
    annotations: { readOnlyHint: true },
    execute: async ({ query }) => {
      const results = await searchProducts(query)
      return { count: results.length, results }
    }
  },
  { signal: controller.signal }
)
```

3. Test with the [Model Context Tool Inspector](https://developer.chrome.com/docs/ai/webmcp)

## Repository Structure

```
webmcp-skill/
├── SKILL.md                    # Agent instructions
├── references/
│   ├── api-reference.md
│   ├── security.md
│   └── browser-setup.md
└── examples/
    ├── imperative-tool.ts
    ├── react-spa-tool.tsx
    └── form-tool.html
```

## Publishing Your Fork

1. Push this repo to a **public** GitHub repository
2. Add topics: `agent-skills`, `webmcp`, `cursor`, `claude-code`
3. Verify discovery: `npx skills add . --list`
4. Share: `npx skills add Blackie360/webmcp-skill`
5. Listing appears on [skills.sh](https://skills.sh) after indexing

## References

- [W3C WebMCP Specification](https://webmachinelearning.github.io/webmcp/)
- [Chrome WebMCP Documentation](https://developer.chrome.com/docs/ai/webmcp)
- [WebMCP Explainer (GitHub)](https://github.com/webmachinelearning/webmcp)
- [Agent Skills Specification](https://agentskills.io)
- [Skills CLI](https://github.com/vercel-labs/skills)

## License

MIT — see [LICENSE](LICENSE).
