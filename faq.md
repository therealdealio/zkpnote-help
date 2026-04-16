# Frequently Asked Questions

## General

### What does ZKP mean?
ZKP stands for Zero-Knowledge Proof — a cryptographic method that lets you prove something (like authorship) without revealing the underlying data. ZKPnote uses this concept to let you prove you wrote a note without exposing its contents.

### What is ZKPnote?
ZKPnote is a privacy-first note-taking app that encrypts your notes end-to-end and anchors them on the Solana blockchain. It combines a markdown editor, a Solana wallet, and a peer-to-peer marketplace for selling knowledge assets.

### Is ZKPnote free?
The core note-taking and encryption features are free. The marketplace charges a 2% fee on sales, which is handled automatically by the on-chain smart contract.

### Is ZKPnote open source?
ZKPnote is proprietary software. The smart contract source, developer documentation, and help guides are publicly available for transparency.

### Can I use a rich text editor instead of Markdown?
Yes! Toggle between Markdown and Rich Text modes using the toolbar. Rich Text gives you a familiar word-processor experience with bold, italic, headings, lists, and more. Your notes are stored as Markdown internally, so you can switch between modes freely.

### Can I use dark or light mode?
Yes, click the sun/moon icon in the editor toolbar to toggle between dark and light mode for the writing area. Your preference is saved automatically.

### Can I use ZKPnote offline?
Yes. Your notes save to your browser's local storage (IndexedDB) as you type, even without an internet connection. When you reconnect, your vault will sync to the cloud. Just don't clear your browser data before syncing — local storage is the primary store until sync completes.

### Can I access my vault from multiple devices?
Yes. Your seed phrase is your key. Enter it on any device with a browser to unlock your vault. Your encrypted notes sync through the cloud, so you'll see the same vault everywhere.

## Vault & Security

### What happens if I lose my seed phrase?
Your vault is gone. There is no password reset, no recovery email, no support ticket. ZKPnote is designed so that only you can access your notes. Write your seed phrase down and store it somewhere safe.

### Can ZKPnote read my notes?
No. All encryption and decryption happens on your device. The server only stores encrypted data. Even the ZKPnote team cannot read your notes.

### What encryption does ZKPnote use?
XChaCha20-Poly1305 via libsodium for note encryption. 256-bit keys derived from your seed phrase or Phantom wallet signature using HKDF.

### Is my data backed up?
Yes. Your encrypted vault is automatically synced to cloud storage. You can also export a backup file manually. Both are encrypted — useless without your seed phrase.

## Marketplace

### Do I need to prove my note before selling?
Yes. You must prove your note on-chain before listing it on the marketplace. Click the Prove button (purple shield icon) in the editor toolbar first. This establishes your authorship on the Solana blockchain before any commercial activity. If you try to sell without proving, you'll be prompted to prove first.

### Can I resell a note I bought?
No. Purchased notes are tagged and the Sell button is disabled. Additionally, if you copy the content into a new note and try to list it, the similarity detection system will block it.

### What are the sale types?
- **Copies:** Unlimited copies, you keep the original
- **Original:** One-time transfer, note removed from your vault
- **Auction:** Timed bidding, highest bidder wins

### How does the auction work?
Set a starting bid, optional reserve price, and duration (1h to 7 days). Bidding doesn't lock SOL — the winner pays when the auction ends by clicking "Pay & Claim Note."

### What's the marketplace fee?
2% of every sale, deducted automatically by the on-chain smart contract. The buyer pays the full price; the contract splits 98% to the seller and 2% to the ZKPnote treasury.

### How does content preview work?
When listing a note, you choose how much buyers see before purchasing:
- **Masked:** Redacted structure showing headings, lists, and paragraph shapes without actual text
- **Partial:** First N characters shown (you control how much via a slider)
- **Full:** Entire note visible before purchase

### What stops someone from copying my content?
Three layers of protection:
1. **Purchase tagging:** Notes bought from the marketplace cannot be relisted
2. **Listing similarity (70%):** When creating a listing, the server compares content against all existing marketplace listings using trigram-based similarity. Anything 70%+ similar to another seller's listing is blocked.
3. **Proof similarity (90%):** Content is also compared against all on-chain proved notes from other authors. If your note is 90%+ similar to another author's proved work, you cannot list it — even if that content was never listed for sale. This protects proved intellectual property beyond the marketplace.

### Can I cancel or unlist something I put on the marketplace?
Yes. Open your listing in the marketplace and click **Cancel Listing**, then confirm. For "Original" listings (where the note was removed from your vault), the note is automatically restored to your vault. For "Copies" and "Auction" listings, the note stays in your vault and the listing is simply removed.

