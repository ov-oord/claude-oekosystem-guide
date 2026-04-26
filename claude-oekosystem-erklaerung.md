# Das Claude Ökosystem — Einsteiger-Leitfaden

Ein Überblick über die 6 Kernkonzepte von Claude (Anthropic), erklärt für Anfänger: Was ist was, wozu dient es, und wie hängt alles zusammen?

---

## 1. Skills — On-Demand Verhalten

**Was sind Skills?**
Skills sind Markdown-Dateien (`.md`), die Claude mit speziellen Anweisungen für bestimmte Aufgaben ausstatten. Du kannst dir Skills wie "Persönlichkeits-Erweiterungen" vorstellen — sie verändern, wie Claude auf bestimmte Anfragen reagiert.

**Wie funktionieren sie?**
- Du legst eine `.md`-Datei mit Anweisungen an (z. B. `brainstorming.md`)
- Claude erkennt den passenden Skill entweder automatisch (`auto-detect`) oder durch einen Trigger-Begriff (z. B. `/brainstorming`)
- Es gibt **globale Skills** (für alle Projekte, z. B. via Superpowers-Plugin mit 12 Skills) und **Projekt-Skills** (im Ordner `SKILLS/`, nur für ein Projekt)
- Vault-Kopien landen unter `claude-assets/skills/`

**Beispiel aus der Praxis:**
Du hast die Skills `brainstorming`, `debugging` und `TDD` (Test-Driven Development) aktiv. Wenn du Claude sagst "Lass uns brainstormen", aktiviert er automatisch den passenden Skill und verhält sich entsprechend.

---

## 2. Subagents — Spezialisierte Helfer

**Was sind Subagents?**
Subagents sind isolierte KI-Instanzen mit eigenem Kontext. Sie sind wie "Assistenten deines Assistenten" — Claude kann mehrere Subagents parallel laufen lassen, ohne dass sie sich gegenseitig stören.

**Warum sind sie nützlich?**
- Sie verhindern **Context-Bloat** (= der Hauptchat wird nicht mit Zwischenarbeit "vollgemüllt")
- Du kannst bis zu **10 parallel** laufen lassen (Stand 2026)
- Verschiedene Typen: `Explore` (Recherche), `Plan` (Planung), `General` (allgemein), `Code-Reviewer` (Code prüfen)

**Beispiel aus der Praxis:**
Du willst einen Vault-Scan (Obsidian-Dateien durchsuchen) durchführen. Statt den Hauptchat zu überfluten, starte einen Subagent, der das im Hintergrund erledigt. Das Ergebnis kommt sauber zurück.

> 💡 **Aktiviert durch:** Das `superpowers`-Skill-Plugin

---

## 3. Hooks — Automatische Aktionen

**Was sind Hooks?**
Hooks sind Shell-Skripte, die bei bestimmten Events **automatisch ausgeführt** werden — ohne dass du etwas tippen musst.

**Wie funktionieren sie?**
Hooks lauschen auf Events wie:
- `PreToolUse` — Bevor Claude ein Tool benutzt
- `PostToolUse` — Nachdem Claude ein Tool benutzt hat
- `SessionStart` — Wenn eine neue Session startet

Die Konfiguration erfolgt in `.claude/settings.local.json`.

**Beispiel aus der Praxis:**
Du hast einen Hook, der nach jedem Schreib-/Editiervorgang automatisch das Skript `fix_frontmatter.py` ausführt. Dieses Skript korrigiert Umlaute (ä, ö, ü) im Frontmatter deiner Markdown-Dateien — vollautomatisch, jedes Mal.

> 💡 **Vorteil:** Wiederkehrende Aufgaben passieren im Hintergrund, ohne dass du daran denken musst.

---

## 4. Plugins — Pakete

**Was sind Plugins?**
Plugins sind Bündel aus Skills + Agents + Commands. Sie sind also "Pakete", die mehrere Funktionen auf einmal mitbringen — ähnlich wie Apps auf einem Smartphone.

