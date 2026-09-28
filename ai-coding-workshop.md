---
marp: true
theme: default
paginate: true
size: 16:9
footer: 'AI Coding Workshop @ Raiffeisen'
style: |
  section {
    font-size: 26px;
  }
  section.lead h1 {
    font-size: 2.1em;
  }
  section.lead h2 {
    font-size: 1.1em;
    font-weight: 400;
    color: #555;
  }
  table {
    font-size: 0.7em;
  }
  code {
    font-size: 0.82em;
  }
  blockquote {
    border-left: 6px solid #d40000;
    padding-left: 0.8em;
    font-style: italic;
    color: #333;
  }
---

<!-- _class: lead -->
<!-- _paginate: skip -->

# 🤖 AI Coding im Alltag
## Wie Agentic Coding uns schneller macht – sicher und kontrolliert

**AI Coding Workshop @ Raiffeisen**
Kurzvortrag, 15–20 Minuten

---

## Agenda

1. Warum AI Coding — und wo die Grenzen liegen
2. Raiffeisen-Leitplanken
3. AI-Readiness: was vor dem ersten Agent-Run stimmen muss
4. Die wichtigsten Konzepte — einfach erklärt, mit Beispielen
   *(LLM, Tokens, Agent, Prompt- vs. Context-Engineering, Instructions, Skills, Subagents, Multi-Agent, MCP)*
5. Risiken & Spec-Driven Development
6. Nächste Schritte

---

## Warum AI Coding?

> AI Coding kann Reibung reduzieren, aber nur auf einer gesunden Engineering-Basis.

- Schnellere Exploration, weniger Boilerplate, schnelleres Onboarding in fremdem Code
- **Kein** Ersatz für Architektur, Tests, Code Reviews oder Verantwortung
- Ein KI-Agent verstärkt, was im Repo schon da ist — gut **und** schlecht
- Merksatz für den ganzen Vortrag:

> Der Agent ist so gut wie dein Repo, deine Tests und dein Context.

---

## Raiffeisen-Leitplanken — Regel #1: Vertraulichkeit

- Source Code ist **mindestens „Intern“**. Verarbeitet die Anwendung höher klassifizierte Daten, übernehmen Code und Kopien die höchste Klassifizierung.
- Keine vertraulichen/geheimen Code- oder Dateninhalte in externe AI-Tools — nur **freigegebene Enterprise-Instanzen** und freigegebene Use Cases.
- SITAN & AGB **vor** der Nutzung prüfen: Wo werden Daten gespeichert? Fliessen sie ins Training? Wer hat Zugriff?
- MCP-Server und Agents brauchen **explizite Governance** (Zielsystem, Datenklassifizierung, vertrauenswürdige Quellen, Auth-Modell).
- Die menschliche Verantwortung bleibt bei uns — Review, Test, Nachvollziehbarkeit **vor** jedem Merge.

*(Quelle: „AI for Code“-Governance, „AI & Security“, MCP-Freigabeprozess, Generative AI @ Raiffeisen)*

---

## AI-Readiness: bevor der Agent das Repo berührt

| # | Voraussetzung | Warum |
|---|---|---|
| 1 | Saubere Architektur | Klare Modulgrenzen → klare KI-Ergebnisse |
| 2 | Automatisierte Quality Gates | Linting, Tests, SonarQube fangen Fehler ab |
| 3 | Schneller Feedback-Loop | Schnelle Builds/Tests = sichere Iteration |
| 4 | Kontext by Design | README, ADRs, `AGENTS.md`/`CLAUDE.md` |
| 5 | Security-Isolation | Sandbox, keine Secrets im Kontext, Least Privilege |
| 6 | Team-Fähigkeit | Diffs hinterfragen statt blind mergen |

**Ohne 1–3 verstärkt KI bestehende Probleme** — unklare Grenzen und flaky Tests werden zu *schnellerem* Chaos.

---

## Wie ich einen AI-Agenten sehe

Vier Analogien, die im Team funktionieren:

- 🤝 **Helfer** — schneller, effizienter Mitarbeiter
- 🧭 **Discoverer** — findet sich im Projekt zurecht, kennt Dinge, die ich noch nicht gemacht habe
- 👥 **Pair Programmer** — ich bleibe der Reviewer
- ⚙️ **Executor** — führt Dinge aus, die ich selbst noch nie umgesetzt habe

> Es ist ein Kollege, kein Orakel. Die Verantwortung bleibt bei mir.

---

<!-- _class: lead -->

# Die wichtigsten Konzepte
### Einfach erklärt — mit Beispiel

---

## Konzept 1 — Was ist ein LLM?

Ein **Large Language Model** ist ein neuronales Netz, trainiert auf riesigen Textmengen. Es sagt jeweils das **wahrscheinlichste nächste Token** (Textstück) voraus — es „weiss" nichts, es **rechnet Wahrscheinlichkeiten**.

