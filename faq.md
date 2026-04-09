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
Two layers of protection:
1. **Purchase tagging:** Notes bought from the marketplace cannot be relisted
2. **Similarity detection:** When creating a listing, the server compares content against all existing listings using trigram-based similarity. Anything 70%+ similar to another seller's listing is blocked.

## Proof of Originality

### What is Proof of Originality?
Proof of Originality registers a SHA-256 hash of your note on the Solana blockchain. The first person to register a hash wins — it permanently proves that you created that specific content at a specific time. Think of it as a timestamped digital fingerprint that nobody can forge or backdate.

### How do I prove my note?
Open the note in the editor and click the **shield icon** (purple) in the toolbar. ZKPnote hashes your note content and submits a transaction to Solana. You'll need a small amount of SOL to cover the transaction fee. Once confirmed, the shield turns green and a banner shows the Solana Explorer link.

### Can someone else prove my content?
Only if they prove it before you do. Once a SHA-256 hash is registered on-chain, that exact content is claimed — no one else can register the same hash. This is why it's a good idea to prove your original work as soon as you're ready.

### What is the Verify page?
The Verify page is a public page at `/verify` (accessible from the marketplace nav bar and sidebar) where anyone can check whether a piece of content has been proved on-chain. Paste in any text and ZKPnote will check for exact matches and also surface similar proved content. No login or wallet is required.

## Wallet

### Do I need SOL to use ZKPnote?
You need a small amount of SOL for on-chain operations (proving notes, marketplace transactions). On test networks, use the Airdrop button to get free test SOL.

### Can I use my Phantom wallet?
Yes. Click "Connect with Phantom" on the welcome screen. You'll need to sign a message to derive your encryption key (no SOL spent). Your Phantom wallet handles transaction signing.

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

## AI Integration

### Can I use ZKPnote with Claude or other AI assistants?
Yes. ZKPnote includes an MCP (Model Context Protocol) server that lets AI assistants like Claude interact directly with your vault. The MCP server can save, read, update, delete, and list notes, as well as verify content and search for similar proved content.

### How do I set up the MCP server?
1. Build the MCP server from `packages/mcp-server/` in the ZKPnote source
2. Set the `ZKPNOTE_SEED_PHRASE` environment variable to your vault seed phrase
3. Configure your AI client (e.g., Claude Desktop) to use the server via stdio transport
4. The server derives your encryption keys from the seed phrase — it has full read/write access to your vault

### Is the MCP server safe?
The MCP server runs locally on your machine and communicates via stdio (not over a network). Your seed phrase is used only to derive keys. However, any AI assistant connected through the MCP server has full access to your vault, so only use it with trusted AI clients.

### What is ZKPnote's current status?
ZKPnote is currently in **Alpha Testing** on the Solana **devnet**. This means the app is fully functional but uses test SOL (not real money) for all blockchain operations. Mainnet launch is planned for a future release.
