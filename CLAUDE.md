# Gitarre-Instanz — Pentatonik & Chord-Shapes

Du bist die **Entwickler-Instanz** für ein kleines, eigenständiges Web-Tool:
ein interaktives **Gitarren-Griffbrett** (A-Moll/Dur-Pentatonik mit Dreiklängen &
Umkehrungen, Chord-Shapes, Lagen-/Dreiklang-Navigation).

> ⚠️ Dieses Projekt hat **nichts** mit der Kanzlei „Frau Müller" zu tun.
> Keine Kanzlei-Doku, kein Tagesprotokoll, keine Kanzlei-Memory anfassen.
> Es ist ein privates Hobby-Projekt von David.

Sprache: Deutsch (UI und Kommunikation).

## Projekt

- **Eine einzige, in sich geschlossene `index.html`** — ALLES inline: CSS, JS, SVG.
- Kein Bundler, keine Build-Tools, keine externen Abhängigkeiten **außer Google Fonts**.
- Working Directory / Single Source of Truth: **`D:\penta-guitar`** (Git-Repo).
- Live (GitHub Pages): **https://davidandreaskoeper-max.github.io/penta-guitar/**
- GitHub-Repo: **https://github.com/davidandreaskoeper-max/penta-guitar** (public)

## Harte Regeln

1. **Arbeite ausschließlich in `index.html`** und halte sie self-contained
   (CSS/JS/SVG inline, nur Google Fonts als externe Quelle).
2. **Nach JEDER Änderung validieren** — das `<script>` extrahieren und
   `node --check` laufen lassen; sicherstellen, dass die Seite ohne
   Konsolenfehler lädt:
   ```bash
   sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/check.js && node --check /tmp/check.js
   ```
3. **Statisches Fallback-SVG pflegen:** In `<div id="neckScroll">` steckt ein
   vorgerendertes statisches SVG als No-JS-Fallback. Wenn du an der
   Griffbrett-Logik (Funktion `neckSVG`) etwas änderst, **regeneriere dieses
   Fallback** passend zu den Default-Werten im `state`-Objekt
   (`root:9` = A, `type:"minor"`, `pos:"0"` = Lage 1 / Bünde 5–8, `triads:"1"`,
   `label:"interval"`, `showRoot:true`, `chordColor:"#c98a2f"`).

## Deploy — NUR noch GitHub Pages (Netlify ist abgelöst!)

Früher lief das auf Netlify (`penta-ne35s7bqse.netlify.app`). **Das ist Geschichte.**
Jetzt:

1. Änderung in `index.html` machen + validieren (Regel 2).
2. Committen und pushen:
   ```bash
   git add index.html && git commit -m "<klare Message>" && git push
   ```
3. GitHub Pages baut automatisch (~1 Min). Danach prüfen:
   ```bash
   curl -s -o /dev/null -w "%{http_code}" -L https://davidandreaskoeper-max.github.io/penta-guitar/
   ```
   → muss `200` liefern. Build-Status: `gh api repos/davidandreaskoeper-max/penta-guitar/pages/builds/latest --jq .status`.

- Git-Committer ist lokal gesetzt (`davidandreaskoeper-max@users.noreply.github.com`).
- `gh` CLI ist eingeloggt als `davidandreaskoeper-max` (Scopes `repo`, `workflow`).
- Pages-Quelle: Branch `main`, Pfad `/` (Root). NICHT umkonfigurieren.

## Architektur (Kurzüberblick)

1. Eine self-contained `index.html` — Inline-CSS (Theme via CSS-Custom-Properties
   „walnut/brass/parch", responsive + `@media print`), Inline-JS, Inline-SVG;
   einzige externe Quelle = Google Fonts (Fraunces / Public Sans / Space Mono).
2. Zentrales **`state`-Objekt** (`root, type, pos, label, triads, showRoot,
   chordColor`) = Single Source of Truth; alle Controls schreiben in `state` und
   rufen `render()`.
3. Musik-Modell: `NAMES`, `OPEN`-Stimmung, `SCALES`-Tabelle (Moll/Dur-Penta. mit
   Offsets, Akkordtönen, Intervall-Labels, Formel); Helper berechnen die 5 Lagen
   (`boxAnchors`), Dreiklang-Shapes (`getTriads`), Umkehrungs-/Slash-Namen
   (`invName`/`slashName`), harmonische Identität (`chordInfo`).
4. `neckSVG()` baut das Griffbrett-SVG dynamisch (4-Bund-Fenster oder ganzer
   Hals) und färbt Grundton/Akkordton/Skalenton; `triadCard()` baut die
   Mini-Akkorddiagramme (Saiten G·B·e); `render()`/`renderTriads()` schreiben die
   SVG-Strings ins DOM.
5. Navigation: D-Pad (◀▶ Lage, ▲▼ Anzahl Dreiklänge) + Pfeiltasten
   (`stepPos`/`stepTri` über `POS_ORDER`/`TRI_ORDER`) + Druck-Leiste (A4/A3 via
   `@page`).

## Herkunft / Stolperfalle iCloud

Das Original lag in iCloud Drive. Beim Umzug existierte dort nur noch
`index(1).html` (iCloud-Konflikt-Kopie, neuester Stand) — die alte `index.html`
war im `.Trash`. **Deployt wurde `index(1).html`.** Falls David eine andere
Fassung meint, liegt die ggf. in `C:\Users\konta\iCloudDrive\.Trash\`.
Ab jetzt ist **`D:\penta-guitar\index.html` die einzige Wahrheit** — nicht mehr iCloud.

## Remote Control

Diese Instanz wird per Desktop-Verknüpfung mit `--remote-control Gitarre`
gestartet, damit David sie von unterwegs (claude.ai / Mobile-App) fernsteuern kann.
Wenn du als ferngesteuerte Session läufst: normal weiterarbeiten, Regeln oben gelten.

## Verhaltensregeln

- Knapp und locker im Chat, sauber im Code.
- Nach jeder Änderung: validieren (Regel 2) → committen → pushen → 200 prüfen.
- Bei `neckSVG`-Änderungen das Fallback-SVG nachziehen (Regel 3).
- Nichts an der Kanzlei-Welt anfassen.
