
# Benutzerhandbuch mit Quellcode und Playwright erstellen

Diese Anleitung beschreibt einen Arbeitsablauf, mit dem ein ausführliches, bebildertes Benutzerhandbuch für eine Webanwendung erstellt und nach einem Release effizient aktualisiert werden kann.

Der zentrale Gedanke lautet:

> Quellcode, automatisierte Browser-Tests, Screenshots und Markdown gehören in einen gemeinsamen Arbeitsbereich. Dadurch kann die KI die Anwendung, ihre sichtbaren Zustände und die Beschreibung gemeinsam auswerten.

Der Ablauf funktioniert besonders gut in VS Code mit einem Projektordner, den zugehörigen Playwright-Tests und einer Markdown-Dokumentation.

## 1. Warum VS Code und Markdown?

### VS Code als gemeinsamer Arbeitsbereich

In VS Code liegen Quellcode, Tests, Screenshots und Dokumentation direkt nebeneinander. Eine KI kann dadurch gezielt auf die Dateien zugreifen, die für eine Änderung relevant sind.

Zum Beispiel kann sie:

- bestimmte Zeilen einer Markdown-Datei korrigieren oder aktualisieren,
- den Quellcode der zugehörigen Funktion prüfen,
- vorhandene Tests und Screenshots lesen,
- erkennen, ob eine beschriebene Funktion noch zum aktuellen Verhalten passt,
- neue Abschnitte auf Grundlage eines neuen Releases ergänzen.

Das ist übersichtlicher, als Quellcode, Bilder und Dokumentation in voneinander getrennten Programmen zu pflegen.

### Markdown als Hauptformat

Markdown ist als Arbeitsformat besonders geeignet, weil es:

- als reiner Text leicht von Menschen und KI gelesen werden kann,
- Überschriften, Listen, Tabellen und Bilder unterstützt,
- mit Git versioniert und verglichen werden kann,
- in viele andere Formate umgewandelt werden kann,
- in Dokumentationssysteme wie Docusaurus integriert werden kann.

Markdown ist dabei das Arbeitsformat. Für die Veröffentlichung kann daraus später eine Webseite, PDF-Datei, Word-Datei oder SharePoint-Seite entstehen.

## 2. Warum sollte der Quellcode einbezogen werden?

Ein Benutzerhandbuch soll nicht nur die offensichtlichen Funktionen beschreiben. Es sollte auch weniger auffällige Bereiche abdecken, zum Beispiel:

- Passwort ändern,
- Benutzer abmelden,
- Berechtigungen und Rollen,
- Fehlermeldungen,
- leere Zustände,
- Pflichtfelder und Validierungen,
- Speichern, Abbrechen und Zurück-Navigation,
- Filter, Sortierung und Suche,
- Verhalten nach einem Seiten-Refresh.

Menschen übersehen solche Ecken leicht, weil manche Abläufe selbstverständlich wirken. Der Quellcode zeigt dagegen, welche Funktionen tatsächlich implementiert sind und welche Zustände die Anwendung unterscheiden kann.

Die KI sollte den Quellcode jedoch nicht als alleinige Wahrheit für die Benutzeroberfläche verwenden. Entscheidend ist die Kombination aus:

1. implementierter Funktion im Quellcode,
2. automatisiertem Testablauf,
3. sichtbarem Ergebnis im Browser,
4. fachlicher Prüfung durch einen Menschen.

## 3. Warum Playwright?

Playwright automatisiert einen echten Browser. Ein Test kann dadurch denselben Ablauf ausführen wie ein Benutzer:

1. Webseite öffnen,
2. Eingaben machen,
3. Schaltflächen anklicken,
4. Ergebnisse prüfen,
5. Screenshots aufnehmen.

Ein vorhandener Playwright-Test ist deshalb gleichzeitig:

- ein Regressionstest,
- eine präzise Beschreibung eines Benutzerablaufs,
- eine wiederholbare Quelle für Screenshots,
- eine gute Grundlage für die Dokumentation.

Besonders hilfreich ist, dass Screenshots nach einem neuen Release erneut erzeugt werden können. Wird der Test angepasst und wieder ausgeführt, lassen sich die Bilder und die zugehörige Markdown-Datei anschließend aktualisieren.


