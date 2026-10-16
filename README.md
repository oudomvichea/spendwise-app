# Spendwise — Personal Money Tracker

Spendwise is a local-first spending tracker. It includes receipt scanning, on-device AI category suggestions, manual expenses and income, and monthly summaries. It does not use Plaid or a paid AI API.

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

## Use the camera

Choose **Scan receipt → Take a photo** and allow camera access when the browser asks. If camera access is unavailable, use **Choose a photo** instead. Direct camera access requires a secure HTTPS website or localhost. The scanner prepares a resized copy of the image in the browser before OCR. If the photo opens but no item lines are detected, check the lighting and framing or add the items manually. The photo itself is not stored.

## Install on your phone

The ZIP is the app's source files; it is not an Android APK or iPhone App Store download. To add Spendwise to a phone's Home Screen, first publish these static files to a free HTTPS host (for example, GitHub Pages). Then:

- **iPhone:** Open the HTTPS address in Safari, tap Share, choose **Add to Home Screen**, and tap Add.
- **Android:** Open the HTTPS address in Chrome and choose **Install app** or **Add to Home screen** from the menu.

The app includes a web manifest, install icon, and service worker so its interface can open as a home-screen web app. The OCR and AI packages/model still need to download the first time you scan a receipt. Keep an internet connection for the initial scan.

## Data and privacy

- Receipt photos are processed in the browser and are not saved by the app.
- Entries and learned category corrections are stored in this browser on this device.
- Data does not sync between devices. Use **Export backup** to download a JSON backup. Clearing browser data may remove entries.
- The AI model classifies receipt item names inside the browser; there is no per-scan AI API charge.
- Amounts are displayed in USD. This version does not connect to banks.

## Prototype limits

Receipt print layouts vary, so OCR can miss or misread item names, prices, and totals. Review the suggestions before saving. AI categories are suggestions and can be edited.
