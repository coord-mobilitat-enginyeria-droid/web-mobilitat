# Web Mobilitat — Redisseny pàgina mobilitat Escola d'Enginyeria UPF

## PRÒXIMA TASCA (començar per aquí)

- Revisar visualment el mapa azimutal (`card_estudiar_fora.svg`) al navegador i ajustar si cal (etiquetes, opacitats, escala)
- Canviar títol card 1 a "Estudiar enginyeria fora" (suggeriment Alfonso)
- Crear subpàgines: Outgoing (per fases), Incoming
- Preparar blocs HTML nets per a que secretaria els pasti a Liferay

## Context

Estem redissenyant la pàgina de mobilitat de l'Escola d'Enginyeria de la UPF (`/web/enginyeria/mobilitat`). L'objectiu és millorar-la prenent com a model la Facultat de Dret (la millor valorada, 9.5/10) i adaptar-la a Enginyeria.

El flux de treball és: crear la web en local → compartir amb secretaria → secretaria la replica a Liferay (CMS de la UPF) pegant HTML en mode font de l'editor.

## Estat actual

### Fitxers

- `mobilitat_replica.html` — Rèplica exacta de la pàgina actual d'Enginyeria (HTML original amb URLs absolutes). NO TOCAR — és la referència.
- `mobilitat_nova.html` — Landing alternativa en construcció, basada en l'estructura de Dret. Usa el chrome real de Liferay (header, dockbar, breadcrumb, footer) de la rèplica.
- `mobilitat.html` — Versió neta anterior (CSS propi, no Liferay). Referència de contingut, no s'usa.
- `card_estudiar_fora.svg` — Mapa mundial amb projecció azimutal centrada a Barcelona. **65 destinacions d'Enginyeria (59 internacionals + 6 SICUE)** (25 països), siluetes de continents (Natural Earth 110m), línies radials des de Barcelona, anells de distància (2k/5k/10k/15k km). Sense llegenda integrada — els números els porta el HTML hero overlay (trilingüe natiu via Liferay). Fons blau fosc. Generat per `generate_map.py`.
- `generate_map.py` — Script Python 3 (requereix `openpyxl`). Font de veritat: `universitats vs centres UPF.xlsx` (Dropbox) per la llista d'Enginyeria; join per Codi amb `destinacions_upf.csv` per coordenades; `EXTRA_DESTINATIONS` (dins l'script) per universitats amb codis placeholder (USA999, CAN999, etc.). Descarrega Natural Earth GeoJSON i genera el SVG.
- `ne_110m_land.geojson` — Cache local de Natural Earth 110m land (descarregat automàticament per `generate_map.py`).
- `card_incoming.svg` — Skyline de Barcelona estil Cobi/Mariscal: colors vius, traç gruixut, Sagrada Família, Torre Glòries, Hotel W, palmeres, veler, gavines. Sense text UPF.
- `original.html` — HTML brut descarregat de la pàgina d'Enginyeria.
- `dret_original.html` — HTML brut descarregat de la landing de Dret.
- `destinacions_upf.csv` — CSV de totes les destinacions UPF (tots els centres), descarregat de Google Sheets.
- `analisi_webs_mobilitat_upf.md` — Anàlisi comparativa dels 8 centres UPF (ranking, bones pràctiques, recomanacions).

### Decisions preses

- **Estructura landing**: 2 cards (com Dret) → "Estudiar fora" + "Incoming students"
- **Subtítols**: "Abans, durant i després de l'estada" / "Study at UPF School of Engineering"
- **Imatge Card 1**: Mapa mundial amb projecció azimutal centrada a Barcelona, 65 destinacions d'Enginyeria (59 internacionals + 6 SICUE) (25 països), siluetes de continents (Natural Earth), línies radials des de Barcelona, anells de distància
- **Imatge Card 2**: Skyline Barcelona estil Cobi/Mariscal (colors vius, traç gruixut, playful)
- **Estil Cobi/Mariscal**: No hi ha problemes legals — l'estil artístic no està protegit, només el personatge. Encaixa amb Barcelona i és diferenciador respecte als altres centres UPF
- **Layout responsiu**: `aspect-ratio: 16/9` per les imatges, flex amb `upf__limit-page-width` (classe CSS de Liferay que centra el contingut al mateix ample que títol i breadcrumb)
- **Centrament**: Les cards usen `upf__limit-page-width` — la mateixa classe que el títol "Mobilitat" i el breadcrumb. No un max-width fix.

### Decisions recents