### Vorhandene Tests sicher verwenden

Befindet sich im Quellcode-Ordner bereits ein `tests`-Ordner, sollten die vorhandenen Testdateien zuerst in einen separaten Arbeitsordner kopiert werden. Die Originaldateien des Entwicklers sollten sicherheitshalber nicht direkt verändert werden.

Danach kann die Kopie für die Dokumentation angepasst werden, zum Beispiel indem an den wichtigen Schritten Screenshots ergänzt werden. So bleibt der ursprüngliche Test unverändert und kann weiterhin vom Entwicklungsteam verwendet werden.

### Neue Tests mit KI erstellen lassen

Wenn noch keine Playwright-Tests vorhanden sind, kann die KI darum gebeten werden, Tests für die gesamte Webseite zu erstellen. Der Auftrag sollte ausdrücklich festlegen, dass nicht nur eine einzelne Funktion geprüft wird, sondern alle wichtigen Seiten, Benutzerrollen, Eingaben, erfolgreichen Abläufe und Fehlerfälle.

Ein geeigneter Prompt lautet:

```text
Analysiere den gesamten Quellcode und erstelle vollständige Playwright-Tests für
die komplette Webseite. Prüfe alle wichtigen Seiten und Funktionen aus Sicht eines
Benutzers, einschließlich Navigation, Formulare, Speichern, Bearbeiten, Löschen,
Abmelden, Berechtigungen, Validierungen und Fehlermeldungen.

Erstelle für jeden wichtigen Benutzerablauf einen klaren Test. Führe die Schritte
in der richtigen Reihenfolge aus und prüfe nach jeder wichtigen Aktion das sichtbare
Ergebnis. Nimm an den für ein Benutzerhandbuch relevanten Zuständen Screenshots auf,
zum Beispiel vor der Eingabe, nach dem Speichern, bei Fehlermeldungen und nach einer
erfolgreichen Änderung. Verwende aussagekräftige, nummerierte Dateinamen und speichere
die Screenshots im vorgesehenen Dokumentationsordner.

Arbeite zunächst mit einer Kopie vorhandener Testdateien und verändere keine
Originaltests direkt. Weise auf Funktionen hin, die wegen fehlender Testdaten,
Anmeldung oder Berechtigungen manuell geprüft werden müssen.
```

Die von der KI erstellten Tests sollten anschließend fachlich geprüft und bei Bedarf angepasst werden. Erst danach sollten sie ausgeführt und für die Dokumentation verwendet werden.

## 4. Empfohlene Projektstruktur

Eine mögliche Struktur sieht so aus:

```text
react-todo-app/
├── src/                         # Quellcode der Webanwendung
├── tests/                       # Playwright-Tests
│   └── aufgabe-anlegen.spec.js
├── docs-site/                   # Markdown-Dokumentation
│   ├── intro.md
│   ├── Benutzerhandbuch-mit-Quellcode-und-Playwright.md
│   └── static/img/              # erzeugte Screenshots
├── playwright.config.js         # Playwright-Konfiguration
├── package.json                 # Befehle und Abhängigkeiten
└── README.md
```

Screenshots sollten einen nachvollziehbaren Namen bekommen, zum Beispiel:

```text
01-startseite-leere-aufgabenliste.png
02-erste-aufgabe-eingegeben.png
03-erste-aufgabe-hinzugefuegt.png
```

Die Nummerierung hält die Reihenfolge im Test und im Handbuch sichtbar zusammen.

## 5. Voraussetzungen installieren

### Node.js prüfen

Für Playwright wird eine aktuelle Node.js-Version benötigt. In PowerShell kann geprüft werden, ob Node.js und npm vorhanden sind:

```powershell
node --version
npm --version
```

Falls einer der Befehle nicht gefunden wird, muss Node.js zuerst von der offiziellen Node.js-Webseite installiert werden.

### Projekt öffnen

In PowerShell in den Projektordner wechseln:

```powershell
cd C:\projekte\quellcodetodocu\react-todo-app
```

### Abhängigkeiten installieren

Wenn bereits eine `package.json` vorhanden ist, werden die Projektabhängigkeiten mit folgendem Befehl installiert:

```powershell
npm install
```

### Playwright-Browser installieren

