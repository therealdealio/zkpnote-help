# Frequently Asked Questions

## General

### What is ZKPnote?
ZKPnote is a privacy-first note-taking app that encrypts your notes end-to-end and anchors them on the Solana blockchain. It combines a markdown editor, a Solana wallet, and a peer-to-peer marketplace for selling knowledge assets.

### Is ZKPnote free?
The core note-taking and encryption features are free. The marketplace charges a 2% fee on sales, which is handled automatically by the on-chain smart contract.

### Is ZKPnote open source?
ZKPnote is proprietary software. The smart contract source, developer documentation, and help guides are publicly available for transparency.

## Vault & Security

### What happens if I lose my seed phrase?
Your vault is gone. There is no password reset, no recovery email, no support ticket. ZKPnote is designed so that only you can access your notes. Write your seed phrase down and store it somewhere safe.

### Can ZKPnote read my notes?
No. All encryption and decryption happens on your device. The server only stores encrypted data. Even the ZKPnote team cannot read your notes.

### What encryption does ZKPnote use?
XChaCha20-Poly1305 via libsodium for note encryption. 256-bit keys derived from your seed phrase or Phantom wallet signature using HKDF.

### Is my data backed up?
Yes. Your encrypted vault is automatically synced to cloud storage. You can also export a backup file manually. Both are encrypted — useless without your seed phrase.

## Wallet

### Do I need SOL to use ZKPnote?
You need a small amount of SOL for on-chain operations (writing vault hashes, marketplace transactions). On test networks, use the Airdrop button to get free test SOL.

### Can I use my Phantom wallet?
Yes. Click "Connect with Phantom" on the welcome screen. You'll need to sign a message to derive your encryption key (no SOL spent). Your Phantom wallet handles transaction signing.

### Why does Phantom ask me to sign a message?
ZKPnote uses a deterministic message signature to derive your encryption key. Since Ed25519 signatures are deterministic, the same wallet always produces the same key. This is not a transaction — no SOL is spent.

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

## Blockchain

### What gets stored on the blockchain?
Only a SHA-256 hash of your encrypted vault, plus metadata (note count, timestamp, owner address). Your actual note content is never on-chain.

### Which blockchain does ZKPnote use?
Solana. The smart contract is built with the Anchor framework.

### Can I verify my vault hash on-chain?
Yes. Your vault's PDA (Program Derived Address) is deterministic based on your wallet address. You can look it up on any Solana explorer using the program ID `9AbLiwQ82manor3YyArrQhhpxPCFha5xbF187EtdDae5`.
