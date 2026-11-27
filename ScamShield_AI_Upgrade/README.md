# ScamShield AI — upgraded prototype

This project preserves the original single-page React/Tailwind design and upgrades the scanning workflow.

## What works now

- Existing dark navy/teal landing page, dashboard, sidebar, scanners, result page and Safety Academy are retained.
- Message and URL checks call `/api/analyze` for Gemini AI when deployed with the server endpoint configured.
- Screenshot uploads (PNG/JPEG/WEBP, up to 5 MB) can be sent to the AI endpoint for visual analysis.
- If the AI endpoint is not available, message and URL scans fall back to clearly labelled local pattern checks. Screenshot analysis reports that the endpoint is unavailable rather than pretending to analyse the image.
- Scan history persists in the current browser using `localStorage` (up to 100 scans).
- Dashboard statistics are calculated from actual local scans; fabricated sample statistics/history have been removed.
- Login is explicitly guest-only. It does not pretend to create accounts.

## Run locally

The HTML uses CDN-hosted React, Babel and Tailwind, so an internet connection is needed to load the libraries.

1. Install Node.js (LTS).
2. In this folder run: `npx vercel dev`
3. Create a Gemini API key in Google AI Studio.
4. Add `GEMINI_API_KEY` to the local Vercel environment (for example in `.env.local`; do not commit it).
5. Open the local URL printed by Vercel.

The API route is `api/analyze.js`. The key is read only on the server and is never embedded in the browser code.

## Deploy free-tier prototype

1. Upload this folder to a private GitHub repository.
2. Import the repository into Vercel.
3. In Vercel → Project → Settings → Environment Variables, add:
   - `GEMINI_API_KEY` = your secret API key
   - `GEMINI_MODEL` = `gemini-2.5-flash` (optional; the same default is used)
4. Redeploy the project.
5. Test message, URL and screenshot analysis on the deployed domain.

Free-tier availability and quotas can change. Gemini API usage is subject to Google's current model, region, rate and data-use terms. Never put the API key in `index.html`, a public GitHub repository, or a frontend environment variable.

## Data and privacy limitations

- Scan history is saved only in the browser's local storage. It is not cloud backup, not account-based, and can be erased by clearing browser data.
- When AI analysis is used, submitted text or image content is sent to Google's Gemini API through the server endpoint. Do not submit OTPs, passwords, payment card details, government ID numbers or private content without permission.
- The app does not visit URLs, crawl websites, query a threat-intelligence database, or verify sender identity/domain ownership.
- A risk score is an AI-generated indication, not a probability or guarantee. False positives and false negatives are possible.
- Authentication, cross-device cloud history, admin tools, and production-grade rate limiting are not included yet. Add Supabase Auth/DB and appropriate abuse controls before public launch.

## Main files

- `index.html` — existing app with functional scan calls and local history persistence.
- `api/analyze.js` — server-side Gemini API proxy; secret remains server-side.
- `.env.example` — environment variable names only (no real credentials).
