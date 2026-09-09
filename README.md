# Vending Conference 2026 – Event-Page

Statische Landingpage (eine einzige `index.html`, keine Build-Schritte).

## Deploy auf Vercel

1. Repo auf GitHub pushen (`git push`).
2. Auf vercel.com → **Add New Project** → Repo auswählen.
3. Framework Preset: **Other**. Build Command und Output Directory leer lassen.
4. **Deploy** klicken – fertig.

Alternativ per CLI: `npx vercel --prod` im Projektordner.

## Struktur

- `index.html` – die komplette Seite (CSS + JS inline, Bilder als Base64)
- `vercel.json` – Clean URLs + Security-Header
- `_source/` – ursprünglicher TextEdit-Export (wird nicht deployt)
