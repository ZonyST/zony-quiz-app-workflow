# 🚀 Übung 2: CI/CD Pipeline – Quality Gates & Docker Automatisierung

In der letzten Stunde habt ihr erfolgreich Container lokal gebaut und mit `compose` vernetzt. 
Heute machen wir den Schritt zum echten Software-Engineering: **Automatisierung!**

Stellt euch vor, ihr arbeitet im Team. Bevor ein neues Docker Image für die Produktion gebaut wird, muss sichergestellt sein, dass der Code keinen Müll enthält und alle Tests bestanden hat.

**Eure Mission:** Wir bauen einen GitHub Actions Workflow, der als "Türsteher" (Quality Gate) fungiert.
1. Bei **jedem Push** (egal auf welchem Branch) werden Code-Checks (Linting) und Tests ausgeführt.
2. **Nur wenn hierbei alles grün ist** UND wir auf dem `main` Branch sind, baut der Server die Docker Images und pusht sie in eure GitHub Container Registry (GHCR).

---

## 🛠️ Voraussetzungen

- Euer Code liegt in einem **öffentlichen (Public)** Repository auf GitHub. *(Das macht den Umgang mit der Registry für den Anfang viel einfacher).*
- Ihr habt einen Code-Editor (z.B. VS Code) und ein Terminal offen.

---

## 🧠 Vorab: Klärende Fragen (Bitte lesen & verstehen!)

Bevor ihr das Workflow-File schreibt, macht euch kurz Gedanken:

1. **Quality Gates:** Warum lassen wir die Tests auf *jedem* Branch laufen, bauen die Docker Images aber *nur* auf dem `main` Branch?
2. **Abhängigkeiten (Needs):** Unser Workflow hat zwei Jobs: `quality` und `docker`. Was passiert mit dem `docker`-Job, wenn der `quality`-Job fehlschlägt? Wie sagen wir GitHub, dass Job B auf Job A warten muss?
3. **Versionierung (Tags):** Wir laden das fertige Image mit zwei Tags hoch: `latest` und einem `short_sha` (z.B. `a1b2c3d4`). Warum ist der `short_sha` (die ID des Git-Commits) in der echten Welt überlebenswichtig, wenn nachts um 3 Uhr die Server brennen?

---

## 🏃‍♂️ Ablauf & Eure Aufgabe

Wir haben euch ein Grundgerüst (Template) vorbereitet. Es liegt unter `.github/workflows/ci-ghcr-template.yml`.

1. **Datei anlegen:** Kopiert den Inhalt des Templates und erstellt eine neue Datei unter `.github/workflows/ci-ghcr.yml`.
2. **Lücken füllen:** Die Datei ist voller `TODO`-Kommentare. Eure Aufgabe ist es, diese durch die richtigen GitHub Actions und Befehle zu ersetzen. (Nutzt die Spickzettel-Links unten!).
3. **Fehler jagen (WICHTIG):** Im Code sind **absichtlich zwei kleine Linter-Fehler** versteckt! Wenn ihr euren Workflow das erste Mal pusht, *wird* der `quality`-Job rot aufleuchten. Lest die Logs auf GitHub, findet die Fehler im Code, repariert sie und pusht erneut!
4. **Der Test:**
   - Erstellt einen neuen Branch (`git checkout -b test-pipeline`). Pusht etwas. -> *Nur Tests sollten laufen.*
   - Merged oder pusht in den `main` Branch. -> *Tests laufen, danach werden die Docker Images gebaut und in eure GHCR gepusht!*

---

## 📚 Euer Spickzettel (Wichtige Links)

Um die `TODO`s zu lösen, müsst ihr nicht raten. Hier steht genau, wie es geht:

- **Wann soll der Workflow starten (Trigger)?** [GitHub Docs: Events that trigger workflows](https://docs.github.com/de/actions/how-tos/write-workflows/choose-when-workflows-run/trigger-a-workflow)
- **Wie warte ich auf einen anderen Job (`needs`) und wie filtere ich nach dem `main` Branch (`if`)?** [GitHub Docs: Using Jobs](https://docs.github.com/de/actions/how-tos/write-workflows/choose-what-workflows-do/use-jobs)
- **Wie richte ich Node.js ein und installiere Pakete?** [GitHub Docs: Building Node.js](https://docs.github.com/de/actions/tutorials/build-and-test-code/nodejs#installing-dependencies)
- **Wo finde ich vorgefertigte Actions (z.B. checkout)?** [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- **Wie logge ich mich in Docker ein und baue Images?** [GitHub Docs: Publish Docker Images](https://docs.github.com/de/actions/tutorials/publish-packages/publish-docker-images)

---

## ✅ Abnahme: Wann ist die Aufgabe bestanden?

Eure Pipeline ist ein voller Erfolg, wenn:

1. Ein Push auf einen Feature-Branch den Code testet (Lint + Test), aber **keine** Docker Images baut.
2. Der eingebaute Linter-Fehler von euch gefunden und im Code repariert wurde.
3. Ein Push auf den `main` Branch alle Tests besteht **und** erfolgreich in die GHCR pusht.
4. Ihr auf eurer GitHub Repo-Startseite rechts unter **"Packages"** eure fertigen Images (`quiz-web` und `quiz-api`) mit jeweils zwei Tags (`latest` und dem 8-stelligen Git-Hash) sehen könnt.

*Tipp für Profis: Schaut in eure Action Logs! Wenn etwas rot wird, steht die exakte Antwort auf das "Warum" zu 99% in der Fehlermeldung der Konsole.* Viel Erfolg!
