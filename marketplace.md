# Marketplace Guide

The ZKPnote marketplace lets you buy and sell notes, guides, templates, and knowledge assets directly — with payments in SOL and content delivered instantly to your vault.

## Selling a Note

1. Open the note you want to sell
2. Click the **Sell** button ($ icon) in the toolbar
3. Choose your sale type:

### Sale Types

| Type | Description |
|------|-------------|
| **Copies** | Sell unlimited copies. You keep the original in your vault. |
| **Original** | One-time transfer. The note is permanently removed from your vault once listed. |
| **Auction** | Timed bidding. The highest bidder wins when the auction ends. |

### Setting Up Your Listing

- **Price / Starting Bid:** Set in SOL. For auctions, this is the minimum opening bid.
- **Reserve Price** (auction only): Optional minimum price. If the highest bid is below this, the note won't sell.
- **Duration** (auction only): Choose from 1 hour to 7 days.
- **Description:** Up to 300 characters. This is what buyers see before purchasing.
- **Category:** Tutorial, Research, Template, Guide, Course, Recipe, Cheatsheet, General, or Other.
- **Content Preview:** Choose how much buyers see before purchasing:
  - **Masked** (default) — Buyers see the note's structure (headings, lists, paragraphs) with redacted text
  - **Partial** — Buyers see the first N characters of your note as a teaser. Use the slider to choose how much to reveal.
  - **Full** — Buyers see the complete note content before purchasing

### Content Protection

ZKPnote protects sellers from content theft:

- **Purchased notes can't be resold.** The Sell button is disabled for notes you bought from the marketplace.
- **Similarity detection.** If someone copies your content into a new note and tries to list it, the system compares it against existing listings and blocks anything with 70%+ similarity.

## Buying a Note

1. Go to the **Marketplace** (link in the sidebar or top nav)
2. Browse or search for listings by title, author, or keyword. You can also click **Verify** in the nav bar to check if content has been proved on-chain.
3. Filter by category using the pill buttons
4. Click a listing to see details and the content preview — listing previews now show rendered Markdown, so you see formatted text rather than raw code
5. Click **Buy Now** to purchase — an inline confirmation with a full fee breakdown appears before you sign the transaction
6. You can buy the same listing multiple times (useful for gifting or separate vaults)

### What Happens When You Buy

1. Your Solana wallet sends SOL to the smart contract
2. The contract atomically splits the payment: **98% to the seller, 2% marketplace fee**
3. The full note content is delivered to your vault instantly
4. The note appears in your sidebar, ready to read and edit

If you're using Phantom, you'll see a transaction approval popup.

## Auctions

### Bidding

1. Open an auction listing in the marketplace
2. Enter your bid amount (must exceed the current highest bid by at least 0.001 SOL)
3. Click **Bid** — no SOL is charged when bidding
4. Watch the live countdown timer

### Winning

When the auction ends:
- If you're the highest bidder and the reserve price (if any) is met, you'll see a **Pay & Claim Note** button
- Click it to send SOL and receive the note in your vault
- The same 98/2 payment split applies

### If You Don't Win

- If someone outbids you, your bid is simply recorded — no SOL was locked
- If the reserve price isn't met, the auction ends without a sale

## Marketplace Fee

A **2% fee** is automatically deducted from every sale via the on-chain smart contract. This happens atomically — the buyer pays once, and the contract splits the payment between the seller and the ZKPnote treasury.

| Sale Price | Seller Receives | Fee |
|-----------|----------------|-----|
| 0.1 SOL | 0.098 SOL | 0.002 SOL |
| 1.0 SOL | 0.98 SOL | 0.02 SOL |
| 10.0 SOL | 9.80 SOL | 0.20 SOL |

## Verify Page

The **Verify** page lets anyone check whether a piece of content has been proved on-chain — no login or wallet required.

1. Go to the **Marketplace** and click **Verify** in the nav bar (also available in the sidebar)
2. Paste any text into the input field
3. ZKPnote hashes the content and checks it against all on-chain proofs

### What You'll See

- **Exact match:** If someone has proved that exact content, you'll see the proof details — including the prover's address and the Solana transaction link.
- **Similar content:** If no exact match is found, the Verify page also surfaces similar proved content so you can see if a close variation has already been registered.

This is useful for buyers who want to confirm a seller truly proved their work, or for anyone who wants to check if specific content has already been claimed.

## Analytics

Click the **chart icon** in the marketplace nav to view the analytics dashboard:
- Total sales, volume, and fees earned
- Active listings count
- Unique buyers and sellers
- Full transaction history with dates, prices, and transaction signatures
