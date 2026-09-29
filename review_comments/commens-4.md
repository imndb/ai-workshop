# Review `Workshop.html` — 10 weitere Verbesserungspunkte (3. Durchgang)

**Perspektive:** Java-Entwickler mit AI-Coding-Erfahrung
**Fokus dieses Durchgangs:** visuelle Gestaltung, Publikumsführung, Tiefe für erfahrene Entwickler
**Datei:** `doc/Workshop.html`

---

## 1. Zu textlastige Slides entlasten

Mehrere Slides (z. B. Index 4 „RAI-Leitplanken", Index 19 „Risiken") stapeln 5 Bullet-Points mit Halbsätzen — das liest das Publikum, statt zuzuhören. Faustregel: **max. 3 Bullets pro Folie, je ≤ 8 Wörter**, den Rest sagt der Vortragende.

**Beispiel-Kürzung Slide 4:**
- Vorher: „Source Code ist mindestens „Intern". Verarbeitet die Anwendung höher klassifizierte Daten, übernehmen Code und Kopien die höchste Klassifizierung."
- Nachher: „Source Code = mindestens „Intern"" (Rest mündlich erklären)

## 2. Code-Beispiele syntax-highlighten statt nur Farbklassen ungenutzt zu lassen

Im CSS existieren `.comment`, `.kw`, `.str` (Zeilen 100–102), werden aber in den `<pre>`-Blöcken **nirgends verwendet** außer in Slide 14/16. Der `AGENTS.md`-Codeblock (Slide 13) und der MCP-Flow (Slide 18) sind reiner Fließtext ohne Highlighting — wirkt für ein Java-Publikum, das IDE-Highlighting gewohnt ist, unfertig.

**Fix:** `<span class="comment">`/`<span class="kw">` konsequent auf allen `<pre>`-Blöcken anwenden, oder ein leichtgewichtiges JS-Highlighting (z. B. `highlight.js`) einbinden.

## 3. Kontraststarke Diagramme statt reiner Tabellen für Prozess-Slides

Slide 11 (Agentic Loop) und Slide 18 (MCP) stellen Abläufe als reinen `<pre>`-Text mit Pfeilen dar (`Prompt → Reasoning → Tool Call → …`). Das ist für einen Kern-Baustein des Vortrags zu wenig visuell.

**Beispiel:** Ein einfaches SVG- oder CSS-Flow-Diagramm mit Boxen und Pfeilen, die beim Klicken/Advance einzeln aufscheinen (Progressive Disclosure) — macht den Loop im Vortrag lebendiger als ein Codeblock.

## 4. Tempo-Kontrolle: Sprecher-Notizen fehlen komplett

Es gibt keine Möglichkeit, private Redenotizen zu hinterlegen (z. B. „hier 2 Min. Pause für Fragen", „Timing: bis hier 15 Min."). Bei einem 27-Slide-Deck ohne Zeitmarker läuft man leicht aus dem Ruder.

**Vorschlag:** `data-notes="..."` Attribut pro `<section>` plus ein per Tastenkürzel (`N`) umschaltbares Notizfenster, das nur am Presenter-Bildschirm sichtbar ist (z. B. via zweitem Fenster mit `window.open` + `BroadcastChannel`).

## 5. Kein Slide-Timer / keine Fortschrittsanzeige nach Zeit

Der `progress-bar` (Zeile 152) zeigt nur den Slide-Fortschritt, keine Zeit. Bei einem Workshop mit Zeitbudget (z. B. 60 Minuten) hilft ein Timer im HUD enorm, um zu sehen, ob man in Verzug ist.

**Beispiel-Ergänzung im HUD:** `<span id="timer">00:00</span>` mit `setInterval`, der ab Slide-1-Anzeige hochzählt — Vortragende sehen sofort, ob Konzept-Teil (Slides 9–18) zu lange dauert.

## 6. Tiefere technische Slide für erfahrene Java-Devs: Wie funktioniert Tool-Calling wirklich?

Das Deck erklärt den Agentic Loop nur konzeptionell. Ein Java-Entwickler mit AI-Erfahrung fragt: *Wie sieht der tatsächliche JSON-Tool-Call aus, den das Modell erzeugt?*

**Beispiel-Slide-Inhalt:**
```json
{
  "tool_calls": [{
    "name": "run_in_terminal",
    "arguments": { "command": "mvn test -Dtest=ZinsServiceTest" }
  }]
}
```
Das Modell generiert **strukturierten Text**, kein „magisches Ausführen" — die Runtime (IDE/CLI) parst das JSON und ruft die echte Funktion auf. Das entmystifiziert den Begriff „Agent" für technisch versierte Zuhörer und verbindet ihn mit bekannten Konzepten wie Function-Calling/JSON-RPC.

