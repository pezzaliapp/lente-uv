# Lente UV

PWA di **analisi forense per PDF**: trascini un documento e scopri cosa nasconde.
Tutto avviene **nel browser** — il file non viene caricato da nessuna parte.

Controlla:
- **Coda del file** — byte aggiunti dopo l'ultimo `%%EOF` (nascondiglio classico).
- **Indicatori strutturali** — `/JavaScript`, `/Launch`, `/OpenAction`, `/EmbeddedFile`, form, URI…
- **Protezione e allegati** — se il PDF è cifrato/protetto, file incorporati, metadati.
- **Steganografia testuale** — caratteri a larghezza zero nel testo (lo stesso metodo di Inchiostro UV).
- **Stream sospetti** — blocchi ad alta entropia (possibile dato cifrato/compresso).

> **Rileva, non decifra.** Segnala la presenza di dati nascosti/cifrati; il contenuto cifrato
> resta illeggibile senza chiave. "Nessun indicatore" non è una prova di innocenza: stego nei
> pixel (LSB) o tecniche dedicate sfuggono a un controllo generico.

## File

```
index.html          l'app (usa pdf.js da CDN per estrarre il testo)
manifest.json       configurazione PWA
service-worker.js   cache offline (anche dei file pdf.js)
icons/              icone PWA
```

## Pubblicarla — come Inchiostro UV

### 1. GitHub Pages (link permanente)
Crea un repo vuoto su GitHub, poi dalla cartella `lente-app`:

```bash
git init
git add .
git commit -m "Lente UV — analisi forense PDF"
git branch -M main
git remote add origin https://github.com/TUO-UTENTE/lente-uv.git
git push -u origin main
```

Poi: **Settings → Pages → Deploy from a branch → main → / (root) → Save**.
Online su `https://TUO-UTENTE.github.io/lente-uv/`.

> Se preferisci il browser: nel repo vuoto clicca "uploading an existing file" e trascina
> il **contenuto** di `lente-app` (non la cartella), inclusa la sottocartella `icons`.

### 2. Netlify Drop (link in 30 secondi)
https://app.netlify.com/drop → trascina la cartella `lente-app`.

### 3. Cloudflare Pages / Vercel
Collega il repo, build vuota, output `/` (root).

## Note tecniche
- L'estrazione del testo usa **pdf.js** caricato da `cdnjs`. Il primo caricamento richiede
  rete; il service worker poi lo mette in cache per l'uso offline.
- Richiede **HTTPS** per installazione e offline (Pages/Netlify/Cloudflare/Vercel lo danno).
  In locale: `python3 -m http.server` su `http://localhost`.
- I PDF cifrati non permettono l'estrazione del testo (lo strumento lo segnala): il controllo
  steganografia sul testo non si applica, ma gli altri controlli (struttura, coda, entropia) sì.
