# RepairPassport — MVP prototype

**Live app:** https://sheinorshin.github.io/repairpassport/

RepairPassport is a repair card for small phone repair shops in Kazakhstan. At intake the technician
records the device, intake photos, a condition check (works / not working / not tested) and a
preliminary price in about a minute, then sends the card to the customer by WhatsApp or prints it.
Price changes and extra work are approved by the customer with a one-tap reply that carries a
6-character approval code, so the shop keeps proof of what was agreed.

Built for Assignment 5 (Creating an MVP), Technological Entrepreneurship, Astana IT University.
Team: Yerassyl Duisembay, Sayat Abdirayim, Nikita Yurin (group CS-2427).

## Main user flow
1. **Shop:** set up once (shop name + WhatsApp number) → **New card** → model, problem, up to 3 photos,
   condition check, visible damage, preliminary price → **Save card** (a timer measures the time per card).
2. **Shop → customer:** **WhatsApp** / **Share** sends a link to the card; **Print** gives a large-text
   paper card with a QR code and signature lines for customers who do not use links.
3. **Diagnosis / extra work:** the shop adds a price → **Ask customer to approve** → the customer opens
   the link and taps **I agree / I don't agree** → WhatsApp opens with a ready reply and a code →
   the shop enters the code → the approval is recorded with time and method.
4. **Status:** Accepted → Diagnosis → Waiting for part → In repair → Ready → Picked up, sent as an update.

## How it works (no server)
- One static `index.html`. Shop data is stored in the browser's `localStorage` on the shop's phone.
- The customer link carries a copy of the card in the URL **fragment** (`#c=…`, deflate + base64url).
  Browsers never send the fragment to the web server, so GitHub Pages never receives customer data.
- Approval codes are the first 30 bits of SHA-256 over card id, item, description, price and decision,
  so a code only matches the exact price the customer saw.
- Customer data in links is untrusted input: all text is HTML-escaped and only `data:image/*;base64`
  photos are rendered.
- Interface languages: Russian, Kazakh, English.

## Testing support
- **Settings → Test session mode:** enter a tester label (e.g. `A1` for alpha, `B1` for beta). After each
  card the app asks the observer whether the tester needed help.
- **Settings → Export test data (CSV):** exports `cards` (time per card, photos, checks, price,
  sent/printed, complete card, help, approvals) and `events` (every action with a timestamp).

## Not implemented in this MVP
Server and accounts (cards live on one phone; each update is sent as a new link), staff access on
several phones, price breakdown by part and labour, warranty terms, payments, inventory.

## Third-party code
QR codes: [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) 1.4.4,
© 2009 Kazuhiko Arase, MIT license (inlined in `index.html`).