### Can I reorder my notes?
Yes. Hover over a note in the sidebar to reveal a grip handle (six dots), then drag it to a new position. Your custom sort order is saved and persists across sessions.

### Can I pop out a note into its own window?
Yes. Click the pop-out button in the editor toolbar to open the note in a floating window. On Chrome and Edge, the window stays on top of other applications. On Safari and Firefox, it opens as a standard popup. Edits in the floating window sync back to your vault in real time.

## Proof of Originality

### What is Proof of Originality?
Proof of Originality registers a SHA-256 hash of your note on the Solana blockchain. The first person to register a hash wins — it permanently proves that you created that specific content at a specific time. Think of it as a timestamped digital fingerprint that nobody can forge or backdate.

### How do I prove my note?
Open the note in the editor and click the **shield icon** (purple) in the toolbar. ZKPnote hashes your note content and submits a transaction to Solana. You'll need a small amount of SOL to cover the transaction fee. Once confirmed, the shield turns green and a banner shows the Solana Explorer link.

### Can I recover a note from its proof?
Yes. If you've lost a note but still have the proof transaction signature, ZKPnote can recover the full content from the proof database. The recovery is owner-only — only the wallet that created the proof can retrieve the content.

### Can someone else prove my content?
Only if they prove it before you do. Once a SHA-256 hash is registered on-chain, that exact content is claimed — no one else can register the same hash. This is why it's a good idea to prove your original work as soon as you're ready.

### What is the Verify page?
The Verify page is a public page at `/verify` (accessible from the marketplace nav bar and sidebar) where anyone can check whether a piece of content has been proved on-chain. Paste in any text and ZKPnote will check for exact matches and also surface similar proved content. No login or wallet is required.

## Wallet

### Do I need SOL to use ZKPnote?
You need a small amount of SOL for on-chain operations (proving notes, marketplace transactions). On test networks, use the Airdrop button to get free test SOL.

### Can I use my Phantom wallet?
Yes. Click "Connect with Phantom" on the welcome screen. You'll need to sign a message to derive your encryption key (no SOL spent). Your Phantom wallet handles transaction signing. ZKPnote never stores an encrypted seed phrase for Phantom accounts — your wallet remains the only recovery path. You can still add an email address in Profile for news updates.

### Will I see the same notes whether I log in with Phantom or my seed phrase?
Yes. Both login methods derive the same encryption key and wallet address, so you get the same vault regardless of how you log in. Your seed phrase produces the same Solana address as Phantom (via BIP-44 standard derivation), and both modes sign the same internal message to derive identical encryption keys.

### Why does Phantom ask me to sign a message?
ZKPnote uses a deterministic message signature to derive your encryption key. Since Ed25519 signatures are deterministic, the same wallet always produces the same key. This is not a transaction — no SOL is spent.

### Why does my Phantom wallet stay connected now?
ZKPnote caches the encryption signature in your browser session. This means you no longer need to re-approve the signature message every time you refresh the page. Your Phantom wallet stays connected as long as your browser session is active.

## Blockchain

### What gets stored on the blockchain?
Only SHA-256 hashes of your individual notes, plus metadata (timestamp, owner address). Your actual note content is never on-chain.

### Which blockchain does ZKPnote use?
Solana. The smart contract is built with the Anchor framework.

### Can I verify a proof on-chain?
Yes. Each proof creates a PDA (Program Derived Address) derived from the content hash and your wallet address. You can look it up on any Solana explorer using the program ID `Ad67RwgTaeh77UQ5oZXAwt3fTvg3u5oNxNfcc3tGJLbc`. You can also use the [Verify page](/verify) in the marketplace to check any content.

## Sharing

### What happens when I open a share link?
You'll see the note's metadata (title, sender, timestamp) and be asked to connect a wallet before the content is revealed. You can connect with Phantom, paste an existing seed phrase, or — if you're brand new to ZKPnote — sign up inline with a username, email, and password. A fresh wallet is generated on the spot and your encrypted seed is emailed to you via a one-time link.

### Do I need my own vault to read a shared note?
You need a wallet so the sender's access grant can be recorded on-chain. The in-page sign-up flow creates that wallet for you in one step, so brand-new readers don't need to leave the share page.

### Is the share link signed on-chain?
Yes. Standard shares require the reader to sign a view agreement transaction (~0.002 SOL rent). NDA shares additionally require the owner to approve the reader by signing an on-chain NDA contract with the reader's submitted name hash.

