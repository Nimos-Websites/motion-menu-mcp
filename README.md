# Motion Menu Director v1 — MCP server

Motion Menu is an agent-facing design director backed by 597 transplantable motion and UI
patterns. It helps a coding agent establish a visual direction, plan the complete page, validate
the component set, and retrieve the selected HTML/CSS/JS or framework wrappers.

The entire 597-pattern library is free. No entitlement key, card, tier, seat, or subscription is
required.

## MCP endpoint

```text
https://www.motionmenu.ca/api/mcp
```

The endpoint uses JSON-RPC 2.0 over streamable HTTP. Start with `initialize`, then treat
`tools/list` as the runtime authority for available tools and their schemas.

## Connect

Codex:

```sh
codex mcp add motion-menu --url https://www.motionmenu.ca/api/mcp
```

Claude Code:

```sh
claude mcp add --transport http motion-menu https://www.motionmenu.ca/api/mcp
```

Cursor / Windsurf (`mcp.json`):

```json
{
  "mcpServers": {
    "motion-menu": {
      "url": "https://www.motionmenu.ca/api/mcp"
    }
  }
}
```

Cline: see [llms-install.md](./llms-install.md).

Claude Code plugin (optional — the same endpoint plus the Motion Menu workflow skill):

```text
/plugin marketplace add dev55acc-ai/motion-menu-plugin
/plugin install motion-menu-patterns@motion-menu
```

## Director workflow

Use this path when the user asks Motion Menu to design, elevate, redesign, or add motion without
naming an exact pattern:

1. `get_design_digest {}` — read the machine-readable quality and implementation contract.
2. `propose_directions {brief, seed}` — generate three materially different design genomes from
   the business, audience, offer, primary action, real assets, and constraints.
3. Choose one returned `direction_id`.
4. `build_page {intent, direction_id, seed, history, exclusions}` — plan the whole page. Copy the
   exact direction `brief` into `intent` and reuse the same seed; pass recently used patterns in
   `history` and hard vetoes in `exclusions`.
5. `validate_combination {components}` — check the complete selected set and resolve hard
   violations before implementation.
6. `get_pattern {slug}` — retrieve only the approved pattern bodies. Add `framework` with
   `react`, `vue`, `svelte`, or `vanilla` when a wrapper is useful.

Example arguments:

```json
{
  "brief": "Independent architecture practice serving cultural institutions. Primary action: book a portfolio review. Real project photography is available.",
  "seed": "practice-launch-02"
}
```

After selecting a returned direction, use the exact brief as `intent`:

```json
{
  "intent": "Independent architecture practice serving cultural institutions. Primary action: book a portfolio review. Real project photography is available.",
  "direction_id": "<returned direction_id>",
  "seed": "practice-launch-02",
  "history": ["<recently used slug or number>"],
  "exclusions": ["<slug or number that must not recur>"]
}
```

Director output is a page plan, not a rendered website. The host coding agent still adapts the
patterns to the real design system, implements them in the target repository, and verifies the
result in a browser at desktop and mobile widths.

## Manual pattern mode

An explicit pattern choice bypasses autonomous selection. If a user names a number or slug, call
`get_pattern` directly and honor it:

```json
{ "number": "150", "framework": "react" }
```

Numbers are permanent aliases; slugs are preferred in code and build records. Manual lookup
remains available even when a pattern is excluded from autonomous recommendations.

Without an MCP client, pattern bodies are also available over plain HTTP:

```sh
curl https://www.motionmenu.ca/p/150.json
curl https://www.motionmenu.ca/p/velocity-image-bend.md
```

## Supporting tools

The current server also exposes:

- `list_patterns` for catalogue discovery and filtering.
- `get_kit` for the complete design laws and pattern index.
- `recommend_patterns` for quality-gated candidates within a narrower role or intent.
- `check_updates` for catalogue-version and changed-pattern checks.
- `audit_page` and `verify_repair` for deterministic HTML-grammar repair checks on existing pages.

`audit_page` and `verify_repair` do not render a page or judge screenshots. Visual inspection,
accessibility testing, runtime cleanup, and release verification remain the host agent's job.

## Other interfaces

The shadcn registry can retrieve a pattern wrapper directly:

```sh
npx shadcn@latest add "https://www.motionmenu.ca/r/<slug>.json?framework=react"
```

## Links

- [Pattern catalogue](https://www.motionmenu.ca/catalogue)
- [Director and HTTP documentation](https://www.motionmenu.ca/docs)
- [Machine-readable design contract](https://www.motionmenu.ca/p/design-digest.json)
- [Changelog](https://www.motionmenu.ca/changelog)

This repository contains the public registry metadata and install documentation for
`io.github.MotionMenu/motion-menu`. The implementation and pattern source remain in the main
Motion Menu repository.
