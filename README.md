# Odťahovka Bratislava – web

Statický web (jeden `index.html` + priečinok `assets/`), bez databázy a bez PHP. Hostuje sa zadarmo na **GitHub Pages**.

## Čo je v priečinku

| Súbor | Na čo slúži |
|---|---|
| `index.html` | celý web: texty, štýly, vyhľadávač polohy s mapou, kalkulačka ceny, formulár |
| `404.html` | stránka „nenašlo sa“ s tlačidlom Zavolať |
| `assets/` | Berno (SVG), logo, ikony, obrázok pre zdieľanie na Facebooku (`og-image.png`) |
| `site.webmanifest` | ikona a názov pri pridaní webu na plochu telefónu |
| `robots.txt`, `sitemap.xml` | pre Google |
| `.nojekyll` | hovorí GitHubu, aby súbory publikoval tak, ako sú |

## 1. Nahratie na GitHub Pages (15 minút, zadarmo)

1. Založ si účet na https://github.com (ak nemáš). Prihlás sa.
2. Vpravo hore **+** → **New repository**. Názov napr. `odtahovka-bratislava`, **Public**, nič iné neoznačuj, **Create repository**.
3. Na stránke nového repozitára klikni **uploading an existing file**. Pretiahni do okna **obsah tohto priečinka** (súbory `index.html`, `404.html`, `robots.txt`, `sitemap.xml`, `site.webmanifest`, `.nojekyll` a celý priečinok `assets`). Dole klikni **Commit changes**.
   - Alternatíva cez terminál (v tomto priečinku):
     ```
     git remote add origin https://github.com/TVOJ-UCET/odtahovka-bratislava.git
     git push -u origin main
     ```
4. V repozitári: **Settings** → vľavo **Pages** → *Build and deployment* → Source: **Deploy from a branch** → Branch: **main**, priečinok **/ (root)** → **Save**.
5. Do 1–2 minút je web na adrese `https://TVOJ-UCET.github.io/odtahovka-bratislava/`. Otvor ju na mobile a skús **Zistiť moju polohu** – prehliadač sa opýta na povolenie polohy (funguje len cez https, čo GitHub Pages je).

Každá ďalšia zmena = nahrať nový `index.html` cez **Add file → Upload files** (prepíše starý) alebo `git push`. Web sa obnoví do minúty.

## 2. Vlastná doména (odtahovkabratislava24.sk)

1. Kúp doménu (Websupport.sk alebo Active24.sk, cca 15 €/rok) na PRAKTICOM, s. r. o.
2. V repozitári **Settings → Pages → Custom domain** napíš `odtahovkabratislava24.sk` → **Save**. GitHub vytvorí súbor `CNAME`.
3. U registrátora domény nastav DNS záznamy:
   - `A` záznamy pre `@` (koreň domény): `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `CNAME` záznam pre `www` → `TVOJ-UCET.github.io`
4. Po 10 minútach až pár hodinách sa v Settings → Pages zobrazí zelená fajka; zaškrtni **Enforce HTTPS**.

## 3. Čo zmeniť pred spustením (hľadaj v `index.html`)

| Čo | Kde |
|---|---|
| **Telefónne číslo** | blok `KONTAKT` na konci súboru (`telefonZobrazeny`, `telefonMedzinarodny`, `whatsapp`) + `"telephone"` v JSON-LD v `<head>` + `<meta name="description">` |
| **E-mail** | `KONTAKT.email` + `"email"` v JSON-LD |
| **Doména** | `<link rel="canonical">`, `og:url`, `og:image`, `"url"`, `"image"`, `"logo"` v JSON-LD; `robots.txt`; `sitemap.xml` |
| **Ceny** | objekt `CENY` v skripte a tabuľka `<table class="pricelist">` (rovnaké čísla na oboch miestach) |
| **Výjazdové miesto** (odkiaľ sa počíta dojazd) | objekt `BASE` v skripte (teraz Priemyselná 11, Bernolákovo) |
| **Fotky auta** | pošli mi ich, doplním sekciu Galéria |

Tip: v textovom editore (TextEdit v režime „obyčajný text“, VS Code) použi Nájsť a nahradiť pre `0900 000 000`, `+421900000000`, `421900000000` a `odtahovkabratislava24.sk`.

## Technické poznámky

- Vyhľadávač polohy používa GPS telefónu (`navigator.geolocation`), adresy a podkladovú mapu z **OpenStreetMap** (Nominatim, Leaflet 1.9.4 z cdnjs). Adresy sa vyhľadávajú až po stlačení Enter / klepnutí na „Vyhľadať adresu“, nie pri každom písmene (pravidlá Nominatim).
- Dojazd a cena sa počítajú v prehliadači zo vzdušnej vzdialenosti × 1,28 (odhad cesty); nič sa nikam neposiela, kým zákazník sám nestlačí „Poslať polohu na WhatsApp“.
- Formulár nemá server: správu odovzdá do WhatsAppu alebo e‑mailu.
