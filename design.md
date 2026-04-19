# Horizon Natuurvoeding — Design System
> Bijgewerkt april 2026 op basis van het officiële brandbook (20250415_HON_Natuurvoeding_Brandbook-compressed.pdf, 54 pagina's)
> Voor de volledige letterlijke brandbook-teksten: zie `brandbook-extract.md`

---

## 1. Merkidentiteit in één zin

Horizon is een **strategisch communicatiemerk** — geen consumentenmerk. De website is een B2B-podium voor retailers, distributeurs en partners. De productmerken (Horizon, Monki, Jori, Krekeltje) zijn de publieke gezichten; Horizon Natuurvoeding is de "vlag hoog in de mast."

---

## 2. Missie & Hogere Doel

| | |
|---|---|
| **Hogere doel** | 100% van iedereen 100% biologisch laten eten |
| **Overtuiging / Tagline** | Samen bereiken we 'm wel! |
| **Visie** | Dromen én doen — ambitie en realisme zijn twee kanten van dezelfde munt |
| **Aanpak** | Pragmatisch diversificeren: zelfde product, andere verhalen per doelgroep |
| **Reason to exist** | "Hier doen we het elke dag weer voor." |

---

## 3. Merkwaarden (de 5 kernwoorden)

| Waarde | Wat het betekent in communicatie |
|--------|----------------------------------|
| **Integer** | We doen wat we zeggen. Open, oprecht, geen verborgen agenda's. Transparantie is geen marketingtruc. |
| **Puur** | Schrijf zoals we produceren: zonder onnodige toevoegingen. Wat je leest is wat we bedoelen. |
| **Autonoom** | Geen trends, geen conventies. Op een Groningse manier zo nuchter dat het eigenzinnig wordt. |
| **Earthed** | Niet zweverig. Praktisch, menselijk, beide benen op de grond. |
| **Utopistisch** | Dromen groot, maar woorden die aanzetten tot doen. Overtuigen, niet inspireren om het inspireren. |

> **Belangrijk:** Gebruik deze woorden NIET letterlijk in communicatie (geen "integer", "puur", "autonoom" als kopregel). Vertaal ze in gedrag en taalgebruik.

---

## 4. Kleurenpalet (officieel uit brandbook p. 49)

### Primaire kleuren

| Naam | HEX | RGB | CMYK | Pantone | Gebruik |
|------|-----|-----|------|---------|---------|
| **Blauw** | `#009cdc` | 0/156/220 | 75/20/0/0 | 299CV | Primaire kleur. Logo, headlines, CTAs, accenten |
| **Notenpasta / Crème** | `#fff0d8` | 255/240/216 | 0/5/15/0 | — | Achtergrond. De "aarde" in de horizon-metafoor |
| **Zwart** | `#000000` | 0/0/0 | 60/40/40/100 | — | Body tekst, verpakking |
| **Wit** | `#ffffff` | 255/255/255 | 0/0/0/0 | — | Tekst op blauw, clean backgrounds |

### Accentkleur (spaarzaam gebruiken)

| Naam | HEX | RGB | CMYK | Gebruik |
|------|-----|-----|------|---------|
| **Biologisch Groen** | `#60d553` | 96/213/83 | 50/0/80/0 | Uitsluitend voor accenten: labels ('Nieuw!', 'Biologisch'), CTA-knoppen, badges. NOOIT voor grote vlakken of achtergronden. |

### Kleurgebruik tekst — richtlijnen (officieel, p. 50)

**✓ Toegestaan:**
- Witte tekst op blauwe achtergrond
- Zwarte tekst op blauwe achtergrond
- Zwarte tekst op crème achtergrond
- Blauwe tekst op crème achtergrond
- Witte tekst op groene achtergrond

**✗ NIET toegestaan:**
- Crème tekst op witte achtergrond
- Witte tekst op crème achtergrond
- Groene tekst op blauwe achtergrond
- Blauwe tekst op blauwe achtergrond
- Zwarte tekst op groene achtergrond

### Het horizon-gradient (signature element, p. 52)

Er zijn twee officiële gradient-varianten:

**Variant 1 — Blauw (primair):**
```css
background: linear-gradient(to bottom, #009cdc 0%, #b8e8f5 50%, #fff0d8 100%);
```
Met harde "horizon-cut" naar vol blauw. Logo staat net onder die lijn.

**Variant 2 — Crème (alternatief):**
```css
background: linear-gradient(to bottom, #fff0d8 0%, #b8e8f5 50%, #009cdc 100%);
```

> **Gradient-regel:** Er is altijd een **harde, scherpe scheiding** (hard cut) tussen het gradient-gedeelte en het volle kleurvlak. Het logo staat net onder die horizon-lijn. Dit is een kernelement van de visuele identiteit.

---

## 5. Typografie (officieel uit brandbook p. 47–48)

### Lettertypen

| Rol | Lettertype | Stijl | Gebruik |
|-----|-----------|-------|---------|
| **Display / Headlines / Logo** | **Clarendon Extra Narrow Extra Bold** | ALTIJD IN KAPITALEN | Koppen, titels, het logo zelf |
| **Onderregel / Sublines** | **DM Sans Medium** | KAPITALEN, spatiëring 330 | Onderregel bij logo, sublines |
| **Subkoppen** | **DM Sans Medium** | Onderkast | Subkoppen die een titel toelichten |
| **Bodycopy / Accenten** | **DM Sans Regular** | Normaal | Lopende tekst, bijschriften |

> **DM Sans is beschikbaar als Google Font** — gratis te laden via Google Fonts.
> **Clarendon** is een commercieel font (Adobe Fonts / Font Bureau). Voor web: de SVG-bestanden van het logo gebruiken.

### Technische instellingen

| Niveau | Font | Corpsgrootte | Interlinie | Spatiëring |
|--------|------|-------------|------------|------------|
| **Koppen** | Clarendon Extra Narrow ExBold | Variabel (X) | 100% | 0 |
| **Onderregel** | DM Sans Medium | 40% van Kop | 115% | 330 |
| **Subkoppen** | DM Sans Medium | Variabel (X) | 115% | 0 |
| **Bodycopy** | DM Sans Regular | 70% van Subkop | 116% | 0 |

### Typografie-regels

- Headlines zijn **altijd ALL CAPS** in Clarendon — zonder uitzondering
- Body copy is altijd **gewoon, leesbaar, zonder franje**
- Labels en onderregels gebruiken **wide letter-spacing** (spatiëring 330)
- Geen decoratief gebruik van cursief
- Clarendon = Zilla Slab of Rockwell als CSS-fallback voor web

### Web-implementatie (Google Fonts)

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
```

Voor Clarendon: gebruik de aangeleverde SVG-logo bestanden. Als fallback voor koppen gebruik `'Zilla Slab', 'Rockwell', serif`.

---

## 6. Logo-regels (officieel uit brandbook p. 42–46)

### Opbouw
Het logo bestaat uit:
1. **HORIZON** — groot, Clarendon Extra Narrow Extra Bold, in Horizon Blauw
2. **Onderregel** — DM Sans Medium, gespatieerd, ALL CAPS — altijd aangeleverde versies gebruiken

### Beschikbare logo-varianten met onderregel:
- HORIZON / SAMEN BEREIKEN WE 'M WEL!
- HORIZON / NATUURVOEDING
- HORIZON / NUTS | SEEDS | FRUITS
- HORIZON / NUECES | SEMILLAS | FRUTAS

### Marges
De vrijruimte rondom het logo is gelijk aan de **hoogte van de letter 'I'** uit het logo.

### Minimale formaten
- Digitaal: 15px (alleen logo) — 55px (logo + onderregel)
- Print: 15mm (alleen logo) — 20mm (logo + onderregel)

### Logo Dont's (p. 46) — VERBODEN:
- Logo in andere kleuren (rood, groen, grijs, etc.)
- Logo als outline/contour
- Logo met gradient toegepast op de letters zelf
- Logo verticaal geplaatst
- Logo vervormd of uitgerekt
- Zelf een tekst onder het logo typen (altijd aangeleverde versies gebruiken)

### Logo-kleur op achtergronden:
- Op blauw: logo in crème (#fff0d8) of wit
- Op crème: logo in blauw (#009cdc)
- Op gradient: logo in crème of blauw afhankelijk van positie
- NOOIT in zwart of groen

---

## 7. Communicatieprincipes (vertaald naar design)

| Principe | Implicatie voor ontwerp |
|----------|------------------------|
| **De horizon als leidraad** | Gebruik het gradient altijd als verbindend element. Elke pagina/uiting "ademt" de horizon. |
| **Vorm volgt inhoud** | Geen decoratie om de decoratie. Kleur en typografie dragen de boodschap. |
| **Natuurlijke eenvoud** | Klein palet. Weinig kleuren. Krachtige typografie. Geen drukke layouts. |
| **Authentieke autoriteit** | Zelfverzekerd maar niet arrogant. Geen hype-taal in CTAs. Directe, eerlijke buttons. |

---

## 8. Tone of Voice

### Kernomschrijving (officieel brandbook p. 20)
> "Ergens tussen nuchter en inspirerend."

We dromen groots, maar handelen concreet. Geen loze beloften, geen wollig taalgebruik. Directe, eerlijke communicatie. Betrokken en bevlogen, maar met zo min mogelijk woorden en met beide voeten in de klei.

### Schrijfwijzer — wat we NIET zeggen (p. 22–23)
- 'Wij zijn een revolutie in biologisch eten!'
- 'Onze producten zijn speciaal ontwikkeld voor bewuste consumenten.'
- 'Wij maken de wereld beter.'
- 'Biologisch eten begint bij Horizon.'
- 'Onze notenpasta's zijn revolutionair lekker.'
- 'Wij garanderen de beste kwaliteit.'
- 'Duurzaamheid is onze prioriteit.'
- 'Pure smaken.' / 'Een bewuste keuze.' / 'Van boer tot bord.'

### Schrijfwijzer — wat we WEL zeggen (p. 22–23)
- "Sinds 1978 doen wij met voeding wat iedereen zou moeten doen: niet al te gek veel."
- "Wat erop staat, zit erin. Niks meer, niks minder."
- "We werken mét de natuur i.p.v. 'eromheen'."
- "Roosteren, malen en klaar. Lekker hè?"
- "100% biologisch voor 100% van iedereen, dát is de droom."
- "100%-Geduld-Garantie!"
- "We werken al generaties met dezelfde boeren."
- "Zo smaakt eerlijk en verantwoord dus."
- "[naam product] waar niemand met z'n tengels aan heeft gezeten!"
- "100% Voedzaam, 0% Onnodige toevoegingen."
- "Zonder onzin." / "Eerlijk." / "Beide benen op de grond."

### Vertaald naar CTAs:
- ❌ "Meer informatie" → ✅ "Bekijk het assortiment"
- ❌ "Ontdek onze producten" → ✅ "Zien wat we maken?"
- ❌ "Neem contact op" → ✅ "Praat met ons"
- ❌ "Lees meer" → ✅ "Het hele verhaal"

### Slogans in actie (voorbeelden uit brandbook p. 25–28):
- "HOOFD IN DE WOLKEN, POTEN IN DE KLEI."
- "AARDE ZOEKT DROMERS"
- "VOOR BIO-ETERS IN ALLE SMAKEN"
- "'N BETERE WERELD BEGINT IN BETERE AARDE"
- "SAMEN BEREIKEN WE 'M WEL!"

---

## 9. Visuele layout-patronen

### Het signature headline-formaat
Grote, ALL CAPS Clarendon op het horizon-gradient. Wordt gebruikt op billboards, hero secties, campagne-uitingen, folders. De tekst staat op de overgang blauw→crème, of volledig op blauw. Nooit zwevend op wit.

### Logo-gebruik op uitingen
- Logo staat **altijd onderaan** uitingen (folders, billboards, vloot)
- "HORIZON" groot, "NATUURVOEDING" klein eronder, gespatieerd
- Op de website is het logo het navigatie-ankerpunt bovenin
- Kleur: altijd in Horizon Blauw, nooit zwart (tenzij crème achtergrond)

### Horizon-lijn als layout-device
- De letterlijke **harde horizontale lijn** tussen blauw en crème is een terugkerend layout-element
- In het logo: het woord "HORIZON" staat precies op de scheidslijn
- Toepasbaar in website: sectie-overgangen via deze gradient-split

### Groene accenten (knoppen / badges)
- Kleur: `#60d553`
- Tekst erop: wit, DM Sans Medium
- Vorm: afgeronde rechthoeken of cirkels
- Gebruik voor: CTAs ("Klik hier", "Lees meer"), badges ("NIEUW", "onze missie", "100% BIO")
- NOOIT als grote achtergrondvlakken

---

## 10. Website-specifieke aanbevelingen

### Gap analyse brandbook ↔ website

| Brandbook zegt | Implicatie voor de website |
|---------------|---------------------------|
| Gradient = signature element | Hero altijd op gradient, niet op wit |
| ALL CAPS Clarendon voor impact-koppen | Alle H1/H2 in Clarendon Extra Narrow Bold, ALL CAPS |
| Harde horizon-cut als layout-device | Sectie-overgangen via gradient-split |
| Eigenzinnige, directe tone of voice | Alle CTAs herschrijven |
| Horizon = B2B communicatie-merk | Expliciete retailer/partner-sectie nodig |
| Groene accenten alleen voor CTA-knoppen | Groene knoppen voor alle primaire acties |
| Logo altijd met onderregel | Gebruik SVG met "SAMEN BEREIKEN WE 'M WEL!" onderregel |

### Paginastructuur (aanbevolen voor index.html)

1. **Nav** — sticky, glazig (backdrop-filter), logo + 3 links
2. **Hero** — ALL CAPS Clarendon op horizon-gradient: "SAMEN BEREIKEN WE 'M WEL."
3. **Trust strip** — "1978 · SKAL · IJsselstein · 4 merken · 30+ medewerkers"
4. **Over ons / quote sectie** — brand story in eigen woorden
5. **Vijf merkwaarden** — INTEGER · PUUR · AUTONOOM · EARTHED · UTOPISTISCH
6. **Vier merken** — Horizon, Monki, Jori, Krekeltje
7. **B2B sectie** — "WIL JE ONS ASSORTIMENT VOEREN?" (retailers, distributeurs, food service)
8. **Product showcase** — met productfoto's
9. **Footer** — navy/blauw, logo groot, EU Bio badge, adresgegevens

---

## 11. Bedrijfsgegevens

| | |
|---|---|
| **Officiële naam** | Horizon Natuurvoeding |
| **Adres** | Lage Dijk-Noord 20-A, 3401 VA IJsselstein |
| **Telefoon** | 030 688 7730 |
| **Website** | horizonnatuurvoeding.nl |
| **Opgericht** | 1978, door Gaston Smit |
| **Huidige locatie** | IJsselstein (vanaf 1985) |
| **Medewerkers** | ±30 |
| **Assortiment** | Biologische noten, zaden, zuidvruchten |
| **Kernpartnerschappen** | o.a. Almendrera del Sur (amandelen, sinds 1998) |

---

## 12. Tone of voice in één zin

> "Nuchter over grote dromen. Direct, zonder franje. Zoals de natuur: geen haast, geen kunstmatige toevoegingen."

---

## Bronnen

- **Primaire bron:** `brandbook-extract.md` (volledige letterlijke tekst uit het brandbook)
- Brandbook PDF: `20250415_HON_Natuurvoeding_Brandbook-compressed.pdf` (54 pagina's)
- Website: `horizonnatuurvoeding.nl`
