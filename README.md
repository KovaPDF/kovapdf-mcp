# KovaPDF MCP server

KovaPDF is a hosted MCP server for working on PDF files from an AI assistant: compress, merge, split, reorder or rotate pages, convert to and from Word, Excel and pictures, read the text in scans, redact private details for good, check digital signatures, send documents for signature, and convert Bangla or Hindi text between Bijoy, Kruti Dev and Unicode.

There is nothing to install or build. The server runs at:

```
https://mcp.kovapdf.com
```

It speaks streamable HTTP. Sign in is standard OAuth: the first tool call opens the KovaPDF connect page in your browser, you sign in to your KovaPDF account and choose Allow. No API key goes into any settings file. Every job uses the usual limits of the account you connect, and files and results are deleted within an hour.

**You need a KovaPDF account.** A free account is enough to start ([sign up](https://www.kovapdf.com/register)). The tools on the KovaPDF website work without an account, but an assistant always works through the account you connect. The server itself is hosted by KovaPDF; it is not open source.

## Connect it

**Claude (claude.ai and the desktop app):** open Customize, then Connectors. Choose Add, then Add custom connector, name it KovaPDF and paste `https://mcp.kovapdf.com`. On a Team or Enterprise plan, an Owner adds it first under Organization settings, then Connectors. In a chat, choose + and then Connectors to turn KovaPDF on. Custom connectors depend on your Claude plan.

**ChatGPT (web):** open Plugins, choose the + button, then Add custom MCP server. Name it KovaPDF, paste `https://mcp.kovapdf.com` as the server URL, choose OAuth, read the note, choose I understand and want to continue, then Create as a plugin. In a chat, type @ and choose KovaPDF. This option depends on your ChatGPT plan, and in a work or school workspace an admin may need to allow it.

**Claude Code:**

```
claude mcp add --transport http kovapdf https://mcp.kovapdf.com
```

Or install the plugin, which adds the server and a skill: https://github.com/KovaPDF/claude-plugin

**Cursor:** add this to `.cursor/mcp.json` in a project, or to `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "kovapdf": { "url": "https://mcp.kovapdf.com" } } }
```

**VS Code:** add this to `.vscode/mcp.json`:

```json
{ "servers": { "kovapdf": { "type": "http", "url": "https://mcp.kovapdf.com" } } }
```

**Gemini CLI:** install the extension from https://github.com/KovaPDF/gemini-cli-extension, or add this to `settings.json`:

```json
{ "mcpServers": { "kovapdf": { "httpUrl": "https://mcp.kovapdf.com" } } }
```

**Cline:** see [llms-install.md](llms-install.md).

**Any other MCP client:** add a remote server with the URL `https://mcp.kovapdf.com` and the streamable HTTP transport. The server publishes its OAuth details at `https://mcp.kovapdf.com/.well-known/oauth-protected-resource`, so a client that follows the MCP authorization spec finds the rest by itself.

## Tools

| Tool | What it does |
|---|---|
| `compress_pdf` | Makes a PDF smaller, by level or to a target size. |
| `merge_pdfs` | Joins two or more PDFs in the order given. |
| `split_pdf` | Every page on its own, chosen pages, every N pages, or parts under a size. |
| `organize_pages` | Reorders, rotates and deletes pages. |
| `convert_pdf` | PDF to Word, Excel or pictures; Word, Excel, PowerPoint, OpenDocument or pictures to PDF. |
| `ocr_pdf` | Reads the text in a scanned PDF or a picture. |
| `redact_pdf` | Removes words or kinds of details (email, phone, card, date, url, iban, ssn) for good, with a list of every removal. |
| `verify_pdf_signature` | Checks every digital signature in a PDF and reports who signed and whether the file changed after signing. |
| `create_signature_request_draft` | Prepares a signature request without sending anything. |
| `send_signature_request` | Sends a prepared request after the person confirms. |
| `list_signature_requests` | Lists the person's signature requests. |
| `get_signature_request` | Shows one request and who has signed. |
| `bijoy_to_unicode` | Bangla text typed in Bijoy to Unicode. |
| `unicode_to_bijoy` | Unicode Bangla to Bijoy. |
| `kruti_to_unicode` | Hindi text typed in Kruti Dev to Unicode. |
| `get_job_result` | The state and result links of a job. |
| `get_my_usage` | Tasks left on the connected account. |

Files go in as a link (`url`), a small file as `base64`, or a `file_id` from an earlier upload. Results come back as short lived download links.

## Example

Ask your assistant:

> Compress https://example.com/annual-report.pdf to under 500 KB and give me the link.

It calls `compress_pdf` with arguments like these:

```json
{ "file": { "url": "https://example.com/annual-report.pdf" }, "targetKb": 500 }
```

The answer has a download link for the smaller PDF. Long jobs return a job id first; the assistant then reads the result with `get_job_result`.

## Troubleshooting

- **The connect page did not open.** Your client starts sign in on the first tool call. If nothing opens, look for a blocked pop up, or reconnect KovaPDF in your client's connector or MCP settings.
- **401 or "sign in" errors.** The connection has expired or was removed. Reconnect and choose Allow again.
- **A limit message.** Ask the assistant to run `get_my_usage`; it shows what is left on your account and when more comes back.
- **A link could not be downloaded.** `url` must be a public https address. For a private file, attach it in the chat if your client supports that, or send a small file as `base64`.
- **Something else.** Error codes are listed at https://www.kovapdf.com/developers/errors, and you can reach us at https://www.kovapdf.com/contact.

## Listed on

- Official MCP Registry: `com.kovapdf/kovapdf` (`server.json` in this repository is the same entry)
- Docs: https://www.kovapdf.com/developers/docs/connect

## Licence

The text and configuration files here are under the MIT License (see `LICENSE`). The KovaPDF name and logo belong to KovaPDF and are not covered by this licence (see `NOTICE`).

Privacy: https://www.kovapdf.com/legal/privacy · Terms: https://www.kovapdf.com/legal/terms · Security: see `SECURITY.md`.
