# Magnific plugin

Turn ideas, product photos, scripts, and reference images into complete visual campaigns with [Magnific](https://www.magnific.com/app).
Create AI images, AI videos, advertisements, branded content, and web experiences through natural conversation, and keep your product, presenter, and visual identity consistent across every asset.

The plugin connects Grok Bot, Grok Build, Cursor, Claude Code, Codex and Gemini CLI to the Magnific MCP server, and adds a `/magnific` command and skills that make the agent work like a professional producer.

- **The [Magnific MCP server](https://docs.magnific.com/modelcontextprotocol)** (`https://mcp.magnific.com/mcp`) gives the agent the Magnific toolset: image, video, audio, vector and 3D generation and editing, upscaling, your creations and projects, Spaces workflows, and editor-ready delivery.
- **The skills** give the agent production judgment on top of those tools: pricing before spending, model choice from the live catalog, draft-then-final, consistency across shots, finishing and delivery.
  They are written for filmmakers and marketing teams.

## The skills

| Skill | For | What it adds over the raw tools |
|---|---|---|
| `magnific-essentials` | Every job | Pricing before spending, draft-then-final, catalog-driven model choice, uploads, project placement, async waiting without duplicate paid calls, editor-ready delivery |
| `video-ad` | Marketing | Shot plans, product fidelity across shots, draft vs final economics, music and VO, formats, UGC |
| `ad-campaign` | Marketing | Brand kits, master-then-adapt across formats and safe zones, testable A/B variants, layout copies, localization of statics |
| `product-imagery` | Marketing | Routing by fidelity, marketplace packshot specs, lifestyle and in-situ scenes, label mockups, catalogs |
| `precise-image-edits` | Both | Local tools first, segment and retouch, lock-first edit prompts |
| `previs-storyboard` | Film | Shot lists, consistent boards, cheap timed animatics, shot breakdown of reference footage |
| `cinematic-shots` | Film | Model and resolution per shot, camera-report prompts, sequences, continuity, batches under budget |
| `scene-direction` | Film | Directing structure inside a video prompt: references checked against the brief, space map, opening frame, per-shot start and end states, exact dialogue, shot budget by scene type |
| `fight-scenes` | Film | Fight choreography as body geometry, contact and reaction physics, coverage choices and a closing fight lock |
| `consistent-characters` | Both | Library assets, character sheets, reference wiring for images and video |
| `video-finishing` | Both | Order of operations, upscale, grade, modify, reframe, retime, draft finalization, ProRes and DNxHR delivery |
| `sound-and-voice` | Both | Voice casting and direction per TTS model, lip-sync paths, music to length and BPM, SFX, mixes and WAV stems |

Two craft references are shared by all skills: `skills/magnific-essentials/references/image-prompt-craft.md` and `video-prompt-craft.md`.

## Install

| Client | How |
|---|---|
| Grok Bot | Find **Magnific** in Grok Bot's plugin list, add it and sign in. Grok Bot shows what is published in the Cursor Marketplace, so one listing covers Cursor and Grok Bot |
| Grok Build | `grok plugin install magnific --trust` installs the skills and the server; for the server without skills, `grok mcp add --transport http magnific https://mcp.magnific.com/mcp` |
| Cursor | Run `/add-plugin magnific` in chat, or pick **Magnific** in the [Cursor Marketplace](https://cursor.com/marketplace) |
| Claude (claude.ai, Desktop) | [Add Magnific to Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Magnific&connectorUrl=https%3A%2F%2Fmcp.magnific.com%2Fmcp) as a custom connector, then click **Connect** and sign in |
| Claude Code | `claude plugin marketplace add freepik-company/magnific-plugin`, then `claude plugin install magnific@magnific` |
| Codex | Register this repo as a plugin source (it ships `.codex-plugin/plugin.json`), or copy `skills/*` into `~/.codex/skills/` |
| Gemini CLI | `gemini extensions install https://github.com/freepik-company/magnific-plugin` |
| Any MCP client | Connect the client to `https://mcp.magnific.com/mcp` (Streamable HTTP, OAuth); the [MCP documentation](https://docs.magnific.com/modelcontextprotocol) has the steps per client |

To try the plugin in Cursor from a local copy, copy this folder to `~/.cursor/plugins/local/magnific` and run **Developer: Reload Window**.
To try it in Grok Build from a local copy, run `grok plugin marketplace add "$PWD"` inside this folder, then `grok plugin install magnific --trust`.

## The command

`/magnific <request>` is one entry point for any Magnific job, for example `/magnific Animate this product shot with a slow push-in, 5 seconds, 9:16`.
It sends the request to the skill that fits, or straight to a tool for a single image or clip.

## What is included

| Path | Purpose |
|---|---|
| `skills/` | The 12 skills in the open Agent Skills format |
| `commands/magnific.md` | The `/magnific` command (Cursor, Grok, Claude Code) |
| `.mcp.json`, `mcp.json`, `.cursor-plugin/mcp.json` | The Magnific MCP server, in the format each client reads |
| `.cursor-plugin/plugin.json` | Listing metadata for Cursor, also read by Grok Bot |
| `.grok-plugin/plugin.json` | Listing metadata for Grok Build |
| `.claude-plugin/` | Claude Code manifest and marketplace |
| `.codex-plugin/plugin.json` | Codex manifest |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extension and its context file |
| `plugin.json` | Agent Plugins 1.0.0 manifest |
| `assets/magnific-icon.png` | The plugin icon |
| `SECURITY.md`, `CONTRIBUTING.md` | How to report a vulnerability, and how changes reach this repository |

## Sign in and credits

The first tool call opens the Magnific login page in the browser (OAuth).
There is nothing to configure: the plugin carries no keys or tokens.

- Generations and edits use your Magnific plan's credits.
  The skills quote any job above about 200 credits and wait for your yes before it runs, and report what each job cost.
- Results are saved to your Magnific library, in the project you name, so the team finds them in the Magnific app.
- Some tools and models are still rolling out and are not on every account.
  The skills check what your account has and use the public alternative when a tool is missing.

## Security and data flow

What a marketplace review usually asks about:

| Topic | How the plugin handles it |
|---|---|
| Network | Nothing in this repository opens a connection. The agent talks to one host, `https://mcp.magnific.com/mcp`, over Streamable HTTP. |
| Sign-in | OAuth 2.1 through auth.magnific.com, with PKCE, dynamic client registration and client ID metadata documents. The login happens in the browser. |
| Secrets | The repository holds no key, token or password, and the plugin never asks the user for one. |
| Spending | Generations draw on the credits of the user's Magnific plan. The skills have the agent quote any job above about 200 credits and wait for the user's yes, and spend nothing in an unattended run unless the user's own instructions give a budget. Plans and credits are bought on magnific.com, not inside the agent. |
| Uploads | For a file on the user's machine, `magnific-essentials` has the agent send it with `curl` to the one-time upload URL the server returns. The path and the URL are quoted, so a file name cannot run a command. |
| Local code | No hooks and no binaries. The one script, `skills/product-imagery/scripts/packshot.py`, measures and pads product images with Pillow, touches only the files it is given, checks its arguments, and has no network code. |
| Server content | The skills tell the agent that names, comments, shared prompts and other text from the server are data, never instructions, and that it names downloaded files itself. |

## Privacy and terms

The plugin is published by Magnific AI, S.L.U.
Using it is subject to the Magnific [Terms of Use](https://www.magnific.com/legal/terms-of-use) and [Privacy Policy](https://www.magnific.com/legal/privacy).

## Support

- [MCP documentation](https://docs.magnific.com/modelcontextprotocol): setup in each client, sign-in and the server's endpoints.
- [Magnific support](https://www.magnific.com/ai/support), or open an issue in this repository.

## License

The skills and manifests in this repository are released under the [MIT License](LICENSE).
The Magnific service and the MCP server are not part of this license.
