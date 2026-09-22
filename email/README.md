# Launch-Mail „Tickets sind live“ (Mailchimp)

Zwei Varianten, gleicher Inhalt, gleiche Links und Merge-Tags:

- `launch-announcement.html` – Variante 1, editorial und ruhig (Serif-Headlines, Hairlines, viel Weißraum). Ca. 34 KB.
- `launch-announcement-v2.html` – Variante 2 im Look der Landingpage: dunkler Hero mit Bühnenlicht und Speaker-Freistellern,
  Verlauf ins Weiße, Karten mit Fotos, Ticket-Grafiken, oranger Abschluss-Block. Ca. 46 KB.
- `img/` und `img/v2/` – optimierte Bilder (v1 ca. 230 KB, v2 ca. 510 KB). Werden über Vercel unter
  `https://summit.spaetimat.com/email/img/...` ausgeliefert. Die HTML-Dateien selbst werden nicht deployt (`.vercelignore`).

- `followup-2-story.html` bis `followup-5-deadline.html` – die vier Follow-up-Mails der Launch-Sequenz.
  Betreffzeilen, Preview-Texte und Versandplan stehen in `SEQUENZ.md`.

Hinweis zur v2: Headline und Speaker im Hero sind als Bild gebaut (`img/v2/hero.jpg`), damit der Look in allen Mail-Clients
identisch ist. Datum, Text, Buttons und alles darunter sind echter Text.

## In Mailchimp einspielen

1. `git push`, damit die Bilder unter der Vercel-URL erreichbar sind (kurz prüfen: `/email/img/hero.jpg` im Browser öffnen).
2. Mailchimp → Campaigns → Create → Email → Design: **Code your own → Paste in code**.
3. Kompletten Inhalt von `launch-announcement.html` einfügen. Die Merge-Tags
   (`*|FNAME|*`, `*|UNSUB|*`, `*|UPDATE_PROFILE|*`, `*|LIST:ADDRESSLINE|*`, `*|CURRENT_YEAR|*`) füllt Mailchimp automatisch.
4. Preview-Text in Mailchimp setzen (steht auch als Preheader in der Mail):
   „Sa, 28. November 2026 in Düpow. 250 Plätze, Early Bird nur für kurze Zeit: Basic 149 €, VIP 399 €.“
5. Test-Mail an Gmail, Apple Mail und Outlook schicken, dann senden.

## Deliverability-Checkliste

- Absender-Domain in Mailchimp authentifiziert (SPF + DKIM), Absender z. B. `jonas@spaetimat.com`, kein Gmail-Absender.
- Alle Links HTTPS, keine Kurz-URLs, UTM-Parameter sind bereits gesetzt (`utm_campaign=launch`).
- Guter Text-Bild-Anteil, jedes Bild hat Alt-Text, Abmelde-Link und Postadresse im Footer.
