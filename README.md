# Vending Summit 2026 – Event-Page

Statische Landingpage (eine einzige `index.html`, keine Build-Schritte).

## Deploy auf Vercel

1. Repo auf GitHub pushen (`git push`).
2. Auf vercel.com → **Add New Project** → Repo auswählen.
3. Framework Preset: **Other**. Build Command und Output Directory leer lassen.
4. **Deploy** klicken – fertig.

Alternativ per CLI: `npx vercel --prod` im Projektordner.

## Struktur

- `index.html` – die komplette Seite (CSS + JS inline, Bilder als Base64)
- `danke.html` – Danke-Seite nach dem Ablefy-Checkout, erreichbar unter `/danke`.
  In Ablefy beim Produkt unter **Auslieferung → Danke-Seite / Kaufbestätigung** als eigene Website eintragen
  und **„Parameter übergeben“** aktivieren. Die Seite liest `payer_first_name`, `payer_email`, `order_id`,
  `product`, `price`, `state`, `coupon` usw. aus der URL und zeigt nur vorhandene Daten an.
  Test: `/danke?product=VIP-Ticket&payer_first_name=Lukas&payer_email=l@firma.de&order_id=12345&price=399&state=successful`
- `vercel.json` – Clean URLs + Security-Header
- `_source/` – ursprünglicher TextEdit-Export (wird nicht deployt)
