# Hamrevann – handover (sist oppdatert 2026-05-04)

Status og åpne punkter for neste sesjon.

## Status

- Landingsside er live på **fo.hamrevann.no** (hostet i HighLevel som Custom HTML)
- Kildekode: `index.html` (én fil, ingen build-steg)
- GitHub: https://github.com/btannum/hamrevann (branch `main`)
- Deploy-flyt: rediger lokalt → push til GitHub → Bendik kopierer fra raw-URL inn i HL

### Hovedlenker
- Raw HTML for HL-paste: https://raw.githubusercontent.com/btannum/hamrevann/main/index.html
- HL-skjema: form-id `BeBP3aJvOFUJBWAmoVAt` (returnerer prospekt på e-post etter innsending)
- Finn-annonse: finnkode 432186722
- Lokal preview: `python3 -m http.server 8080` → http://localhost:8080

### Eksterne assets (filesafe.space CDN — ikke i repo)
- Hero/galleri/logoer: `assets.cdn.filesafe.space/dm0qyruOPEayjpJ0sC24/media/...`
- Olav-foto: `https://assets.cdn.filesafe.space/dm0qyruOPEayjpJ0sC24/media/69f87b8ed63b4b9e6f4053f7.jpeg`
- Sørlandssenteret-kart: `https://assets.cdn.filesafe.space/dm0qyruOPEayjpJ0sC24/media/69f87be2704b04a49a002ad2.png`

### Lokale assets (i `assets/`, ikke deployet)
- `Tun2_Salgsoppgave_Hamrevann.pdf` — prospektet (sendes via HL-skjema, ikke lenket på siden)
- `sorlandssenteret-kart.png` — backup, deployet versjon bruker filesafe-URL

## Siste endringer (2026-05-04)
1. Ny ingress i hero: 280 boliger / 16 rekkehus / 3 solgt / fra 5,5 MNOK / innflytting høst 2026
2. CTA-er stablet vertikalt: grønn «Meld interesse og last ned prospekt» + outline «Besøk oss på Sørlandssenteret»
3. Ny seksjon `#besok` med kart over Sørlandssenteret (2. etasje ved H&M)
4. Meglerne Tor Even Kristensen og Audun Remesvik fjernet → erstattet med Olav Jarle Dyngeland (olav@hamrevann.no, 459 64 105) som hovedkontakt
5. Finn-lenken flyttet fra skjemaseksjonen til mørk footer (for å hindre «lekkasje»)
6. HL-skjema-iframe oppdatert til ny høyde 708px (skjemaet har nå flere felt og leverer prospekt på e-post)
7. Mobil-fiks: hero-content `width: 100%`, redusert padding (24→16px) og tittel-min (36→30px) på <520px

## Åpne punkter / TODO

- [ ] **Eiendomsmegler Norge-logo**: Står fortsatt i partner-strip («Megler»). Bendik må bekrefte om den skal vekk siden meglerne ikke lenger er hovedkontakt — eller om den blir av formelle grunner (de er nok fortsatt formell selger).
- [ ] **3 solgt vs. 2 solgt**: Hero-ingress sier «3 er solgt», men status-bar under hero sier ingenting om antall. Sjekk at tall holdes konsistent ved oppdateringer.
- [ ] **«Rekkehus» vs «tomannsbolig»**: Hero-ingress sier «16 rekkehus», hero-tag og specs-seksjonen sier «tomannsboliger». Avklare om begge er greie eller om vi skal harmonisere.
- [ ] **Innflytting høst 2026**: Ny info i ingress, men status-bar sier bare «Bygging starter snart». Vurder å oppdatere status-bar.

## Gotchas

- **HL custom HTML**: Bendik limer hele `index.html` inn i HL Custom Code-element. Inkluderer både CSS og JS. `form_embed.js` lastes via `<script src>` direkte (ikke lazy lenger – hadde lazy-load tidligere som ble fjernet i HL-versjonen for enklere paste).
- **Bilder må ligge på CDN**: HL Media Library funker, men siden vi allerede har filesafe.space oppe, brukes den for alle bilder. Lokale paths som `assets/...` virker ikke i deployet versjon.
- **iframe-høyde 708px**: Hardkodet basert på nåværende skjema-felt. Hvis skjema endres må både `data-height` og `style="height:..."` oppdateres.
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
