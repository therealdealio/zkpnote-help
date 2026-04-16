# Using ZKPnote with Claude (MCP Setup)

ZKPnote has a built-in MCP server that lets Claude (and other AI agents) work directly with your encrypted notes. You can ask Claude to save research, summarize notes, search your vault, check proofs, and more — all while your notes stay end-to-end encrypted.

Two ways to connect Claude to your vault:

- **Local (stdio)** — for **Claude Desktop** and **Claude Code** (CLI / VSCode). Runs a local Node.js process on your machine; your seed phrase never leaves your device.
- **Remote (HTTP)** — for **Claude mobile (iOS / Android)** and **claude.ai on the web**, which only accept remote connectors. Requires hosting a ZKPnote instance because the seed phrase lives in the server's environment.

Pick the mode that matches your client below. You can use both at the same time — they read and write the same encrypted vault.

---

## Option A — Local (stdio): Claude Desktop / Claude Code

### What You'll Need

1. **A ZKPnote vault** created with a **seed phrase** (not Phantom)
2. **Node.js 18 or newer** — download from [nodejs.org](https://nodejs.org)
3. **Claude Desktop**, **Claude Code**, or another stdio-MCP-compatible client

> **Phantom users:** The MCP server requires a seed phrase for encryption. If you use Phantom in the browser, you'll need to create a separate seed-phrase vault for use with Claude. The two vaults will be independent.

### Setup (5 minutes)

**Step 1: Get the MCP server**

```bash
git clone https://github.com/therealdealio/zkpnote.git
cd zkpnote/packages/mcp-server
npm install
npm run build
```

**Step 2: Add to Claude**

Open your Claude configuration file and add the ZKPnote server.

**Claude Desktop** — edit `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "zkpnote": {
      "command": "node",
      "args": ["/full/path/to/zkpnote/packages/mcp-server/build/index.js"],
      "env": {
        "ZKPNOTE_SEED_PHRASE": "your twelve word seed phrase here",
        "ZKPNOTE_API_URL": "https://www.zkpnote.com"
      }
    }
  }
}
```

**Claude Code** — edit `~/.claude/settings.json` with the same structure above.

Replace `/full/path/to/` with the actual path where you cloned the repo, and replace the seed phrase with your real 12-word phrase.

**Step 3: Restart Claude**

- **Claude Desktop:** Quit and reopen the app
- **Claude Code:** Start a new terminal session

**Step 4: Try it out**

Ask Claude any of these:

| Ask Claude... | What happens |
|---------------|-------------|
| "List my notes" | Shows all your notes with titles, folders, and proof status |
| "Read my note about Solana" | Finds and displays the matching note |
| "Save this as a note called 'Meeting Notes'" | Creates a new encrypted note in your vault |
| "Check if this text has been proved" | Verifies content against on-chain Solana proofs |
| "Search for notes about pricing" | Queries the knowledge graph for relevant notes |
| "Build a knowledge graph of my notes" | Indexes all notes with summaries and relationships |

---

## Option B — Remote (HTTP): Claude Mobile + claude.ai Web

Claude on phones and at claude.ai only supports **remote** connectors — they connect to a publicly reachable HTTPS endpoint instead of a local process. ZKPnote ships a matching transport at `POST /api/mcp` that mirrors every tool available over stdio.

> **Important URL note:** Always use `https://www.zkpnote.com/api/mcp` (with `www.`). The bare `zkpnote.com` redirects 307 to `www.`, and most HTTP clients drop the `Authorization` header across redirects — your connector will return `Unauthorized: missing bearer token`.

### Current status

The remote endpoint is **single-tenant** today: the seed phrase and bearer token both live in one Vercel deployment. To use it you need your own hosted instance (fork the repo, deploy to Vercel, paste your own seed + token into the env vars). A managed multi-tenant version — where any ZKPnote account unlocks a personal MCP endpoint — is the planned next step.

### Self-hosting the remote endpoint

1. **Fork** [therealdealio/zkpnote](https://github.com/therealdealio/zkpnote) and deploy to Vercel (one-click Import Project).
2. In Vercel → your project → **Settings → Environment Variables**, add three vars (tick both **Production** and **Preview** scopes):
   - `ZKPNOTE_SEED_PHRASE` — your 12-word BIP-39 phrase
   - `ZKPNOTE_MCP_TOKEN` — a long random secret (generate with `openssl rand -hex 32`)
   - `ZKPNOTE_API_URL=https://www.zkpnote.com`
3. Redeploy so the env vars take effect.
4. **Bot-test** (optional but recommended — the repo ships `scripts/test-mcp-bot.mjs`):

```bash
MCP_URL=https://www.zkpnote.com/api/mcp \
MCP_TOKEN=your_long_random_token \
node scripts/test-mcp-bot.mjs
```

### Add the connector in Claude mobile / web

**Claude mobile (iOS or Android):**

1. Open the Claude app → bottom-right avatar → **Settings**
2. **Connectors** → **Add custom connector**
3. **Name:** `ZKPnote` (anything)
4. **URL:** `https://www.zkpnote.com/api/mcp?token=YOUR_TOKEN` — paste the token you set in Vercel as a query-string parameter (this is how mobile forwards credentials; the server also accepts an `Authorization: Bearer` header for clients that support it)
5. Toggle the connector on for a chat, then ask Claude *"What tools do you have from ZKPnote?"* to confirm.

**claude.ai on the web:**

Settings → Connectors → **Add custom connector** → paste the same URL with the token query parameter.

### Authentication model

- `ZKPNOTE_MCP_TOKEN` is a shared secret. Any request whose bearer token matches it gets full vault access — treat the URL+token combo exactly like you would treat the seed phrase itself.
- Auth is checked on **every** request — there is no session. Rotate by changing the env var in Vercel, redeploying, and updating the connector URL.
- The server refuses to start if the token env var is missing (returns 500 `Server misconfigured`), so a partial config surfaces immediately instead of silently accepting unauthenticated requests.
- If you ever leak the URL (a phone screenshot, a shared config, etc.), **rotate the token** and update the Claude connector URL in every installed client.

---

## What Can Claude Do With Your Notes?

**Read and write:** Claude can save new notes, read existing ones, update content, organize into folders, and delete notes you no longer need.

**Batch operations:** Ask Claude to save multiple notes at once (e.g., "Save these five meeting summaries to my Meetings folder") — it batches them into a single vault sync.

**Search and discover:** The knowledge graph tools let Claude find relevant notes by topic without reading every single note, saving time and tokens.

**Verify proofs:** Claude can check whether content has been proved on Solana, search for similar proved content, and even recover lost notes from proof transaction signatures.

**Browse marketplace:** Claude can search marketplace listings, check prices, and view your sales analytics. (Full listing details + cancel + analytics are stdio-only for now.)

## Is It Safe?

The answer depends on which transport you're using.

### Local (stdio) mode

- Your **seed phrase stays on your computer** — it's only used by the local MCP server process to derive encryption keys
- **All notes are encrypted before leaving your device** using XChaCha20-Poly1305
- The MCP server **runs locally** — it's not a cloud service
- The server **doesn't write anything to disk** — everything runs in memory
- Claude sees your **decrypted notes** only during the conversation

### Remote (HTTP) mode

- Your **seed phrase lives in your Vercel deployment's environment variables** — you are trusting your own hosting provider with the key material, the same way any self-hosted app with a master secret does
- **Encryption still happens on the server before anything is stored in Supabase** — the database only ever sees ciphertext
- A **bearer token** (`ZKPNOTE_MCP_TOKEN`) gates the endpoint; anyone who holds the URL + token can read/write your vault, so treat it like a password
- **All traffic is HTTPS** — the token and JSON-RPC payloads are encrypted in transit
- Claude sees your **decrypted notes** only during the conversation

> **Important:** Any AI assistant connected through MCP has full vault access. Only use it with trusted clients. In remote mode, **rotate your token** if you ever share the connector URL outside of your own devices.

## Troubleshooting

**"No notes found"** — Make sure you're using the same seed phrase you used to create the vault in the web app. Phantom wallets use different encryption keys.

**Server not showing up (stdio)** — Check that the path in your config points to the correct `build/index.js` file (must be an absolute path, not relative). Run `npm run build` if the `build/` folder doesn't exist.

**"Unauthorized: missing bearer token" (remote)** — You're probably using `zkpnote.com` without the `www.` prefix. Switch the connector URL to `https://www.zkpnote.com/api/mcp?token=...`; the 307 redirect from the apex domain strips the `Authorization` header.

**"Server misconfigured: ZKPNOTE_MCP_TOKEN not set" (remote)** — The env var hasn't been set in the Vercel deployment, or you forgot to redeploy after adding it. Double-check Production + Preview scopes are both ticked.

**Slow operations** — Each operation syncs your full encrypted vault. Batch saves (`save_notes`) are much faster than individual saves when creating multiple notes.

## More Info

For technical details, tool reference, and architecture diagrams, see the [MCP Server Developer Docs](https://github.com/therealdealio/zkpnote-docs/blob/main/mcp-server.md).