- **Color etiqueta cards**: Vermell UPF (#c8102e) per les dues cards. Decidit.
- **Llegenda del mapa**: Moguda a la cantonada inferior dreta.
- **Projecció mapa**: Implementada projecció azimutal equidistant centrada a Barcelona (41.39°N, 2.17°E). Barcelona al centre, destins radials, línies rectes (propietat azimutal), anells de distància concèntrics. Script `generate_map.py` per regenerar.
- **Destins d'Enginyeria**: La font de veritat per les internacionals és el xlsx (`universitats vs centres UPF.xlsx`, Dropbox), columna "Escola d'Enginyeria" amb "x" → 59 destinacions, 25 països. Les 6 destinacions SICUE (Espanya) es defineixen al `SICUE_DESTINATIONS` de l'script. **Total: 65 destinacions, 26 països**. Continents eliminats del comptatge per ambigüitat.
- **Títol card 1**: Proposta "Estudiar enginyeria fora" (pendent d'aplicar).

### Problemes resolts

- Cards fora de marges → solucionat amb `upf__limit-page-width`
- Cards massa grans → solucionat amb `aspect-ratio: 16/9` en lloc d'alçada fixa
- Mapa amb poc destins → ara 59 destinacions reals d'Enginyeria, font xlsx (joinat amb CSV per coords)
- Mapa sense continents → ara amb siluetes Natural Earth 110m projectades
- Projecció equirectangular → canviada a azimutal equidistant centrada a Barcelona
- Skyline massa fosc → refet en estil Cobi/Mariscal amb colors vius

### Per fer

- [x] Decidir color etiquetes → vermell UPF (#c8102e)
- [x] Projecció mapa → azimutal equidistant centrada a Barcelona
- [ ] Revisar mapa visualment al navegador i ajustar etiquetes/opacitats
- [ ] Canviar títol card 1 a "Estudiar enginyeria fora"
- [ ] Ajustar mapa: responsive crop centrat a Barcelona per pantalles petites
- [ ] Crear subpàgines: Outgoing (per fases), Incoming
- [ ] Preparar blocs HTML nets per a que secretaria els pasti a Liferay

## Anàlisi de referència

### Ranking webs mobilitat UPF (març 2026)

1. Dret 9.5 — 10 pàgines, Google Sheets equivalències + Form, 5 fases post-adjudicació
2. Comunicació 8.5 — 8+ pàgines, 3 Sheets per grau, delegats mobilitat estudiantils
3. Economia 8 — 9+ pàgines, blog WordPress experiències, 6 tutors geogràfics
4. Traducció 8 — millor FAQ (16 preguntes), millor incoming (trilingüe)
5. Medicina 7.5 — 3 modalitats úniques (intercanvi, pràctiques recerca, clíniques)
6. Humanitats 7 — 37 PDFs equivalències per país
7. Polítiques 7 — millor dossier acadèmic PDF, cita virtual Google Calendar
8. Enginyeria 6 — contingut existeix però no es troba (tot comprimit en 1 subpàgina)

### Cards a la UPF — estils observats

| Lloc | Fons etiqueta | Estil |
|---|---|---|
| Dret, Comunicació | #353430 gris fosc | Carousel Swiper, img 190px + label 110px |
| Economia, Enginyeria home | #fff blanc | Grid, greyscale → color on hover |
| UPF home | #eee / blanc | Botons vermell UPF per accents |

## Tècnic

- La UPF usa **Liferay DXP** amb tema `upf-2016-theme`
- Color corporatiu: `#c8102e` (vermell UPF)
- Classe de centrament contingut: `upf__limit-page-width`
- Carousel de Dret: Swiper.js amb `data-autoscale="964"`
- Editor Liferay té mode "Source" per pegar HTML
- Servei web gestionat per Factoria (Biblioteca i Informàtica), peticions via CAU
- Nom oficial en anglès: "School of Engineering"

## Propostes futures

### Automatitzar l'extracció d'assignatures incoming dins del Sheet

Actualment l'extracció dels codis TIC+GEBM oferts a incoming es fa amb Python
local (`extract_incoming_codes.py`, `crosscheck_incoming.py`) sobre un xlsx
baixat manualment del Sheet d'OD. Es pot moure dins del propi Google Sheet amb
Apps Script:

- `Range.getFontColors()` i `Range.getFontLines()` donen accés al color i al
  strikethrough, que és la lògica clau (codi negre = primari; strike =
  descartat).
- Afegir un menú `Incoming → Generar llista` que escrigui els codis en un full
  nou i exporti xlsx via `DriveApp.getFileById(...).getBlob()`.
- La part de cross-check amb la web és fràgil (Liferay canvia templates).
  **Millor invertir el flux**: la web consumeix el Sheet publicat (File →
  Publish to web com a CSV) en lloc que un script consulti la web. Així la
  veritat viu a Secretaria i la web sempre està alineada per construcció.
