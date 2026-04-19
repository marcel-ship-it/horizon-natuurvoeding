# Sprint Briefing — Sky Animation & Design Review
**Bestand:** `index.html`  
**Datum:** april 2026  
**Voor:** Claude Code

---

## Context

De hero-sectie van `index.html` heeft een dag/nacht sky-systeem:
- Een `<canvas id="heroOrbCanvas">` tekent een zon of maan als radiale glow
- Een `.hero-sky` div krijgt een JS-gestuurde sky-gradient op basis van het werkelijke uur
- Een `<button id="skyToggle">` laat de gebruiker dag/nacht handmatig wisselen
- Bij nacht wordt de CSS-class `dark-sky` op `.hero` gezet

Er zijn **5 problemen** die opgelost moeten worden, gegroepeerd in sprints.

---

## Sprint 1 — Zon & Maan: gloed sterker en onderscheidend

**Concept:** De orb is en blijft een *abstracte atmosferische gloed* — geen zichtbare schijf. Het is subtiel: je voelt dat het dag of nacht is, maar er is geen letterlijk rondje op het scherm. Overdag kleurt de hemel warm vanuit een punt, 's avonds/nachts vanuit een koeler punt. De achtergrond (sky-gradient) versterkt dit — blauw/warm bij dag, deep navy bij nacht.

**Probleem met de huidige implementatie:** De gloed is zo groot (`r = 52%` van de canvas) en zo transparant (max opacity 0.72 voor zon, 0.45 voor maan) dat hij visueel wegvalt. Gebruikers zien geen verschil tussen dag en nacht in de orb zelf.

**Wat er moet veranderen:**
- De gloed-straal verkleinen van 52% → 28% van de kleinste dimensie (scherper, meer gelokaliseerd)
- De opaciteiten verhogen: zon kern van 0.72 → 0.92, maan kern van 0.45 → 0.72
- Zon: warm geel-wit centrum (`rgba(255,248,210,...)`) dat uitwaaiert naar transparant
- Maan: koeler blauw-wit centrum (`rgba(210,228,255,...)`) — duidelijk anders dan de zon

**Exacte code-instructie:**

Zoek de functie `drawOrb(t, isDark)` in het `<script>` blok (rond regel 1233).

Vervang de hele functie-body door:

```javascript
function drawOrb(t, isDark) {
  const canvas = document.getElementById('heroOrbCanvas');
  if (!canvas) return;
  const w = heroSky.offsetWidth  || window.innerWidth;
  const h = heroSky.offsetHeight || window.innerHeight * 0.68;
  if (canvas.width  !== w) canvas.width  = w;
  if (canvas.height !== h) canvas.height = h;
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0, 0, w, h);

  const leftPct = 8  + 84 * t;
  const topPct  = 88 - 76 * Math.sin(Math.PI * t);
  const cx = w * (leftPct / 100);
  const cy = h * (topPct  / 100);

  // Gloed-straal: 28% van de kleinste dimensie (was 52% — te diffuus)
  const r = Math.min(w, h) * 0.28;

  const grd = ctx.createRadialGradient(cx, cy, 0, cx, cy, r);

  if (isDark) {
    // Maan — koeler blauw-wit, duidelijk anders dan de zon
    grd.addColorStop(0,    'rgba(210, 228, 255, 0.72)'); // was 0.45 — nu zichtbaar
    grd.addColorStop(0.18, 'rgba(190, 215, 248, 0.38)');
    grd.addColorStop(0.45, 'rgba(170, 200, 240, 0.12)');
    grd.addColorStop(1,    'rgba(0, 0, 0, 0)');
  } else {
    // Zon — warm geel-wit
    grd.addColorStop(0,    'rgba(255, 248, 210, 0.92)'); // was 0.72 — nu overtuigend
    grd.addColorStop(0.15, 'rgba(250, 230, 170, 0.52)');
    grd.addColorStop(0.38, 'rgba(245, 215, 130, 0.18)');
    grd.addColorStop(1,    'rgba(0, 0, 0, 0)');
  }

  ctx.fillStyle = grd;
  ctx.fillRect(0, 0, w, h);
}
```

**Verifieer na implementatie:** Overdag zie je een warme gloed die over de hemelblauwe achtergrond beweegt. 's Nachts zie je een koelere, blauwige gloed op de navy achtergrond. Geen harde schijf — puur sfeer. Het verschil tussen dag en nacht moet direct duidelijk zijn.

---

## Sprint 2 — Sky gradients: gebruik officiële Horizon brandkleuren

