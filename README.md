# Spendwise — Personal Money Tracker

Spendwise is a local-first spending tracker. It scans receipts, suggests categories, tracks income and expenses, and summarizes spending by month. Entries stay in this browser on this device; they do not sync automatically.

## Run on Windows

1. Unzip the project.
2. Open the `receipt-spending-tracker` folder.
3. In File Explorer, click the address bar, type `cmd`, and press Enter.
4. Run:

   ```bat
   py -m http.server 8000
   ```

5. Open `http://localhost:8000` in Chrome or Edge.
6. Stop the server with `Ctrl+C` in the Command Prompt window.

## Receipt scanning and categorization

Choose **Scan receipt → Take a photo** and allow camera access. The app reads the receipt in your browser. It first uses saved corrections and product-name matching. If configured, it sends only item names (not receipt photos, amounts, or store names) to the OpenAI categorizer. Otherwise it uses its on-device model for less familiar items. Review suggestions before saving.

Photos and PDFs are processed in the browser and are not stored. PDFs with selectable text are read directly; scanned PDFs use OCR (up to the first 5 pages). A clear photo or PDF helps the scanner recognize item names and prices.

## Monthly category totals

Use the month selector at the top of the dashboard. **Monthly spending by category** shows each category's expense total and share for that selected month.

## Deleting expenses and receipts

In **Recent activity**, use **×** to delete only that row. The receipt icon deletes every saved entry from that receipt, including its tax line, after a confirmation prompt. Deleting an item does not automatically recalculate the receipt tax.

## Optional OpenAI API setup

API categorization is optional. API use is billed separately from a ChatGPT subscription, and costs depend on the model and tokens used. Check current rates in the [OpenAI API pricing page](https://developers.openai.com/api/docs/pricing).

The app must call a backend because this app's browser code is public. **Never put your OpenAI API key in `index.html`, the GitHub repository, or the phone app.** The included Cloudflare Worker keeps the key in a server-side secret and sends only product names to OpenAI.

1. Create a Cloudflare account and install Wrangler on your computer (Node.js is required).
2. In a command prompt, open the `receipt-spending-tracker\worker` folder and run `npx wrangler login`.
3. Add your API key as a Cloudflare secret:

   ```bat
   npx wrangler secret put OPENAI_API_KEY
   ```

   Paste the key into the Wrangler prompt; do not put it in a command or file.
4. Deploy the Worker:

   ```bat
   npx wrangler deploy
   ```

5. Copy the deployed Worker URL. In `index.html`, set `OPENAI_CATEGORIZER_URL` to that URL ending in `/categorize` (for example, `https://spendwise-categorizer.your-account.workers.dev/categorize`).
6. Upload the updated app files to GitHub Pages and wait for deployment.
7. In the OpenAI API project settings, set a hard usage limit and monitor usage. A browser-origin allowlist helps with cross-origin browser requests, but it is not authentication; configure rate limiting for the Worker because its URL is public.

The Worker allows requests from `https://oudomvichea.github.io` by default. If you publish the app at another origin, update `APP_ORIGIN` in `worker/wrangler.toml` before deploying the Worker. The current Worker uses `gpt-6-luna`; change `OPENAI_MODEL` there if you want a different supported model.

## Install on your phone

Publish the static app files to an HTTPS host such as GitHub Pages, then:

- **iPhone:** Open the HTTPS address in Safari, tap Share, choose **Add to Home Screen**, and tap Add.
- **Android:** Open the HTTPS address in Chrome and choose **Install app** or **Add to Home screen** from the menu.

The app includes a web manifest, icons, and service worker. OCR, PDF, on-device AI, and OpenAI API features need an internet connection when their libraries or services are used.

## Data and privacy

- Transactions, receipts' parsed items, and saved category corrections are stored in browser storage on this device.
- Data does not sync between devices. Use **Export backup** to download a JSON backup. Clearing browser data may remove entries.
- When OpenAI categorization is enabled, only product names are sent to the configured Worker and OpenAI. Receipt photos are not sent to OpenAI by this feature.
- The app does not connect to banks. Amounts are displayed in USD.

## Prototype limits

Receipt layouts and abbreviated product names vary. OCR and automatic category suggestions can still be wrong; review unusual or unclear lines. Deleting an item does not recompute the remaining receipt's tax allocation.
