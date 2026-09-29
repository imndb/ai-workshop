# Review `Workshop.html` — 10 konkrete Verbesserungspunkte

Perspektive: Java-Entwickler mit AI-Coding-Erfahrung

## 1) Storyline klar in 3 Akte trennen
Aktuell springen die Themen (z. B. Quellen/Merksatz vor späteren Technik-Slides). Besser:
1. **Warum + Leitplanken**
2. **Konzepte + Demo**
3. **Risiken + Umsetzung im Team**

**Beispiel:** Nach „Spec-Driven Development“ direkt „Nächste Schritte“, dann „Merksatz“, dann „Quellen“ als letzte Folie.

## 2) Java-nahe Live-Demo einbauen
Die Folien erklären viel, aber ein 3–5-Minuten-Durchlauf im echten Repo wirkt stärker.

**Beispiel-Demo:**
- Endpoint erweitern (`HelloController`)
- Test ergänzen (`HelloControllerIntegrationTest`)
- `mvn test` laufen lassen
- Diff gemeinsam reviewen

So sehen alle den Agentic Loop in der Praxis.

## 3) „Gute“ vs. „schlechte“ Prompts gegenüberstellen
Der Unterschied zwischen Prompting und Context Engineering ist gut, aber noch abstrakt.

**Beispiel:**
- Schlecht: „Mach das besser.“
- Gut: „Refactore nur `HelloController`, keine neuen Dependencies, Tests dürfen nicht abgeschwächt werden, liefere Patch + Begründung.“

Damit lernen Teilnehmer sofort reproduzierbares Arbeiten.

## 4) Token/Context mit realen Größenordnungen erklären
„100k Tokens“ ist für viele schwer greifbar.

**Beispiel-Tabelle:**
- Kleine Java-Klasse: ~1k–3k Tokens
- Service + Testklasse: ~4k–8k
- Langes Build-Log: 5k–30k

Kernaussage: Kontext gezielt auswählen statt „alles in den Prompt“.

## 5) Risiken mit echten Failure-Cases zeigen
Die Risiko-Slide ist gut, aber konkrete Fehlschläge prägen sich besser ein.

**Beispiele:**
- Agent setzt `@Disabled` statt Bug zu beheben
- Assertion wird abgeschwächt, damit Tests grün werden
- Halluzinierte API-Methode in Spring

Lernpunkt: „Grün“ ist nicht automatisch „richtig“.

## 6) Security-Folie um konkrete Schutzregeln ergänzen
Prompt-Injection wird erwähnt, aber ohne operatives Vorgehen.

**Beispiel-Regeln:**
- Keine Secrets im Prompt
- Keine ungeprüften externen MCP-Server
- Tool-Permissions minimal halten (least privilege)
- Vor Merge: Security-Review-Checkpoint

Das hilft Teams, direkt handlungsfähig zu sein.

## 7) `AGENTS.md` als sofort nutzbares Template zeigen
Die Idee ist stark; noch besser mit copy-paste-fähigem Minimalstandard.

**Beispiel-Inhalt:**
- Build/Test-Befehl (`mvn -q test` oder `mvn verify`)
- Architekturgrenzen (z. B. keine Layer-Verletzung)
- Verbotene Aktionen (keine Secrets, keine stillen Dependency-Adds)
- Definition of Done (Tests + Review + kurze Begründung)

So wird aus Theorie direkt Teamstandard.

## 8) „Wann kein Agent?“ explizit machen
Sehr wichtig für Senior-Entwickler: nicht jede Aufgabe passt.

**Beispiele für „kein Agent zuerst“:**
- Kritische Security-Fixes unter Zeitdruck
- Komplexe Domainlogik ohne gute Tests
- Migrationsschritte mit hohem Datenrisiko

Dann erst Ask/Plan-Modus nutzen, später automatisieren.

## 9) Mehr Fokus auf Review-Kompetenz statt nur Geschwindigkeit
Die Präsentation betont Tempo; ergänzend sollte Qualität als Haupthebel sichtbarer sein.

**Beispiel-Checkliste für Diff-Review:**
- Verändert sich Verhalten unbeabsichtigt?
- Sind Randfälle durch Tests abgedeckt?
- Bleibt die Architektur konsistent?
- Entstehen neue Kopplungen/Tech Debt?

Damit bleibt AI ein Verstärker guter Engineering-Praktiken.

## 10) Abschluss mit 30-Tage-Umsetzungsplan
Der „Nächste Schritte“-Slide ist gut, kann aber konkreter sein.

**Vorschlag:**
- Woche 1: 1 Repo pilotieren + Leitplanken definieren
- Woche 2: `AGENTS.md` + 1 Skill einführen
- Woche 3: 2 Live-Use-Cases messen (Zeit/Qualität)
- Woche 4: Retrospektive + Standards nachschärfen

So endet der Vortrag mit einem klaren, realistischen Rollout.

---

## Kurzfazit
Inhaltlich ist das Deck stark und verantwortungsbewusst. Mit mehr **Java-naher Demo**, **konkreten Failure-Cases** und **operativer Umsetzung (30-Tage-Plan)** wird die Präsentation deutlich wirksamer für echte Teams im Alltag.