**Probleem:** De sky-gradients zijn technisch correct maar niet brand-aligned. Overdag-kleuren zijn willekeurig blauwig; de brandbook schrijft Horizon Blauw `#009cdc` voor als primaire kleur. De nacht-gradient is goed (deep navy), maar de overgangsmoment-kleuren (dawn, dusk, evening) hangen niet samen met het officiële brand-palet.

**Officiële brandkleuren (design.md):**
- Horizon Blauw: `#009cdc`
- Hemelsblauw (gradient middentoon): `#b8e8f5`
- Crème (aarde/horizon): `#fff0d8`
- Nacht: behoud huidige deep navy `#050e1f` → `#0f2548`

**Wat er moet veranderen:**

Zoek het `const skies = { ... }` object (rond regel 1183).

Vervang het gehele object door:

```javascript
const skies = {
  night:     'linear-gradient(180deg, #050e1f 0%, #071528 25%, #091c38 50%, #0c2040 70%, #0f2548 100%)',
  dawn:      'linear-gradient(180deg, #0c2040 0%, #1a4a7a 20%, #2e7ab0 38%, #5aacd4 55%, #90d0ea 72%, #c8ecf8 88%, #e8f6fc 100%)',
  morning:   'linear-gradient(180deg, #0078b8 0%, #009cdc 30%, #3db8e8 52%, #80d4f0 70%, #b8e8f5 84%, #e4f5fc 100%)',
  midday:    'linear-gradient(180deg, #0070a8 0%, #009cdc 28%, #3ab6e4 50%, #7ed0f0 68%, #b8e8f5 84%, #e8f8fc 100%)',
  afternoon: 'linear-gradient(180deg, #006da0 0%, #0092d0 28%, #38b2e0 50%, #7cccee 68%, #b4e6f4 84%, #e4f6fc 100%)',
  dusk:      'linear-gradient(180deg, #082040 0%, #14366a 18%, #2458a0 34%, #4a84c4 50%, #7ab8e0 66%, #aadaf2 80%, #d4eef8 100%)',
  evening:   'linear-gradient(180deg, #050e1f 0%, #081428 25%, #0c1c3a 50%, #101e3c 70%, #122040 100%)',
};
```

**Verifieer na implementatie:** Overdag moet de sky de karakteristieke Horizon-blauwtint hebben (`#009cdc`). De overgang van sky naar het crème hero-earth moet visueel de "horizon-lijn" suggereren die centraal staat in de Horizon-merkidentiteit.

---

## Sprint 3 — Nachtmodus: lucht donker, grond blijft crème

**Concept:** De aarde verandert nooit. Alleen de lucht wordt donker. Dit is de kern van de Horizon-metafoor: de hemel boven de horizonlijn is dag of nacht, maar de aarde eronder blijft altijd de vertrouwde crème van Horizon Natuurvoeding. De harde horizonlijn tussen donkere lucht en lichte aarde is juist het krachtigste visuele moment.

**Probleem:** De huidige CSS heeft twee foute overrides:
```css
.hero.dark-sky .hero-earth { background: #ede3cc; } /* grond wordt iets donkerder — niet doen */
.hero.dark-sky .hero-missie { color: var(--color-navy); } /* tekst wordt navy — maar op crème is dit ok */
```

De `hero-earth` achtergrond mag bij nacht **nooit** veranderen. Die blijft `#fff0d8`.

Het enige wat wél mag veranderen zijn kleine details die anders slecht leesbaar worden op de crème achtergrond in combinatie met de donkere sky (bijv. als een knop een witte rand heeft die nu wegvalt).

**Wat er moet veranderen:**

Zoek het CSS-blok `/* DARK SKY overrides */` (rond regel 404). Vervang het hele block door:

```css
/* DARK SKY overrides — alleen de lucht verandert, de aarde blijft crème */

/* hero-earth achtergrond: NIET aanpassen — blijft #fff0d8 */

/* Tekst op de crème aarde: navy is prima leesbaar, geen aanpassing nodig */

/* Scroll-indicator: iets donkerder zodat hij zichtbaar blijft tegen de donkere sky */
.hero.dark-sky .scroll-indicator {
  border-color: rgba(0, 156, 220, 0.6);
  color: rgba(0, 156, 220, 0.7);
}
```

**Let op:** Controleer na implementatie of er andere elementen in `.hero-earth` zijn die bij nacht problemen geven (bijv. knoppen met witte achtergrond die wegvallen op crème). Fix alleen wat visueel echt kapot is — niet preventief alles omgooien.

