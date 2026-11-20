# Installing Motion Menu Director v1 in Cline

Motion Menu is a remote streamable-HTTP MCP server. All 597 patterns and every Director tool are
free; no entitlement key or authorization header is required.

## Install

```sh
cline mcp install motion-menu https://www.motionmenu.ca/api/mcp --transport streamable-http --yes
```

This writes a `streamableHttp` entry to Cline's MCP settings:

```json
{
  "mcpServers": {
    "motion-menu": {
      "transport": {
        "type": "streamableHttp",
        "url": "https://www.motionmenu.ca/api/mcp"
      }
    }
  }
}
```

No further configuration is required. Restart or reconnect Cline if its MCP tool list was already
open when you installed the server.

## Use Director mode

For autonomous design work, call the tools in this order:

1. `get_design_digest {}`
2. `propose_directions {brief, seed}`
3. Choose a returned `direction_id`
4. `build_page {intent, direction_id, seed, history, exclusions}`
5. `validate_combination {components}`
6. `get_pattern {slug, framework?}`

The `intent` passed to `build_page` must be the same text as the direction `brief`, and the seed
must also remain unchanged. Use `history` to cool down recently repeated patterns and `exclusions`
for hard vetoes.

Motion Menu returns design direction, composition evidence, and transplantable code. Cline remains
responsible for adapting that code to the real project, rendering it, and checking desktop,
mobile, reduced motion, accessibility, and runtime behavior.

## Use manual pattern mode

If the user specifies a pattern number or slug, skip autonomous selection and call `get_pattern`
directly. For example:

```json
{ "number": "150", "framework": "react" }
```

An explicit human selection remains available even when that pattern is not eligible for an
autonomous page plan.

See [README.md](./README.md) for connection examples, supporting tools, and the full workflow.