## AI Integration

### Can I use ZKPnote with Claude or other AI assistants?
Yes. ZKPnote includes an MCP (Model Context Protocol) server with 18 tools that lets Claude and other AI agents read, write, search, and verify notes in your encrypted vault. You can ask Claude to save research, summarize notes, build a knowledge graph, check on-chain proofs, browse the marketplace, and more — all while your notes stay end-to-end encrypted.

### How do I set up the MCP server?
See the full setup guide: **[Using ZKPnote with Claude (MCP Setup)](./mcp-setup.md)**

The short version:
1. Clone the repo and build: `cd zkpnote/packages/mcp-server && npm install && npm run build`
2. Add the server to your Claude config with your seed phrase and the API URL
3. Restart Claude and ask "List my notes" to verify

The guide covers Claude Desktop, Claude Code CLI, and VS Code setup with copy-paste config snippets.

### Is the MCP server safe?
Yes. The MCP server runs **locally on your machine** — your seed phrase never leaves your device. All encryption happens before data touches the network. The server communicates via stdio (not over a network port) and writes nothing to disk. However, any AI assistant connected through MCP has full vault access, so only use it with trusted clients.

### What tools does the MCP server provide?
18 tools across four categories:

- **Vault** (8 tools) — save, batch save, read, update, delete, reorder, list notes and folders
- **Proofs** (4 tools) — verify content on-chain, search similar proved content, list proofs, recover lost notes
- **Marketplace** (4 tools) — browse listings, get details, cancel listings, view analytics
- **Knowledge Graph** (2 tools) — build a searchable index of all notes with summaries and relationships, query by keyword/project/tag

For the full tool reference, see the [MCP Server Developer Docs](https://github.com/therealdealio/zkpnote-docs/blob/main/mcp-server.md).

### What is ZKPnote's current status?
ZKPnote is currently in **Alpha Testing** on the Solana **devnet**. This means the app is fully functional but uses test SOL (not real money) for all blockchain operations. Mainnet launch is planned for a future release.

## Editor & Productivity

### How do I see all keyboard shortcuts?
Press **⌘/** anywhere in the app. The cheat sheet groups every shortcut by Navigation, Tabs, View, Editor, and Proofs & Sync.

### How do I link between notes?
Type `[[` in any note. As you type, an autocomplete dropdown shows matching note titles. Press Enter or Tab to insert. In the rendered preview, hover any wikilink for a 280ms popover with the target's first 400 characters; click to jump.

### Can I see what links to a note?
Yes. The **Backlinks** panel below the editor shows every note that references the current one (via `[[wikilink]]`), plus every outgoing link.

### How do I undo a change from yesterday?
Click the **clock icon** in the editor toolbar. ZKPnote auto-saves a revision every 10 seconds while you edit, keeping the last 50 per note. View any revision inline, restore with one click. History stays on your device — never synced.

### How do I paste an image?
Press ⌘V in the editor with an image in your clipboard, or click the **image button** in the toolbar. ZKPnote saves it as an encrypted attachment in your vault and inserts a short reference (`zkp-attach:<id>`) into the markdown — your note body stays clean instead of getting filled with a long base64 string.

### Why does my note say `zkp-attach:` instead of showing the image?
That's the markdown reference to an encrypted image attachment in your vault. In the preview pane, ZKPnote resolves it and shows the actual image. The short reference keeps your markdown readable.

### Do tables survive switching between Markdown and Rich Text mode?
Yes. ZKPnote preserves GFM pipe-table syntax round-trip. Edit in either mode and the formatting stays intact when you switch back.

### What is Focus Mode?
Press **⌘.** to collapse the sidebar, status bar, and tabs — just you and the editor. Press Esc or ⌘. again to restore. Use the **Mode ▾** dropdown in the toolbar if you forget the shortcut.

### How do I export multiple notes at once?
Click **Select** at the top of the sidebar. Check the notes you want (Cmd-click or Shift-click for ranges). The action bar lets you **Export .md** (downloads a zip of markdown files) or **Delete** in bulk.

### Can I open multiple notes side by side?
Open several notes — each becomes a tab above the editor. Drag tabs to reorder. Middle-click or **⌘W** closes. **⌘⇧T** reopens the last closed tab. **⌘1**…**⌘9** jumps by index.

### Can I use ZKPnote on my phone?
Yes. ZKPnote is a responsive web app that works in mobile browsers. The wallet bar and Send/Sell buttons hide automatically to keep the interface clean. Open the sidebar via the hamburger button.
