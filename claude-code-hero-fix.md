# Hero Horizon Fix — Zelfcorrigerende Claude Code Prompt

## Context
Dit is de website van Horizon Natuurvoeding (`index.html`). De hero-sectie heeft een dag/nacht hemel met een zon/maan die langs de hemel beweegt op basis van de tijd van de gebruiker. De structuur klopt grotendeels. Er is één hardnekkig probleem dat opgelost moet worden.

## Het probleem
De horizonlijn in de hero snijdt NIET door de onderkant van de tekst "WE 'M WEL." zoals in het logo. In plaats daarvan begint de cream aarde pas ONDER de letters, waardoor de horizon op de baseline zit in plaats van erdoor.

## Referentie: het logo
Bekijk `assets/images/hornavologo.svg`. De SVG heeft:
- viewBox: `0 0 595.28 121.32` (totale hoogte = 121.32)
- Horizonlijn (`<line class="cls-2">`) op `y1="77.77"` en `y2="77.77"`
- Hoofdletters lopen van ~y=1.78 tot ~y=77.77 (letterhoogte = ~76 units)
- De lijn zit op de **baseline** van de hoofdletters
- Onder de lijn (y=77.77 tot y=121.32) staat de tagline in kleine letters

**De lijn raakt de onderkant van de letters. De serifs steken er net onderdoor.** Dat is het horizon-effect: de typografie "staat" op de horizon, met de voeten net in de grond.

## Brandboek kleuren (uit `20250415_HON_Brandbook.pdf` en CSS):
- `--color-blue: #00AEEF` — Horizon blauw (lucht)
- `--color-cream: #FAF0DC` — Cream (aarde, letters)
- `--color-navy: #003A5C` — Navy (donker blauw)
- `--color-sky: #B8E8F5` — Lichtblauw

## Gewenste eindresultaat
1. **Lucht**: blauwe gradient (`#1a7ab5` → `#6ec6e8` → lichtblauw aan horizon)
2. **Tekst**: ALLE drie de regels (SAMEN, BEREIKEN, WE 'M WEL.) zijn `color: #FAF0DC` (cream)
3. **Aarde**: `background: #FAF0DC` (cream) — dezelfde kleur als de letters
4. **Horizonlijn**: de overgang tussen blauwe lucht en cream aarde loopt DOOR de onderkant van "WE 'M WEL." — de serifs van de letters verdwijnen in de aarde. De aarde begint dus al terwijl de letters nog lopen.
5. **Effect**: omdat de letters cream zijn en de aarde cream is, "smelten" de voeten van de letters samen met de aarde. Alleen het bovenste deel van de letters is zichtbaar tegen de blauwe lucht. Dit is exact het logo-effect vertaald naar groot formaat.
6. **Dag/nacht**: de sky-achtergrond verandert al correct via JavaScript op basis van `new Date().getHours()`. Dat hoeft NIET aangepast te worden.
7. **CTA knoppen**: op de cream aarde, primaire knop is `#00AEEF` (Horizon blauw) met witte tekst. Ghost knop is navy.

## Technische aanpak
De hero bestaat uit twee flex-children van `.hero`:
- `.hero-sky` (flex: 1, de lucht) — bevat de tekst
- `.hero-earth` (fixed height, de aarde) — bevat de CTA knoppen

**De oplossing**: geef `.hero-earth` een negatieve `margin-top` zodat de cream aarde omhoog schuift en de onderste ~10-15% van de tekstregel "WE 'M WEL." bedekt. Compenseer met `padding-top` zodat de CTA knoppen op de juiste positie blijven.

De lettergrootte van `.hero-hl-line` is `clamp(3.4rem, 10.5vw, 9.5rem)` met `line-height: 1`.
10-15% van 9.5rem = ~0.95rem tot ~1.4rem.

**Correcte waarden**:
De gebruiker heeft een tekening gemaakt waarop de horizon **midden door de letter-body** van "WE 'M WEL." loopt — ongeveer 50% van de letterhoogte vanaf de onderkant. De onderste helft van de letters verdwijnt in de cream aarde.

Font-size is `clamp(3.4rem, 10.5vw, 9.5rem)` met `line-height: 1`.
50% van 9.5rem = ~4.75rem op desktop. Begin met:

```css
.hero-earth {
  margin-top: -4.75rem;
  padding-top: calc(48px + 4.75rem);
}
```

Itereer van daar: als de overlap te klein is, verhoog `margin-top` (meer negatief). Als te groot, verlaag. Target: de horizon loopt precies halverwege de letters van "WE 'M WEL."

Gebruik vaste `rem`-waarden, geen `clamp()` — dat veroorzaakt rekenfouten bij negatieve waarden.

## Verificatie-stappen (voer deze UIT vóór je klaar meldt)

### Stap 1: Meet de overlap in de browser
Voeg tijdelijk deze CSS toe en open de pagina in een headless browser of render de HTML:
```css
.hero-earth { outline: 2px solid red; }
```
De rode lijn moet DOOR de letter-bodies van "WE 'M WEL." lopen, niet eronder.

### Stap 2: Vergelijk met het logo
Open `assets/images/hornavologo.svg` en meet: de horizonlijn (`y=77.77`) als percentage van de viewBox hoogte (`121.32`) = 64%. De lijn zit dus op 64% van de totale SVG hoogte — maar wat telt is dat hij op de BASELINE van de grote letters zit.

Op de website: de horizon (bovenkant `.hero-earth`) moet op de baseline van de tekst zitten, waarbij de cream aarde net de serifs/voeten van de letters bedekt.

### Stap 3: Controleer alle kleuren
- Letters: `color: #FAF0DC` op alle drie `.hl-samen`, `.hl-bereiken`, `.hl-wemwel`
- Aarde: `background: #FAF0DC` op `.hero-earth`
- Lucht: blauw gradient op `.hero-sky`
- Geen bruine of donkere `--earth-color` variabelen meer in gebruik

### Stap 4: Controleer geen dubbele horizon
Zorg dat er GEEN extra pseudo-elementen (`::before`, `::after`) op `.hero-earth` of `.wemwel-wrap` zitten die een tweede horizonlijn tekenen. Er mag maar één horizon zijn: de overgang tussen `.hero-sky` en `.hero-earth`.

### Stap 5: Verhoog of verlaag `margin-top` indien nodig
De gebruiker wil dat de horizon **halverwege de letter-body** van "WE 'M WEL." loopt — de onderste ~50% van de letters verdwijnt in de cream aarde, de bovenste ~50% is zichtbaar tegen de blauwe lucht.

- Te weinig overlap (horizon zit onder of aan de baseline): verhoog `margin-top` (bijv. `-5rem`)
- Te veel overlap (meer dan 60% van de letter is bedekt): verlaag (bijv. `-3.5rem`)
- Correct: de lijn loopt precies halverwege de hoofdletters, zoals in de schets van de gebruiker

Controleer dit door een screenshot te maken van de pagina en te meten waar de cream aarde begint t.o.v. de letters.

## Wat je NIET moet aanraken
- De JavaScript dag/nacht logica (zon, maan, sky-gradients per tijdstip)
- De rest van de pagina (stats, products, values, footer)
- De nav
- De animaties (wordRise, fadeUp, sunBreath)

## Deliverable
Een werkend `index.html` waar de horizon door de onderkant van "WE 'M WEL." loopt. Geen extra lijnen, geen bruine kleuren, geen overlay-divs. Schone CSS.
