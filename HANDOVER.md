# Hamrevann – handover (sist oppdatert 2026-05-04)

Status og åpne punkter for neste sesjon.

## Status

- Landingsside er live på **fo.hamrevann.no** (hostet i HighLevel som Custom HTML)
- Kildekode: `index.html` (én fil, ingen build-steg)
- GitHub: https://github.com/btannum/hamrevann (branch `main`)
- Deploy-flyt: rediger lokalt → push til GitHub → Bendik kopierer fra raw-URL inn i HL Custom Code-elementet

### Hovedlenker
- Raw HTML for HL-paste: https://raw.githubusercontent.com/btannum/hamrevann/main/index.html
- HL-skjema: form-id `BeBP3aJvOFUJBWAmoVAt` (returnerer prospekt på e-post etter innsending)
- YouTube-video i film-seksjon: `HzZXGZ_lvQw`
- Finn-annonse: finnkode 432186722
- Lokal preview: `python3 -m http.server 8080` → http://localhost:8080

### Eksterne assets (filesafe.space CDN — ikke i repo)
- Hero/galleri/logoer: `assets.cdn.filesafe.space/dm0qyruOPEayjpJ0sC24/media/...`
- Olav-foto: `https://assets.cdn.filesafe.space/dm0qyruOPEayjpJ0sC24/media/69f87b8ed63b4b9e6f4053f7.jpeg`
- Sørlandssenteret-kart: `https://assets.cdn.filesafe.space/dm0qyruOPEayjpJ0sC24/media/69f87be2704b04a49a002ad2.png`

### Lokale assets (i `assets/`, ikke deployet)
- `Tun2_Salgsoppgave_Hamrevann.pdf` — prospektet (sendes via HL-skjema, ikke lenket på siden)
- `sorlandssenteret-kart.png` — backup, deployet versjon bruker filesafe-URL

## Sideoppbygging (rekkefølge)

1. **Hero** — bilde + tag + tittel + ingress + 2 stablede CTA-er
2. **Status-bar** — «Innflytting høst 2026», 1 linje også på mobil
3. **Film-seksjon** (`#film`) — autoplay YouTube med custom kontroller (se gotchas)
4. **Fordeler** — 3 kort
5. **Galleri** — 6 bilder, lightbox
6. **Beliggenhet** — avstandsliste
7. **Spesifikasjoner** — 10 spec-celler
8. **Besøk Sørlandssenteret** — kart + Olav
9. **Skjema** — HL-iframe (lazy-loadet)
10. **Partnere** — Konsmo + Eiendomsmegler Norge logoer (ingen lenker)
11. **Olav-kort** — kontaktperson
12. **Finn-footer**
13. **Sticky CTA på mobil**

## Siste endringer (2026-05-04, denne sesjonen)

1. **Status-bar**: «Bygging starter snart» → «Innflytting høst 2026», `clamp` font-size + `nowrap` så teksten holder seg på 1 linje på mobil
2. **Eiendomsmegler Norge**: lenke til nettsiden fjernet (logo står)
3. **Konsmo Fabrikker**: lenke til nettsiden fjernet (logo står)
4. **Ny film-seksjon (`#film`)** plassert mellom status-bar og fordeler:
   - Mørk grønn-svart bakgrunn med subtil radial-glow
   - 16:9-ramme, max-width 1180px, drop-shadow + tynn lys-border
   - YouTube-iframe på `youtube-nocookie.com` med `enablejsapi=1`
   - URL-params: `autoplay=1&mute=1&controls=0&loop=1&playlist=ID&modestbranding=1&rel=0&playsinline=1&iv_load_policy=3&disablekb=1&fs=0`
   - Iframe `pointer-events: none` + transparent shield over for klikk-håndtering
   - **Klikk på video** = play/pause via YT iframe API postMessage
   - **Lyd-knapp** (bunn-høyre): toggler mute/unMute via postMessage
   - **Fullskjerm-knapp** (ved siden av lyd): native Fullscreen API på `.film-frame`-elementet (åpner ikke YouTube), CSS-fallback `position: fixed; inset: 0` for eldre iOS
   - Begge knapper alltid synlige (z-index 6) på både mobil og PC

## Åpne punkter / TODO

- [ ] **3 solgt-tall**: Hero-ingress sier «3 er solgt». Hold konsistent når flere selges.
- [ ] **«Rekkehus» vs «tomannsbolig»**: Hero-ingress sier «16 rekkehus», hero-tag og specs-seksjonen sier «tomannsboliger». Avklare om begge er greie eller om vi skal harmonisere.
- [ ] **Eiendomsmegler Norge-logo**: Står fortsatt i partner-strip («Megler»). Bekreft om den blir av formelle grunner eller skal fjernes.
- [ ] **YouTube postMessage-events**: Listener håndterer både `onStateChange` og `infoDelivery` — hvis play/pause-toggle oppfører seg rart, sjekk om YT har endret iframe-API-formatet.

## Gotchas

- **HL custom HTML**: Bendik limer hele `index.html` inn i HL Custom Code-element. Inkluderer både CSS og JS.
- **Bilder må ligge på CDN**: HL Media Library funker, men siden vi allerede har filesafe.space oppe, brukes den for alle bilder. Lokale paths som `assets/...` virker ikke i deployet versjon.
- **HL-iframe-høyde 708px**: Hardkodet basert på nåværende skjema-felt. Hvis skjema endres må både `data-height` og `style="height:..."` oppdateres.
- **Film-seksjon — YT iframe API**:
  - `enablejsapi=1` MÅ være med i embed-URL for at postMessage skal fungere
  - Etter iframe `load`-event sender vi `{event: "listening", id: "filmVideo", channel: "widget"}` — uten `id+channel` får vi ingen state-events tilbake
  - Vi håndterer både `onStateChange` og `infoDelivery` events fordi YT bruker begge
  - Lyd starter alltid muted (browser autoplay-policy) — brukeren må klikke lyd-knapp for å skru på
  - `pointer-events: none` på iframe — alle klikk fanges av shield/buttons over
  - Custom fullscreen kaller `requestFullscreen()` på `.film-frame`-wrapperen, ikke iframen → går aldri til youtube.com
- **Ingen build/lint**: én HTML-fil, rediger og push.

## Kommandoer

```bash
# Lokal preview
cd ~/Prosjekter/hamrevann && python3 -m http.server 8080

# Push endring
git add index.html && git commit -m "..." && git push origin main

# Hent raw URL etter push
echo "https://raw.githubusercontent.com/btannum/hamrevann/main/index.html"
```