**Beispiel:**
`"Der Himmel ist ___"` → Modell schlägt `"blau"` vor, weil diese Fortsetzung in den Trainingsdaten am häufigsten vorkam.

**Für die Praxis wichtig:**
- LLMs sind **probabilistisch** → gleiche Frage kann leicht unterschiedliche Antworten liefern
- Trainings-Cutoff: das Modell kennt die Welt nur bis zu einem Stichtag → aktuelle Infos müssen als Kontext oder über Tools (MCP, Web-Fetch) mitgegeben werden

---

## Konzept 2 — Tokens & Context Window

- Text wird in **Tokens** zerlegt (grob: 1 Token ≈ 4 Zeichen)
- Beispiel: `"Kontoauszug"` → z.B. drei Tokens: `Kon` `to` `auszug`
- **Context Window** = das Kurzzeitgedächtnis des Modells: alles, was es in einer Anfrage „sieht" — Prompt, Dateien, bisheriger Verlauf, Tool-Ergebnisse
- Ist das Fenster voll, wird älterer Inhalt komprimiert oder vergessen (**Compaction**)

**Praxis-Tipp:**
Sessions unter ca. **100k Tokens** halten. Werkzeuge dafür: `/context` (Stand anzeigen), `/compact "wichtige Entscheidungen behalten"`, `/clear` (Neustart zwischen Aufgaben).

---

## Konzept 3 — Was ist ein AI-Agent? (Agentic Loop)

**Assistant** chattet nur — **Agent** handelt: liest Dateien, schreibt Code, führt Befehle aus, wertet Ergebnisse aus.

Der sogenannte **ReAct-Loop**:
`Prompt → Reasoning → Tool Call → Tool-Ergebnis → (wiederholen) → Antwort`

**Beispiel:**
„Füge einen Test für die Zinsberechnung hinzu."
→ Agent liest `ZinsService.java`
→ schreibt `ZinsServiceTest.java`
→ führt `mvn test` aus
→ sieht einen Fehler in der Zusicherung
→ korrigiert den Test
→ wiederholt, bis alle Tests grün sind

Der Loop läuft **autonom** — der Mensch setzt das Ziel und prüft das Ergebnis.

---

## Konzept 4 — Prompt-Engineering vs. Context-Engineering

**Prompt-Engineering** = **wie** ich frage (Rolle, Beispiele, Struktur, Ton)
> „Du bist Senior-Java-Reviewer. Prüfe folgenden Diff strikt auf Security-Probleme, gib Findings nach Priorität aus."

**Context-Engineering** = **was** das Modell überhaupt sehen darf/soll (Dateien, Regeln, Tools, Historie)
> Statt Regeln in jedem Prompt zu wiederholen: einmal in `AGENTS.md` hinterlegen → jede Session lädt sie automatisch

**Merksatz:**

> Context Engineering schlägt Prompt Engineering — es ist strukturell wichtiger für wiederholbare, gute Ergebnisse.

---

## Konzept 5 — Project Instructions

`AGENTS.md` / `CLAUDE.md` / `copilot-instructions.md`: Regeln **einmal** definieren statt in jedem Prompt zu wiederholen.

| Ebene | Datei |
|---|---|
| User (global) | `~/.copilot/copilot-instructions.md` |
| Projekt | `.github/copilot-instructions.md`, `AGENTS.md`, `CLAUDE.md` |

```markdown
# AGENTS.md
## Grundregeln
- Starte bei unklaren Anforderungen mit Rückfragen oder einem Plan.
- Keine Secrets, .env-Dateien oder produktiven Daten lesen/ausgeben.
## Engineering-Standards
- Halte bestehende Architektur- und Modulgrenzen ein.
- Ergänze/aktualisiere Tests für jede fachliche Änderung.
```

---

## Konzept 6 — Skills (wiederverwendbare Mini-Prozesse)

Ein **Skill** = ein Ordner mit `SKILL.md` (Name, Beschreibung, Anleitung). Er wird automatisch getriggert, wenn seine Beschreibung zur Aufgabe passt — oder manuell per `/skill-name`.

```markdown
---
name: raiffeisen-code-review
description: Prüft Code auf Qualität, Tests, Security und
  Raiffeisen-Kontext. Verwenden, wenn ein PR reviewed werden soll.
allowed-tools: Read, Grep, Glob
---
Führe ein kurzes, strenges Code Review durch. Prüfe insbesondere:
- keine Secrets oder produktiven Daten
- Tests und Quality Gates vorhanden
- klare Modulgrenzen
Berichte Findings nach Priorität: Kritisch / Warnung / Vorschlag.
```

Drei Typen: **Knowledge**-Skills (Fachwissen), **Workflow**-Skills (wiederkehrende Abläufe), **Tool**-Skills (wie ein CLI korrekt genutzt wird).

