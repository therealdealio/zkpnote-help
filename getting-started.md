# Getting Started with ZKPnote

ZKPnote is a privacy-first note-taking app with blockchain-backed proof of originality. Your notes are encrypted end-to-end — only you can read them.

## Creating Your Vault

### Option A: Seed Phrase (Recommended)

1. Open ZKPnote and click **Create New Vault**
2. You'll receive a **12-word seed phrase** — this is your master key
3. **Write it down and store it safely.** If you lose it, your vault is gone forever. There is no password reset.
4. Confirm the seed phrase to unlock your vault

Your seed phrase generates both your encryption key (for securing notes) and your Solana wallet (for marketplace transactions).

### Option B: Phantom Wallet

If you already have a Solana wallet via the Phantom browser extension:

1. Click **Connect with Phantom** on the welcome screen
2. Approve the connection in Phantom
3. Sign the key derivation message (this does NOT spend SOL — it generates your encryption key)
4. Your vault is now linked to your Phantom wallet

**Note:** Your Phantom wallet must be a full wallet (not view-only). You'll need to approve two prompts: one to connect, one to sign the key derivation message.

## Writing Notes

- Click **New Note** in the sidebar (or press the + button)
- Write in **Markdown** — ZKPnote supports GitHub Flavored Markdown including headings, lists, code blocks, tables, and more
- Toggle between **Edit** and **Preview** mode using the view toggle in the toolbar
- Notes auto-save as you type

## Organizing with Folders

- Click the folder icon in the sidebar to create folders
- Drag and drop notes between folders
- Folders support nesting (folders within folders)
- Right-click a folder to rename or delete it

## Your Wallet

The wallet bar at the top shows your:
- **Solana address** (click to copy)
- **SOL balance** (auto-refreshes)
- **Send/Receive** buttons for SOL transfers

If you're on a test network, the **Airdrop** button gives you free test SOL.

## Syncing

ZKPnote automatically syncs your encrypted vault to the cloud. You can also:
- **Manual sync:** Click the sync button in the sidebar
- **On-chain hash:** Each sync writes a SHA-256 hash to Solana, creating an immutable timestamp proving your notes existed at that moment

## Locking Your Vault

Click **Lock** in the sidebar to clear all data from memory. You'll need your seed phrase (or Phantom wallet) to unlock again.

## Backup & Restore

- **Export:** Sidebar > Export Vault — downloads an encrypted JSON file
- **Import:** Sidebar > Import Vault — restores from a previously exported file

Your backup file is encrypted with your key. Without your seed phrase, the backup is unreadable.
