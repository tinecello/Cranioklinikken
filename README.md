# Cranioklinikken Oslo — nettsted for GitHub + Netlify

Statiske filer, ingen byggesteg. Legg innholdet i `site/` i repo-roten (eller sett
Netlify «Publish directory» til `site`).

```
index.html      forside
blogg.html      bloggoversikt
404.html        feilside (Netlify bruker den automatisk)
robots.txt
sitemap.xml
netlify.toml    redirects, sikkerhetsheadere, cache
assets/         bilder, video, favicon  ← MÅ FYLLES, se under
```

---

## 1. Filer du må legge i `assets/` før publisering

> **Merk:** mediefilene følger ikke med i denne pakken — de ligger allerede
> i `assets/` i repoet. All HTML peker på `/assets/…`, så det eneste du
> gjør er å legge disse filene inn ved siden av den eksisterende
> `assets/`-mappen — **ikke slett eller overskriv den**.
>
> Stemmer ikke filnavnene under med det som faktisk ligger i repoet, si fra
> med navnene, så retter jeg dem i koden.

| Fil | Hva | Merknad |
|---|---|---|
| `logo-symbol.png` | logoen | finnes i dagens utkast |
| `klinikk-atmosfaere.mp4` | stående klinikkvideo | komprimer til < 4 MB, H.264 |
| `klinikk-poster.jpg` | første bilde av klinikkvideoen | 880×1173 |
| `hilsen-kjetil.mp4` | Kjetils hilsen, liggende | |
| `kjetil-poster.jpg` | posterbilde | 1280×720 |
| `hilsen-kjetil.vtt` | **undertekster** til Kjetil-videoen | se punkt 4 |
| `behandling.jpg` | bilde i Metoden-seksjonen | 880×1056 |
| `avslutning.jpg` | bakgrunn nederst | 1600×900 |
| `blogg-*.jpg` | bloggbilder (hentet fra Wix, ligger i pakken) | 1600 px bred |
| `og-bilde.jpg` | delingsbilde | **1200×630**, tekstfri |
| `favicon.svg` + `apple-touch-icon.png` | ikoner | 180×180 for apple |

Lagre foto som WebP i tillegg hvis mulig — halverer vekten.
**Ikke** hotlink bilder fra Wix; last dem ned og legg dem her.

---

## 2. Søk og opp: dette er gjort

- Én `<h1>` per side, deretter `h2`/`h3` i riktig rekkefølge
- `lang="nb"`, `title` og `meta description` unike per side (55/155 tegn)
- `rel=canonical` på begge sider
- Open Graph + Twitter-kort for deling
- **Strukturerte data (JSON-LD):** `MedicalBusiness` med adresse, priser og
  lenker til sosiale profiler; `FAQPage` på forsiden (gir utvidet visning i
  Google); `Blog` + `BreadcrumbList` på bloggsiden
- Brødsmulesti på bloggsiden, synlig og i schema
- `alt`-tekst på alle meningsbærende bilder, tom `alt` på dekorbilder
- `width`/`height` på bilder og video (unngår layouthopp = bedre CLS)
- `loading="lazy"` på alt under første skjermbilde
- `preconnect` til fontleverandøren, `display=swap` på fontene
- Semantisk `header`/`main`/`section`/`footer`/`nav` med `aria-label`
- «Hopp til innhold»-lenke, `aria-expanded` på menyknappen, 44 px trykkflater
- `prefers-reduced-motion` respekteres
- robots.txt + sitemap.xml
- Sikkerhetsheadere og ett års cache på assets via `netlify.toml`
- apex → www-omdirigering (301) så Google ser én kanonisk adresse
- `WebSite`-node som binder sidene sammen, og `isPartOf` på FAQ-en
- `MedicalBusiness` utvidet med e-post, kart, koordinater,
  åpningstider, betalingsmåter og `MedicalTherapy` som tjeneste
- `preconnect` og `preload` av hovedbildet (bedre LCP)
- Pene URL-er: /blogg, og /index.html → /

---

## 3. Dette MÅ du gjøre selv

1. **Bytt domenet.** Søk og erstatt `https://www.cranio.no` i `index.html`,
   `blogg.html`, `sitemap.xml`, `robots.txt` og `netlify.toml` hvis adressen blir
   en annen.
2. **Kontaktinfo — bekreftet.** Klinikken har ikke felles telefonnummer, så
   `telephone` er tatt ut av JSON-LD; praktiske spørsmål går direkte til
   terapeuten. E-post `post@cranio.no` ligger i JSON-LD og synlig i footeren
   på alle sider. Sørg for at Google Business Profile viser det samme.
3. **Åpningstider.** Tidene varierer; kjernetid man–fre 09:30–20:30 er lagt inn
   i JSON-LD. Bruk samme kjernetid i Google Business Profile.

4. **Undertekster** til Kjetil-videoen (`hilsen-kjetil.vtt`). Uten dem faller
   `<track>`-taggen bare bort, men de gir både tilgjengelighet og indeksering
   av det han faktisk sier.
5. **Bloggartiklene.** De fire publiserte Wix-innleggene ligger nå som egne sider
   under `/blogg/` med `BlogPosting`-schema, `datePublished`, forfatter og
   lokale bilder, og er lagt inn i `sitemap.xml`. Nye innlegg lages etter samme mal.
   Vurder 301-redirect fra de gamle `/post/…`-URL-ene når Wix kobles av.
6. **Google Business Profile.** Viktigere enn nettsiden for «kraniosakralterapi
   Oslo»-søk. Sørg for at navn, adresse og åpningstider er identiske med siden.
7. **Google Search Console + Bing Webmaster Tools:** meld inn sitemap.
8. **Analyse.** Ingen sporingskode er lagt inn (ingen cookie-banner nødvendig
   ennå). Legger du på Google Analytics, trenger du et samtykkebanner etter
   norsk rett.
9. **Vurder `AggregateRating` i JSON-LD** når du har antall Google-anmeldelser.
   Google krever både snitt og antall — jeg utelot det framfor å gjette.

---

## 4. Etter publisering

Kjør Lighthouse i Chrome (Ytelse / Tilgjengelighet / SEO) og
[Rich Results Test](https://search.google.com/test/rich-results) på begge sider.
Den største gjenstående ytelsesposten er videofilene — hold dem under 4 MB hver.

---

## Designfiler

`Cranioklinikken Forside.dc.html` og `Cranioklinikken Skrifter.dc.html` i
prosjektroten er arbeidsfilene for design. `site/`-mappen er byggeversjonen som
publiseres. Endres designet, må endringen speiles i `site/`.