**Aktive Beispiele:**
| Plugin | Status | Funktion |
|--------|--------|----------|
| `superpowers v5.0.7` | Aktiv | Globale Skills und Subagent-Fähigkeiten |
| `skill-creator` | Aktiv | Neue Skills erstellen und verwalten |
| `optibot` | Geplant (installiert) | Optimierungsaufgaben |

**Installation:** Global via `settings.json`

---

## 5. MCP — Externe Tools

**Was ist MCP?**
MCP steht für **Model Context Protocol** — ein offenes Protokoll, das Claude mit externen APIs und Diensten verbindet. Ohne MCP ist Claude "abgeschlossen"; mit MCP kann er echte externe Aktionen ausführen.

**Konfigurierte MCPs (Beispiel):**
- **Notion MCP** — Direktzugriff auf Notion-Seiten und Datenbanken (permission erlaubt)
- **Tavily** — Web-Suche direkt aus Claude heraus
- **Google Drive** — Zugriff auf Dateien in Drive
- ⚠️ Notion wurde teilweise abgelöst → MCP-Einträge evtl. bereinigen

**Warum ist das wichtig?**
MCP ist der Unterschied zwischen einem Chatbot und einem echten KI-Agenten. Ohne MCP kann Claude nur reden; mit MCP kann er z. B. eine Notion-Seite aktualisieren, eine Webseite abrufen oder eine GitHub-Datei pushen.

---

## 6. Memory — Session-Übergreifend

**Was ist Memory?**
Memory ist ein persistentes Notizsystem — Claude kann Informationen über mehrere Sessions hinweg speichern und wieder abrufen. Ohne Memory "vergisst" Claude nach jeder Session alles.

**Wo wird gespeichert?**
```
~/.claude/projects/[hash]/memory/
```

**Typen von Memory-Einträgen:**
- `user` — Informationen über dich (Präferenzen, Kontext)
- `feedback` — Feedback aus vergangenen Sessions
- `project` — Projektspezifische Infos
- `reference` — Referenzmaterial

**In der Praxis:**
Das OvidCoord-Projekt hat bereits **30+ Memory-Einträge** aktiv. Diese werden automatisch bei jedem Session-Start geladen, sodass Claude sofort "im Bilde" ist.

---

## Zusammenspiel der 6 Konzepte

```
┌─────────────────────────────────────────────────────────┐
│                    CLAUDE SESSION                       │
│                                                         │
│  Skills (Verhalten)   ←→   Plugins (Pakete)            │
│       ↓                         ↓                       │
│  Subagents (parallele Arbeit)                          │
│       ↓                                                 │
│  Hooks (Automatisierung)   MCP (externe Tools)         │
│                                  ↓                      │
│             Memory (Session-übergreifend)               │
└─────────────────────────────────────────────────────────┘
```

| Konzept | Analogie | Hauptvorteil |
|---------|----------|--------------|
| Skills | Persönlichkeitsmodus | Spezialisiertes Verhalten |
| Subagents | Assistenten | Parallele Arbeit ohne Chaos |
| Hooks | Automatische Regeln | Hintergrund-Automatisierung |
| Plugins | App-Pakete | Alles auf einmal installieren |
| MCP | Schnittstellen | Echte externe Aktionen |
| Memory | Langzeitgedächtnis | Kontext über Sessions hinweg |

---

## Empfohlene Lernreihenfolge für Anfänger

1. **Skills** — Einfachster Einstieg, sofort sichtbarer Effekt
2. **Memory** — Verstehen, wie Claude "lernt"
3. **MCP** — Wenn du externe Dienste anbinden willst
4. **Hooks** — Wenn du Automatisierung brauchst
5. **Subagents** — Für komplexe, parallele Aufgaben
6. **Plugins** — Wenn du alles kombinieren willst

---

*Dokument basiert auf dem Claude Ökosystem Map (OvidCoord, 25.04.26)*
