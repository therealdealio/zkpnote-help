# Using ZKPnote with Claude (MCP Setup)

ZKPnote has a built-in MCP server that lets Claude (and other AI agents) work directly with your encrypted notes. You can ask Claude to save research, summarize notes, search your vault, check proofs, and more — all while your notes stay end-to-end encrypted.

## What You'll Need

1. **A ZKPnote vault** created with a **seed phrase** (not Phantom)
2. **Node.js 18 or newer** — download from [nodejs.org](https://nodejs.org)
3. **Claude Desktop**, **Claude Code**, or another MCP-compatible client

> **Phantom users:** The MCP server requires a seed phrase for encryption. If you use Phantom in the browser, you'll need to create a separate seed-phrase vault for use with Claude. The two vaults will be independent.

## Setup (5 minutes)

### Step 1: Get the MCP server

```bash
git clone https://github.com/therealdealio/zkpnote.git
cd zkpnote/packages/mcp-server
npm install
npm run build
```

### Step 2: Add to Claude

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
        "ZKPNOTE_API_URL": "https://zkpnote.vercel.app"
      }
    }
  }
}
```

**Claude Code** — edit `~/.claude/settings.json` with the same structure above.

Replace `/full/path/to/` with the actual path where you cloned the repo, and replace the seed phrase with your real 12-word phrase.

### Step 3: Restart Claude

- **Claude Desktop:** Quit and reopen the app
- **Claude Code:** Start a new terminal session

### Step 4: Try it out

Ask Claude any of these:

| Ask Claude... | What happens |
|---------------|-------------|
| "List my notes" | Shows all your notes with titles, folders, and proof status |
| "Read my note about Solana" | Finds and displays the matching note |
| "Save this as a note called 'Meeting Notes'" | Creates a new encrypted note in your vault |
| "Check if this text has been proved" | Verifies content against on-chain Solana proofs |
| "Search for notes about pricing" | Queries the knowledge graph for relevant notes |
| "Build a knowledge graph of my notes" | Indexes all notes with summaries and relationships |

## What Can Claude Do With Your Notes?

**Read and write:** Claude can save new notes, read existing ones, update content, organize into folders, and delete notes you no longer need.

**Batch operations:** Ask Claude to save multiple notes at once (e.g., "Save these five meeting summaries to my Meetings folder") — it batches them into a single vault sync.

**Search and discover:** The knowledge graph tools let Claude find relevant notes by topic without reading every single note, saving time and tokens.

**Verify proofs:** Claude can check whether content has been proved on Solana, search for similar proved content, and even recover lost notes from proof transaction signatures.

**Browse marketplace:** Claude can search marketplace listings, check prices, and view your sales analytics.

## Is It Safe?

Yes. Here's how your data stays private:

- Your **seed phrase stays on your computer** — it's only used by the local MCP server process to derive encryption keys
- **All notes are encrypted before leaving your device** using XChaCha20-Poly1305 (military-grade encryption)
- The MCP server **runs locally** — it's not a cloud service
- The server **doesn't write anything to disk** — everything runs in memory
- Claude sees your **decrypted notes** only during the conversation — they aren't stored on Anthropic's servers after the session ends

## Troubleshooting

**"No notes found"** — Make sure you're using the same seed phrase you used to create the vault in the web app. Phantom wallets use different encryption keys.

**Server not showing up** — Check that the path in your config points to the correct `build/index.js` file (must be an absolute path, not relative). Run `npm run build` if the `build/` folder doesn't exist.

**Slow operations** — Each operation syncs your full encrypted vault. Batch saves (`save_notes`) are much faster than individual saves when creating multiple notes.

## More Info

For technical details, tool reference, and architecture diagrams, see the [MCP Server Developer Docs](https://github.com/therealdealio/zkpnote-docs/blob/main/mcp-server.md).
