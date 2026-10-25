# Trafikspel – Projektinfo

## Vad är detta?
En övningsapp inför teoriprovet för B-körkort (svenska vägmärken och trafikregler). Den är en enda HTML-fil, utan byggverktyg och utan ramverk: bara vanilla JavaScript, CSS och SVG. Den fungerar på både mobil (Pixel 8) och dator.

**Filer i arbetsmappen**
- `index.html` – själva appen.
- `marken.js` – alla 598 officiella vägmärkesbilder (WebP, inbäddade) plus Transportstyrelsens namn och beskrivningar. **Måste ligga bredvid `index.html`.**
- `app.html` – samma app utan `<html>`/`<head>`-skal (källan till Claude-artefakten).
- `PROJEKT-INFO.md` – den här filen.

**Claude-artefakt (privat länk):** https://claude.ai/artifact/Q6r2pRXYt5vrz9zpsSgGi7

---

## Avdelningar
| Flik | Innehåll |
|------|----------|
| Hem | Dagens övning (12 frågor, smart urval), "Öva på mina fel", statistik, dagar i rad |
| Vägmärken | Samtliga 331 märken i vägmärkesförordningen plus 267 varianter (598 bilder) i 12 kategorier. Valet **Vanligaste** (209 st) eller **Komplett** (598 st). Öva per kategori eller "Alla". Detaljkort med varianter, vändkort och test |
| Regler | 9 ämnen med korta lektioner, nyckeltal och 80 frågor. Ämnet Hastighet har ett interaktivt reglage för stoppsträcka |
| Vem kör först? | 12 animerade korsningar (högerregeln, vänstersväng, huvudled, stopplikt, utfartsregeln, cirkulationsplats, övergångsställe). Bilarna kör i rätt ordning när du har svarat |
| Provläge | Snabbprov (25 frågor, 20 min) och fullt prov (65 frågor, 50 min, 52 rätt för godkänt). Facit visas först när du lämnar in |

## Hur inlärningen fungerar
- Varje fråga har en "låda" 0–5 (Leitner-system). Rätt svar flyttar upp den, fel svar flyttar ned den till 0.
- Dagens övning väljer i första hand frågor du har svarat fel på, sedan sådana du inte har sett och sist sådana med låg låda.
- Ett märke räknas som "sitter" när både *märke → betydelse* och *betydelse → märke* har nått tillsammans minst 4.

## Vägmärkesdata
- Bilder, namn och beskrivningar har hämtats från Transportstyrelsens webbplats (vägmärkesförordningen) den 25 oktober 2026. Bilderna är skalade till högst 300–400 px och konverterade till WebP.
- Tipsen under "Tänk på" (`TIPS` i koden) är egna formuleringar.
- Vilka märken som räknas som "Vanligaste" styrs av listan `COMMON` i byggskriptet. Det är en bedömning av vad som är vanligast i trafiken och på teoriprovet.
- Varje bild har ett id som motsvarar Transportstyrelsens filnamn (till exempel `a19-1` eller `c31-5`) och en kod (`A19`).

## Lagring
Framstegen sparas i webbläsarens `localStorage` (nyckeln `trafikspel.v1`), alltså separat per enhet och per webbläsare. Om du vill synka mellan mobil och dator går det att lägga till Firebase på samma sätt som i London-projektet.

---

## Publicering – GitHub Pages (samma upplägg som London-appen)
1. Skapa ett nytt repo på GitHub, till exempel `poptrem911/trafikspel` (Public).
2. Ladda upp **både** `index.html` och `marken.js` till repots rot (Add file → Upload files → Commit changes).
3. Settings → Pages → Source: *Deploy from branch* → `main` / `(root)` → Save.
4. Efter cirka en minut är appen live på https://poptrem911.github.io/trafikspel/

**Uppdatera:** be Claude ändra i `index.html` i den här mappen, och dra sedan in den nya filen i repot (`index.html` → pennan → dra in filen → Commit changes).

**Tips för Pixel 8:** öppna adressen i Chrome → menyn ⋮ → *Lägg till på startskärmen*. Då startar appen som en vanlig app.

---

## Teknik
- Typsnitt: Overpass (rubriker), Atkinson Hyperlegible (brödtext, mycket lättläst), Overpass Mono. Laddas från Google Fonts, med systemtypsnitt som reserv.
- Vägmärkena är Transportstyrelsens officiella bilder (från `marken.js`). Korsningsanimationerna är ritade i SVG.
- Regelfrågor finns i arrayen `RULES`: `[id, ämne, fråga, [rätt svar, fel, fel, fel], förklaring, märke?]`. Det första svaret är alltid det rätta, och svaren blandas när frågan visas.
- Korsningarna finns i arrayen `SCEN`. Varje bil har `from` (S/E/N/W), `move` (straight/left/right), samt facit (`answer`) och körordning (`order`).
- Ljust och mörkt läge följer telefonens inställning.
- Tangentbord på datorn: 1–4 väljer svar, Enter går till nästa fråga.

## Att lägga till frågor
Lägg till en rad i `RULES`, till exempel:
```js
['park12','park','Din fråga?',['Rätt svar','Fel 1','Fel 2','Fel 3'],'Förklaring.','c_park'],
```
