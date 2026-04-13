# Getting Started with ZKPnote

ZKPnote is a privacy-first note-taking app with blockchain-backed proof of originality. Your notes are encrypted end-to-end — only you can read them.

**The app is live at [zkpnote.vercel.app](https://zkpnote.vercel.app).**

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

**Persistent sessions:** Phantom sessions persist across page refreshes. Once you approve the signature message, ZKPnote caches it in your browser session so you won't need to re-approve every time you reload the page.

**Optional email for news updates:** Phantom users can open **Profile** and add an email address — purely for product news. No seed phrase is ever stored server-side for Phantom accounts; your wallet remains the only recovery path.

### Option C: Sign Up from a Shared Note Link

If someone sends you a ZKPnote share link and you don't yet have a wallet, you can create one inline:

1. Open the share link
2. Choose **Sign Up with Email**
3. Enter a **username**, **email**, **password**, and **verify password**
4. ZKPnote generates a brand-new 12-word seed phrase in your browser, encrypts it with your password (AES-GCM, PBKDF2), and emails you a secure one-time link so you can recover the phrase later
5. Your account is registered and you can immediately view the shared note

This path is for people who are brand new to ZKPnote — existing users should still unlock their vault normally before viewing a share.

## Writing Notes

- Click **New Note** in the sidebar (or press the + button)
- Write in **Markdown** or **Rich Text** — toggle between modes using the toolbar. Rich Text gives you a familiar word-processor experience; your notes are stored as Markdown internally, so you can switch freely.
- ZKPnote supports GitHub Flavored Markdown including headings, lists, code blocks, tables, and more
- Toggle between **Edit** and **Preview** mode using the view toggle in the toolbar
- Toggle **dark/light theme** for the editor by clicking the sun/moon icon in the toolbar. Your preference is saved automatically.
- Notes auto-save as you type

## Proving a Note

ZKPnote lets you prove you were the first person to create a piece of content — directly on the Solana blockchain.

1. Open the note you want to prove
2. Click the **Prove** button (purple shield icon) in the editor toolbar
3. ZKPnote computes a SHA-256 hash of your note and registers it on Solana in a single transaction
4. Once confirmed, the shield icon turns **green** to indicate the note is proved
5. A banner appears at the top of the note with a link to the transaction on **Solana Explorer**

That's it. Your note's content fingerprint is now permanently recorded on-chain with a timestamp. If anyone ever questions who wrote it first, the blockchain has your receipt.

**Cost:** Each proof requires a small SOL transaction fee (fractions of a cent on mainnet).

### Proof Icons

After proving a note, a **green shield icon** appears next to it in the sidebar. This makes it easy to see at a glance which notes have been proved and which haven't.

## Reordering Notes

You can reorder notes within a folder using drag and drop:

1. Hover over a note in the sidebar — a **grip handle** (six dots) appears on the left
2. Drag the note to a new position
3. A **purple indicator line** shows where the note will be placed
4. Drop to confirm — your custom order is saved automatically

Notes with a custom order keep their position even after editing. Notes without a custom order fall back to sorting by last edited.

## Pop-Out Floating Window

Want to keep a note visible while working in other apps? Pop it out into a floating window:

1. Open a note in the editor
2. Click the **pop-out button** in the toolbar (next to the theme toggle)
3. The note opens in a floating window that stays on top of other applications (Chrome/Edge)
4. Edit the title or content directly in the floating window — changes sync back to your vault in real time
5. The floating window shows a live word count
6. Close the floating window by clicking its close button, or switch notes in the main app (the pop-out auto-closes)

**Note:** On Safari and Firefox, the pop-out opens as a standard popup window (no always-on-top behavior). Chrome and Edge support full always-on-top via the Document Picture-in-Picture API.

## Searching Notes

Use the search bar at the top of the sidebar to quickly find notes:

- Type any keyword — notes are filtered in real time by **title** and **content**
- Matching is case-insensitive
- While searching, the folder tree flattens to show all matching notes regardless of which folder they're in
- Click the **X** button (or clear the input) to return to the normal folder view

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

Sync pushes your encrypted notes to Supabase so they're available across devices. Your actual note content is never readable by the server — only encrypted blobs are stored.

To prove individual notes on-chain, use the **Prove** button in the editor toolbar (see [Proving a Note](#proving-a-note) above).

## Locking Your Vault

Click **Lock** in the sidebar to clear all data from memory. You'll need your seed phrase (or Phantom wallet) to unlock again.

## Backup & Restore

- **Export:** Sidebar > Export Vault — downloads an encrypted JSON file. You can also export individual notes as `.md` Markdown files.
- **Import:** Sidebar > Import Vault — restores from a previously exported JSON file. You can also import `.md` Markdown files directly as new notes.

Your backup file is encrypted with your key. Without your seed phrase, the backup is unreadable.