Die Playwright-Bibliothek allein reicht nicht aus. Zusätzlich müssen die Browser-Binärdateien installiert werden:

```powershell
npx playwright install
```


## 6. Playwright-Test ausführen

### Alle Tests ausführen

```powershell
npx playwright test
```

### Einen einzelnen Test ausführen

```powershell
npx playwright test tests/aufgabe-anlegen.spec.js
```

### Test mit sichtbarem Browser ausführen

Während der Entwicklung ist der sichtbare Modus hilfreich:

```powershell
npx playwright test tests/aufgabe-anlegen.spec.js --headed
```

### Einen Test langsam verfolgen

```powershell
npx playwright test tests/aufgabe-anlegen.spec.js --debug
```

### HTML-Testbericht öffnen

Nach dem Testlauf kann der Bericht geöffnet werden:

```powershell
npx playwright show-report
```

Im Bericht sind unter anderem Testschritte, Anhänge, **Screenshots, Videos von diesem Testflow** und bei Fehlern Traces sichtbar.

## 7. Tests für ein Benutzerhandbuch schreiben

Ein Dokumentations-Test sollte wichtige sichtbare Zustände abdecken. Für jeden wichtigen Zustand sollte geprüft werden, dass die erwarteten Texte und Steuerelemente vorhanden sind.

Ein vereinfachtes Beispiel:

```js
import { test, expect } from "@playwright/test";

test("Aufgabe anlegen", async ({ page }) => {
  await page.goto("/");

  await expect(
    page.getByText("No todos yet. Add one above!")
  ).toBeVisible();

  await page.getByRole("textbox", {
    name: "What needs to be done?",
  }).fill("Monatsbericht erstellen");

  await page.screenshot({
    path: "docs-site/static/img/02-erste-aufgabe-eingegeben.png",
    fullPage: true,
  });

  await page.getByRole("button", {
    name: "Add",
    exact: true,
  }).click();

  await expect(
    page.getByText("Monatsbericht erstellen", { exact: true })
  ).toBeVisible();

  await page.screenshot({
    path: "docs-site/static/img/03-erste-aufgabe-hinzugefuegt.png",
    fullPage: true,
  });
});
```

### Gute Testschritte

Ein guter Test benennt die fachliche Handlung und prüft das sichtbare Ergebnis:

- leere Startseite anzeigen,
- Formular ausfüllen,
- Eingabe validieren,
- Datensatz speichern,
- Erfolgsmeldung oder neuen Eintrag prüfen,
- Eintrag bearbeiten,
- Eintrag als erledigt markieren,
- Eintrag löschen,
- Fehlerfall prüfen.

Screenshots sollten an den Stellen entstehen, die ein Benutzerhandbuch erklären soll. Ein Screenshot nach jedem einzelnen Klick ist nicht automatisch sinnvoll. Wichtiger sind die Zustände, die eine Entscheidung oder einen sichtbaren Unterschied zeigen.

## 8. Webseiten mit Anmeldung und Berechtigungen

Wenn die Webseite eine Anmeldung benötigt, sollte für automatisierte Tests ein eigenes Testkonto verwendet werden. Das Konto sollte nur die Rechte besitzen, die der jeweilige Test benötigt.

Die Zugangsdaten gehören nicht in den Quellcode und nicht in Git. Sie können zum Beispiel als Umgebungsvariablen gesetzt werden:

```powershell
$env:TEST_USERNAME = "testkonto@example.com"
$env:TEST_PASSWORD = "Passwort-nicht-in-Git-speichern"
```

Ein Login-Test kann anschließend eine Session speichern:

```js
import { test as setup, expect } from "@playwright/test";

setup("Login durchführen", async ({ page }) => {
  await page.goto("/login");

  await page.getByLabel("E-Mail").fill(process.env.TEST_USERNAME);
  await page.getByLabel("Passwort").fill(process.env.TEST_PASSWORD);
  await page.getByRole("button", { name: "Anmelden" }).click();

  await expect(page).toHaveURL(/dashboard/);
  await page.context().storageState({
    path: "playwright/.auth/user.json",
  });
});
```

Die gespeicherte Session kann von nachfolgenden Tests verwendet werden. Der Ordner `playwright/.auth/` muss in `.gitignore` eingetragen werden.