**Verifieer na implementatie:** Toggle naar nacht. De `.hero-sky` (lucht) is deep navy. De `.hero-earth` (grond) blijft exact dezelfde crème `#fff0d8` als overdag. De horizonlijn tussen donkere lucht en lichte aarde is scherp en herkenbaar — dat is het merk.

---

## Sprint 4 — Toggle icoon: vervang tiny SVG line-art

**Probleem:** Het toggle-icoon is een 16×16 SVG (`<svg width="16" height="16" viewBox="0 0 24 24">`). De zon heeft 8 kleine lijntjes die samen een minuscuul spinnetje vormen. Bij 16×16 is dit nauwelijks zichtbaar en ziet er "gek" uit.

**Wat er moet veranderen:**

**Stap 1 — Maak de knop groter.**  
Zoek `.sky-toggle` in de CSS (rond regel 368):
```css
.sky-toggle { width: 36px; height: 36px; ... }
```
Verander naar:
```css
.sky-toggle { width: 44px; height: 44px; ... }
```

**Stap 2 — Vergroot het SVG-icoon.**  
Zoek in de HTML het `<svg id="skyToggleIcon"` element (rond regel 890):
```html
<svg id="skyToggleIcon" width="16" height="16" ...>
```
Verander naar:
```html
<svg id="skyToggleIcon" width="22" height="22" ...>
```

**Stap 3 — Vervang de SVG icon-paths in de JavaScript.**  
Zoek `const sunSVG` en `const moonSVG` (rond regel 1194).

Vervang door:

```javascript
// Zon — gevulde schijf met 8 stralen (filled, niet line-art)
const sunSVG = `
  <circle cx="12" cy="12" r="5" fill="rgba(255,235,130,0.95)" stroke="none"/>
  <g stroke="rgba(255,235,130,0.95)" stroke-width="2" stroke-linecap="round">
    <line x1="12" y1="1" x2="12" y2="4"/>
    <line x1="12" y1="20" x2="12" y2="23"/>
    <line x1="1" y1="12" x2="4" y2="12"/>
    <line x1="20" y1="12" x2="23" y2="12"/>
    <line x1="4.22" y1="4.22" x2="6.34" y2="6.34"/>
    <line x1="17.66" y1="17.66" x2="19.78" y2="19.78"/>
    <line x1="4.22" y1="19.78" x2="6.34" y2="17.66"/>
    <line x1="17.66" y1="6.34" x2="19.78" y2="4.22"/>
  </g>`;

// Maan — gevulde crescent (filled, niet outline)
const moonSVG = `
  <path d="M20 13.5A8 8 0 1 1 10.5 4a6 6 0 0 0 9.5 9.5z"
        fill="rgba(220,235,255,0.95)" stroke="none"/>`;
```

**Let op bij de sunSVG:** verwijder de `stroke` attribute die momenteel op het parent SVG element staat via JS (`icon.setAttribute('stroke', ...)`). De nieuwe iconen sturen hun eigen kleur.

Zoek in de `render()` functie (rond regel 1304):
```javascript
icon.setAttribute('stroke', 'rgba(255,255,255,0.85)');
```
**Verwijder deze regel.** De kleur zit nu in de SVG-paden zelf.

**Verifieer na implementatie:** De zon en maan iconen zijn duidelijk herkenbaar op 44×44px, gevuld (niet outline), en passen qua kleur bij de hemel-toestand.

---

## Sprint 5 — Full design review: frontend-design + ui-ux-pro-max pass

**Wat dit inhoudt:**  
Ga systematisch door de volledige `index.html` en toets elk onderdeel aan het Horizon brand system (`design.md`) én de frontend-design/ui-ux-pro-max regels. Dit is geen grote rebuild — het is een nauwkeurige correctie-pass.

Voer onderstaande checks uit en fix alles wat niet klopt:

### 5A — Typografie

- [ ] Alle `<h1>` en `<h2>` elementen: `font-family: 'Zilla Slab', 'Rockwell', serif; text-transform: uppercase; font-weight: 900;` (Clarendon fallback)
- [ ] Body tekst: `font-family: 'DM Sans', sans-serif; font-weight: 400; line-height: 1.6;`
- [ ] Subkoppen: `DM Sans Medium` (weight 500)
- [ ] Labels/badges/onderregels: `DM Sans Medium, letter-spacing: 0.12em, text-transform: uppercase`
- [ ] Geen cursief gebruik (decoratief cursief verwijderen als het er is)
- [ ] Minimum 16px body tekst op mobile

### 5B — Kleuren

