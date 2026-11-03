# Kimi Code compatibility proposal

`kimi.plugin.json` exposes the existing 12 skills, command and hosted MCP connection using Kimi Code's [plugin manifest](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/plugins.html). It adds no hooks or dependencies and leaves the validated skills unchanged.

## Test this checkout

In Kimi Code, run `/plugins install <absolute-path-to-this-checkout>`, then `/plugins info magnific` to inspect diagnostics. Run `/reload` or start a new session. If needed, enable the server with `/plugins mcp enable magnific magnific` and reload.

Use `/magnific:magnific What is my credit balance?` for the command, or `/skill:magnific:magnific-essentials` for the foundation skill. Complete Magnific OAuth through the client. Account and credit requirements, data handling and service terms are described in [README.md](README.md).

## Review checklist

- Confirm all 12 skills and the namespaced command are discovered without diagnostics.
- Verify OAuth and a read-only account/tool-discovery request.
- Estimate a small generation with `simulate_cost`; obtain the user's approval before spending credits, then verify job completion and output retrieval.

This proposal does not claim a completed OAuth or generation test in Kimi. Installing a custom plugin does not publish it in Kimi's Curated catalog; that requires a separate partner listing. After this change is accepted and included in a release, the repository URL can be used with `/plugins install https://github.com/freepik-company/magnific-plugin`.