---

## Konzept 7 — Subagents

Ein **Subagent** ist ein eigener Agent mit **eigenem, isoliertem Context-Fenster**. Nur sein **Endergebnis** fliesst zurück in die Hauptkonversation — das spart Tokens und hält den Hauptkontext sauber.

**Beispiel:**
Ein `log-analyzer`-Subagent liest ein 5’000-Zeilen-Build-Log und meldet der Hauptsession nur:
> „3 Fehler gefunden: NullPointerException in `OrderMapper:42`, vermutlich fehlender Null-Check. 2 Warnungen (deprecated API)."

Typische Einsatzgebiete: Code Review, Log-Analyse, Recherche — parallel zur eigentlichen Hauptaufgabe.

---

## Konzept 8 — Multi-Agent / Orchestrierung

Mehrere (Sub-)Agenten arbeiten **parallel oder in Ketten** an Teilaufgaben.

**Beispiel:**
Ein „Planner"-Agent zerlegt ein Feature in Tasks → mehrere „Worker"-Agenten implementieren Tasks parallel → ein „Reviewer"-Agent prüft alle Diffs gegen die Architekturregeln.

**Vorteil:** höherer Durchsatz, weniger Hand-offs
**Risiko:** „Runaway Agents", Endlosschleifen, verlorene Human-Checkpoints, sich fortpflanzende Fehler

→ braucht klare Leitplanken: Ziel/Grenzen/Akzeptanzkriterien vorgeben statt den exakten Weg zu diktieren.

---

## Konzept 9 — MCP (Model Context Protocol)

Offener Standard von Anthropic: verbindet Agenten mit **externen Systemen** (GitHub, Jira, Datenbanken, …).

```
Host (Copilot/Claude) → MCP-Client ──protokoll──► MCP-Server ──► externes System
```

**Beispiel:** Ein GitHub-MCP-Server erlaubt dem Agenten, Issues zu lesen und PRs zu erstellen — ohne dass jemand eine Custom-Integration bauen muss.

> ⚠️ MCP-Server sind ein reales Sicherheitsrisiko (Prompt Injection, Tool-Missbrauch). Nur von Raiffeisen freigegebene MCP-Server verwenden — nie blind installieren.

---

## Risiken & typische Entwickler-Sorgen

- **„Almost right"-Frust** — Output sieht plausibel aus, braucht aber Nacharbeit
- **Debugging-Aufwand** — schneller generierter Code ≠ schneller ausgelieferter Code
- **Quality Drift** — ohne Architektur-/Testleitplanken entstehen technische Schulden
- **Data Leakage** — Prompts, Logs, Screenshots können geschützte Infos enthalten
- **Kosteneskalation** — lange Sessions & grosse Context Windows verbrauchen Tokens

**Mitigation:** in Ask/Plan-Modus starten, Autopilot nur für klar begrenzte Low-Risk-Tasks, Diffs immer vor Merge reviewen.

---

## Spec-Driven Development — kurz

Nicht „prompt bis's passt", sondern strukturiert:

```
0. Feature & Datenklassifizierung klären
1. Kleine Spec mit Akzeptanzkriterien schreiben
2. KI nach Rückfragen/Edge-Cases fragen lassen
3. Plan freigeben, bevor Dateien geändert werden
4. In kleinen Schritten implementieren
5. Mit Tests, Review und Security-Checks verifizieren
```

**Loop:** Spec → Review → Plan → Implement → Verify
Die Spec überlebt Context-Resets — neue Sessions starten von ihr, nicht von einer vagen Erinnerung an den Chat-Verlauf.

---

## Nächste Schritte

- ✅ AI-Readiness-Score pro Repo erheben (Checkliste von vorhin)
- ✅ `AGENTS.md`/`CLAUDE.md` in aktiven Repos anlegen
- ✅ Erstes `SKILL.md`-Template im Team-Space ablegen
- ✅ Guardrails definieren: Team-Standard für `permissions-config.json`

**Demnächst geplant:** Context-Engineering-Vertiefung, Token-Effizienz, MCP-Architektur & eigene MCP-Server, Multi-Agent-Orchestrierungsmuster, Prompt-Injection & Agent-Sandboxing.

---

<!-- _class: lead -->

# Merksatz

> Der AI-Agent ist so gut wie dein Repo, deine Tests und dein Context.
> Alles andere ist Marketing.

### Fragen & Diskussion

---

## Quellen

- ti&m — *Agentic Coding for Software Engineers*, Kurs 2026-07-22
- Raiffeisen — *AI Coding Workshop @ Raiffeisen* (Governance, Readiness, Tooling)
- Anthropic — Model Context Protocol (modelcontextprotocol.io), Claude Code Docs
- GitHub Copilot CLI Dokumentation
