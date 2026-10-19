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
  Test: `/danke?product=VIP-Ticket&payer_first_name=Lukas&payer_email=l@firma.de&order_id=12345&price=399&state=successful&test=1`
  (`&test=1` verhindert, dass der Test als echter Kauf an Meta geht, siehe unten)
- `vercel.json` – Clean URLs + Security-Header
- `_source/` – ursprünglicher TextEdit-Export (wird nicht deployt)

## Tracking (Meta Pixel `1544241537270144`)

| Seite | Event | Daten |
|---|---|---|
| `index.html` | `PageView` | – |
| `index.html` | `InitiateCheckout` (Klick auf „Ticket sichern“) | Ticket, Preis netto, UTMs |
| `danke.html` | `PageView` | Advanced Matching: E-Mail, Vor- und Nachname aus Ablefy (vom Pixel gehasht) |
| `danke.html` | `Purchase` | `value` (Ablefy-`price`), `currency`, `content_ids` (`vs26-basic` / `vs26-vip`), `num_items`, `contents`, `order_id`, `payment_state`, `coupon`, UTMs |

- **UTMs:** Der Checkout läuft über Ablefy, darum merkt sich die Landingpage die `utm_*`-Parameter
  90 Tage in `localStorage` (`vs_attr`). Mit dem Purchase gehen die UTMs des letzten Besuchs
  (`utm_source` … `utm_id`, `landing_page`) und vom Erstkontakt `first_utm_source`, `first_utm_medium`,
  `first_utm_campaign`, `first_referrer` mit. In Meta als eigene Conversions oder im Events Manager auswertbar.
- **Purchase feuert** nur mit Bestelldaten in der URL („Parameter übergeben“ in Ablefy muss an sein),
  nicht bei abgebrochener oder fehlgeschlagener Zahlung und pro Bestellung nur einmal (Reload zählt nicht).
  `eventID` = `purchase_<order_id>` für eine spätere Deduplizierung mit der Conversions API.
- **Test:** `&test=1` an die Danke-URL hängen, dann landet das Purchase-Event nur in der Browser-Konsole.
- In Ablefy **nicht zusätzlich** einen Meta-Pixel mit Purchase-Event hinterlegen, sonst zählt jeder Kauf doppelt.