## 7. Vergleichsslide fehlt: Copilot vs. Claude Code vs. Cursor — konkrete Unterschiede

Slide 7 („Stufen des AI-Coding") nennt Tools nur als Beispiele in einer Tabellenzelle. Ein Java-Team, das sich für ein Tool entscheiden muss, braucht mehr Substanz.

**Beispiel-Vergleichstabelle:**

| Kriterium | GitHub Copilot | Claude Code | Cursor |
|---|---|---|---|
| IDE-Integration | Tief (IntelliJ, VS Code) | CLI/Terminal-first | Eigene IDE (VS Code Fork) |
| Agent-Modus | Ja (Copilot CLI/Agent Mode) | Ja (nativ) | Ja (Composer) |
| Enterprise-Governance | RAI-Instanz vorhanden | Zu prüfen | Zu prüfen |
| Am besten für | Bestehende JetBrains-Workflows | Tiefe CLI-Automatisierung | Schnelles Prototyping |

Das liefert eine echte Entscheidungsgrundlage statt nur Namedropping.

## 8. Interaktivität: keine Publikums-Abstimmung / kein Live-Poll

Bei einem Workshop-Format (nicht reiner Vortrag) fehlt jede Interaktion außer Vor-/Zurück-Navigation. Eine kurze Abstimmung erhöht Engagement massiv.

**Beispiel:** Nach Slide 3 („Warum AI Coding?") eine Slide einbauen: *„Wer von euch nutzt bereits täglich einen AI-Agenten? 🙋"* — mit einfachem Zeige-die-Hand-Moment oder QR-Code zu einem Mentimeter/Slido-Poll. Kostet 2 Minuten, erhöht aber die Aufmerksamkeit für den Rest des Vortrags erheblich.

## 9. Barrierefreiheit: Farbkontrast und Tastaturzugänglichkeit prüfen

`--muted-2: #6d7581` auf `--bg: #14171c` (z. B. `.hint`, `.comment`) hat einen sehr niedrigen Kontrast (~3.2:1, WCAG AA verlangt 4.5:1 für Fließtext). Für Projektionen in hellen Räumen oder Teilnehmer mit Sehschwäche problematisch.

**Fix:** `--muted-2` für Fließtext auf mindestens `#8a92a0` anheben oder nur für rein dekorative Elemente verwenden. Zusätzlich: Der Slide-Wechsel per Tastatur funktioniert gut (Pfeiltasten, Leertaste), aber es gibt keinen sichtbaren Fokus-Indikator auf den HUD-Buttons außer dem generischen `:focus-visible` — für Screenreader-Nutzer fehlt z. B. `aria-live` auf dem Counter, damit Foliennummern-Wechsel angesagt werden.

## 10. Abschluss-CTA fehlt — was passiert nach dem Workshop konkret?

Slide 21 („Nächste Schritte") ist gut gemeint, aber unpersönlich: „AI-Readiness-Score pro Repo erheben" — von wem, bis wann, wie? Ohne Verantwortlichkeit verpufft das im Alltag.

**Beispiel-Verbesserung:**
```
Nächste Schritte — mit Owner & Deadline

☐ AI-Readiness-Score für Team-Repo XY   → [Name]   bis Freitag
☐ AGENTS.md im Hauptrepo anlegen         → [Name]   bis nächste Woche
☐ Erstes SKILL.md im Team-Space          → [Name]   bis Sprint-Ende
☐ Follow-up-Termin in 2 Wochen           → gebucht: [Datum]
```

Ein Projektor-Screenshot mit echten Namen (oder Platzhaltern zum Ausfüllen live im Workshop) macht aus einer Wunschliste einen verbindlichen Aktionsplan.

---

## Kurz-Fazit (3. Durchgang)

Während der erste Durchgang (`comments-1.md`) technische Deck-Bugs und Java-Beispiele adressierte und der zweite (`commens-2.md`) Governance und Team-Prozesse vertiefte, fokussiert dieser dritte Durchgang auf **Wahrnehmung und Vortragsqualität**: weniger Text pro Folie, echtes Syntax-Highlighting, ein Tool-Calling-Deep-Dive für erfahrene Entwickler, ein handfester Tool-Vergleich, Publikumsinteraktion, Barrierefreiheit und ein Abschluss mit echten Verantwortlichkeiten statt einer reinen Wunschliste.

**Die fünf wirksamsten Hebel aus diesem Review:**

1. Slides entschlacken (max. 3 Bullets, Rest mündlich)
2. Tool-Calling-JSON zeigen — entmystifiziert „Agent" für Techniker
3. Konkreter Tool-Vergleich (Copilot/Claude Code/Cursor) als Entscheidungshilfe
4. Timer + Sprecher-Notizen für saubere Zeitführung
5. „Nächste Schritte" mit Owner + Deadline statt vager Absichtserklärung