Für eine vollständige Berechtigungsprüfung sind mindestens diese Fälle sinnvoll:

- nicht angemeldeter Benutzer wird zur Anmeldung weitergeleitet,
- berechtigter Benutzer kann die Funktion verwenden,
- Benutzer ohne erforderliche Rolle erhält eine verständliche Fehlermeldung,
- Administrator sieht zusätzliche Funktionen,
- Abmelden beendet die geschützte Session.

Bei MFA, CAPTCHA oder externem Single Sign-on sollte eine vom Unternehmen unterstützte Testumgebung oder ein Test-Identity-Provider verwendet werden. Zugangsdaten und Sicherheitsmechanismen dürfen nicht in einer Dokumentation veröffentlicht werden.

## 9. Screenshots für die KI auswertbar machen

Screenshots helfen der KI, sichtbare Details zu erkennen, die aus dem Quellcode allein nicht sicher hervorgehen. Die Bilder sollten deshalb:

- aussagekräftige Dateinamen besitzen,
- nach Möglichkeit keine echten personenbezogenen Daten enthalten.

Ein geeigneter Prompt für die Auswertung lautet zum Beispiel:

```text
Lies die vorhandenen Markdown-Dateien, den relevanten Quellcode, die Playwright-Tests
und alle zugehörigen Screenshots.

Aktualisiere das bestehende Benutzerhandbuch vollständig und fachlich korrekt.
Beschreibe bei jedem Screenshot, welche Seite und welcher Zustand zu sehen sind.
Fokussiere besonders auf sichtbare Beschriftungen, Eingabefelder, Schaltflächen,
Meldungen, Statusanzeigen und die Reihenfolge der Benutzerhandlungen.

Ergänze fehlende Voraussetzungen, genaue Schritte, erwartete Ergebnisse,
Fehlerfälle und Hinweise zu Berechtigungen. Verwende klare Überschriften,
nummerierte Handlungsanweisungen und verständliche Formulierungen für neue Benutzer.
Verwende keine Behauptung, die weder im Quellcode, im Test, im Screenshot noch
in einer fachlichen Vorgabe belegt ist. Markiere offene Punkte zur manuellen Prüfung.
Behalte die vorhandenen Bildpfade bei oder aktualisiere sie nur, wenn die Dateien
wirklich umbenannt wurden.
```

Die KI sollte anschließend immer durch einen Menschen geprüft werden. Ein Screenshot beweist, was in genau diesem Zustand sichtbar war, aber nicht automatisch jede fachliche Regel der Anwendung.

## 10. Nach einem neuen Release aktualisieren

Nach einer Änderung an der Webanwendung empfiehlt sich dieser Ablauf:

1. Betroffene Playwright-Tests anpassen.
2. Tests ausführen und neue Screenshots erzeugen.
3. Fehlgeschlagene Tests fachlich prüfen.
4. Markdown mit Quellcode, Tests und neuen Bildern aktualisieren.
5. Handbuch im Browser gegen die aktuelle Anwendung lesen.
6. Nur geprüfte Änderungen nach SharePoint veröffentlichen.

Der Nutzen von Playwright liegt darin, dass der Ablauf wiederholbar ist. Nach der Anpassung des Tests können die Screenshots mit demselben Befehl erneut erstellt werden:

```powershell
npx playwright test tests/aufgabe-anlegen.spec.js
```

Damit sinkt das Risiko, dass das Handbuch nach einem Release alte Bildschirmzustände zeigt.

## 11. Vorgehen ohne Quellcode

Ein Handbuch kann auch ohne Zugriff auf den Quellcode erstellt oder aktualisiert werden. Der Ablauf ist dann etwas aufwendiger:

1. Alle wichtigen Benutzerabläufe manuell durchspielen.
2. Von den relevanten Zuständen Screenshots aufnehmen.
3. Screenshots nach Funktion und Reihenfolge benennen.
4. Ein vorhandenes Handbuch in Markdown übertragen.
5. Falls kein Handbuch existiert, zuerst eine fachlich geprüfte Grundstruktur schreiben.
6. Die Markdown-Datei, Screenshots und vorhandenen fachlichen Unterlagen gemeinsam in VS Code öffnen.
7. Die KI mit einem klaren Aktualisierungsauftrag arbeiten lassen.
8. Alle Beschreibungen manuell gegen die echte Webseite prüfen.