Controleer elk kleurgebruik tegen de officiële palet:
- Horizon Blauw: `#009cdc` (niet `#00aeef`, niet `#0090cc`)
- Crème: `#fff0d8` (niet `#faf0dc`, niet `#f5ead0`)
- Groen: `#60d553` — **alleen** voor CTA-knoppen en badges, NOOIT als vlak
- Zwart: `#000000` voor body tekst
- Wit: `#ffffff` voor tekst op blauw

Verboden combinaties (brandbook p.50):
- Crème tekst op witte achtergrond → fix
- Witte tekst op crème achtergrond → fix
- Groene tekst op blauwe achtergrond → fix
- Zwarte tekst op groene achtergrond → fix

### 5C — Horizon-gradient

Het signature gradient-element heeft een **harde cut** (geen vloeiende overgang):
```css
background: linear-gradient(to bottom, #009cdc 0%, #b8e8f5 50%, #fff0d8 100%);
```
De hero-sectie heeft dit al (via `.hero-sky` + `.hero-earth`). Controleer of de "horizonlijn" — de harde overgang van hemelkleur naar crème — visueel zichtbaar en scherp is. Als die overgang te zacht is, voeg toe aan `.hero-earth`:
```css
box-shadow: 0 -2px 0 0 rgba(0, 156, 220, 0.15);
```

### 5D — CTAs

Controleer alle knopteksten en -stijlen:
- Primaire CTA-knoppen: achtergrond `#60d553`, tekst wit, `font: DM Sans Medium, uppercase, letter-spacing`
- Hover: lichtere variant van groen (bijv. `#72e066`)
- **Verboden CTA-teksten:** "Meer informatie", "Ontdek", "Lees meer", "Neem contact op"
- **Correcte CTAs:** "Bekijk het assortiment", "Zien wat we maken?", "Praat met ons", "Het hele verhaal"
- Minimum touch target: 44×44px

### 5E — Contrast & Accessibility

- Controleer alle tekst/achtergrond-combinaties: minimum ratio 4.5:1
- Alle `<img>` elementen moeten een beschrijvend `alt`-attribuut hebben
- De toggle-knop heeft al `aria-label="Wissel tussen dag en nacht"` — laat dit staan
- `prefers-reduced-motion` — als er CSS-animaties zijn die geen `@media (prefers-reduced-motion: reduce)` respekt, voeg toe

### 5F — Mobile responsiveness

Test visueel (of via resize) op:
- 375px (iPhone SE): geen horizontale scroll, geen overlappende elementen
- 768px (tablet): layout klopt, kolom-breedtes correct
- 1440px (desktop): max-width containers voorkomen dat tekst te breed wordt

Controleer specifiek:
- Hero-tekst: niet te klein op mobile (min 16px body, min 32px H1)
- B2B-sectie: tekst leesbaar op mobile
- Footer: logo niet te groot of te klein

### 5G — Tone of Voice in UI-tekst

Loop alle zichtbare tekst door. Verwijder of vervang:
- Hype-taal: "revolutionair", "speciaal", "beste kwaliteit", "duurzaamheid is onze prioriteit"
- Generieke taal: "Pure smaken", "Een bewuste keuze", "Van boer tot bord"
- Vervanging door directe, nuchter-Hollandse taal conform de schrijfwijzer in `design.md`

---

## Volgorde van uitvoering

Voer de sprints uit in deze volgorde:

1. **Sprint 1** (orb) — visueel grootste impact, meest zichtbaar
2. **Sprint 2** (sky gradients) — kleuren corrigeren geeft direct een betere achtergrond voor Sprint 1
3. **Sprint 3** (dark-sky overrides) — pas uitvoeren nadat Sky (Sprint 2) klopt
4. **Sprint 4** (toggle icoon) — kleine wijziging, snel gedaan
5. **Sprint 5** (design review) — alleen nadat 1-4 klaar zijn

Elke sprint is **zelfstandig uitvoerbaar** — als één sprint problemen geeft, kunnen de andere gewoon doorgaan.

---

## Controleer dit na alle sprints

- [ ] Laad de pagina overdag → zon beweegt over brand-blauwe hemel
- [ ] Laad de pagina 's nachts → maan beweegt over donkere navy hemel
- [ ] Klik toggle naar nacht → hele hero (sky + earth) wordt donker, maanschijf zichtbaar
- [ ] Klik toggle terug naar dag → sky en earth keren terug naar dag-kleuren
- [ ] Controleer mobile (375px) → geen layout-breuk
- [ ] Controleer toggle-icoon → zon en maan herkenbaar en goed gekleurd

---

*Bronnen: `design.md` (volledig Horizon design system), brandbook `20250415_HON_Natuurvoeding_Brandbook-compressed.pdf`*