Ein geeigneter Prompt lautet:

```text
Du bist Experte für die Erstellung von Benutzerhandbüchern.
Vervollständige und aktualisiere die bestehende Markdown-Datei anhand der
beigefügten Screenshots und fachlichen Unterlagen.

Lies jeden Screenshot genau. Beschreibe, welche Seite, Überschrift, Schaltflächen,
Eingabefelder, Meldungen und Zustände sichtbar sind. Fokussiere besonders auf die
Texte in den Bildern und beschreibe die Benutzerhandlungen in der richtigen Reihenfolge.

Mache die Anweisungen genauer und für neue Benutzer leicht verständlich.
Ergänze Voraussetzungen, erwartete Ergebnisse, Hinweise, Fehlerfälle und offene
Fragen. Erfinde keine Funktionen, die in den Bildern oder Unterlagen nicht belegt sind.
Markiere Stellen, die ein Fachanwender prüfen muss.
```

Ohne Quellcode kann die KI nicht zuverlässig feststellen, ob eine Funktion fehlt, nur unter bestimmten Rollen erscheint oder im Hintergrund anders arbeitet. Deshalb ist die fachliche Prüfung hier besonders wichtig.

## 12. Markdown nach SharePoint übertragen

In SharePoint kann ein Markdown-Webpart eingefügt werden. Dazu wird der Inhalt der fertigen `.md`-Datei direkt kopiert und in das Markdown-Webpart eingefügt.

Vorgehen:

1. Die Markdown-Datei lokal fertigstellen und fachlich prüfen.
2. In SharePoint eine Seite öffnen oder eine neue Seite erstellen.
3. Ein Markdown-Webpart einfügen.
4. Den Inhalt der `.md`-Datei kopieren und in das Webpart einfügen.
5. Die Darstellung prüfen und die SharePoint-Seite veröffentlichen.

Screenshots müssen nicht zwingend übernommen werden. Wenn das Handbuch auch ohne Bilder verständlich ist, kann man auf sie verzichten. Wenn Screenshots benötigt werden, müssen sie zusätzlich als Bild-Webparts eingefügt oder an einem für SharePoint erreichbaren Speicherort abgelegt werden. Lokale Pfade wie `./static/img/bild.png` funktionieren für andere SharePoint-Benutzer nicht automatisch.

## 13. Qualitätsprüfung vor der Veröffentlichung

Vor der Veröffentlichung sollten diese Fragen beantwortet werden:

- Bezieht sich jeder Schritt auf die aktuelle Version der Anwendung?
- Stimmen sichtbare Texte und Schaltflächen mit der Webseite überein?
- Sind alle wichtigen Funktionen, einschließlich weniger offensichtlicher Funktionen, enthalten?
- Sind Screenshots lesbar und frei von echten Zugangsdaten oder personenbezogenen Daten?
- Sind Rollen und Berechtigungen verständlich beschrieben?
- Funktionieren alle Bildlinks?
- Wurde das Handbuch mit einem Benutzerkonto ohne Sonderrechte geprüft?
- Sind offene Annahmen und fachlich ungeklärte Punkte markiert?
- Wurde die veröffentlichte SharePoint-Seite nach dem Einfügen noch einmal getestet?

## Kurzfassung

Der effizienteste Arbeitsablauf ist:

1. Quellcode, Tests, Screenshots und Markdown in einem VS-Code-Projekt sammeln.
2. Playwright-Tests für wichtige Benutzerabläufe schreiben oder anpassen.
3. Während der Tests Screenshots der entscheidenden Zustände erzeugen.
4. Die KI mit Quellcode, Tests, Screenshots und einem präzisen Prompt arbeiten lassen.
5. Das Ergebnis fachlich prüfen und in Markdown pflegen.
6. Nach jedem Release Tests und Screenshots erneut ausführen.
7. Die geprüfte Markdown-Dokumentation in SharePoint übertragen oder als Dokument bereitstellen.

So wird das Benutzerhandbuch zu einem wiederholbaren Teil des Entwicklungs- und Releaseprozesses anstatt zu einer einmalig erstellten Datei, die schnell veraltet.
