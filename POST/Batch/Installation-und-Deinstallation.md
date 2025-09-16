**6**

## <span id="page-48-0"></span>**Installation und Deinstallation**

Dieses Kapitel beschreibt die Installation von POST Batch. Zur Protokollierung der Installation kann das [Installationsprotokoll](#page-240-1) im Anhang verwendet werden.

## <span id="page-48-1"></span>**6.1 AUSLIEFERUNG**

Sie erhalten POST Batch zum Download in einer ZIP-Datei. Die ZIP-Datei hat folgenden Namen:

#### tpost<Version>.zip

<Version> bezeichnet die aktuelle Version. Der Installer ist nicht plattformspezifisch und kann für folgende Platformen gestartet werden:

- win\_x86\_64
- linux\_x86\_64

Im Folgenden wird das Installationsverzeichnis auch als *TOLERANT\_HOME* Verzeichnis bezeichnet.

Zur vollständigen Verwendung von POST Batch wird ab Version 9.0 eine zusätzliche Lizenzdatei benötigt, welche im config-Verzeichnis von *TOLERANT\_HOME* abgelegt werden muss. Je nach Produkttyp wird eine der folgenden Lizenzen benötigt:

- postService.lic
- postBatch.lic

— post.lic (generische Lizenz)

Um eine vollständige Test- oder Produktivlizenz zu erhalten, wenden Sie sich bitte an unseren Support (siehe Kapitel *['Support'](#page-10-0)* (Seite [3\)](#page-10-0)). Der maximale Gültigkeitszeitraum einer so ausgestellten Lizenz beträgt 2 Jahre.

Weitere Informationen zur Lizenzdatei und deren Monitoring siehe Abschnitt *['TOLERANT Lizenz über](#page-155-0)[prüfen'](#page-155-0)* (Seite [148\)](#page-155-0) und Abschnitt *'***??***'* (Seite **??**).

## <span id="page-49-0"></span>**6.2 INSTALLATIONSVORBEREITUNGEN**

Vor der Installation sollten folgende Punkte vorbereitet und abgestimmt sein:

- Zielsystem: Das System/der Rechner, auf dem der Service installiert werden soll
- Installationsverzeichnis: Das Installationsverzeichnis sollte festgelegt sein.
- Port: Der Web-Port und die interne Ports der mitinstallierten Services sollten festgelegt sein
- Firewall: Der Service-Port sollte für HTTP- oder HTTPS-Verkehr zwischen dem Service und den Clients freigeschaltet sein.
- Plattenplatz: Für die Installation werden kurzfristig ca. 2 GB verwendet. Nach der Installation können die ausgelieferte ZIP-Datei und das Verzeichnis installtmp gelöscht werden, so dass der Plattenbedarf um ca. 1 GB zurückgeht.

## <span id="page-49-1"></span>**6.3 INSTALLATION**

Die Installation läuft über ein interaktives Skript. Das Skript fragt während der Installation eventuell einige Installationsparameter ab, wie bspw. Installationspfad oder eine Portnummer. Soll die Installation als sogenannte "Silent Installation", also ohne Benutzerinteraktion ablaufen, so kann das Skript über eine Konfigurationsdatei gesteuert werden (siehe Abschnitt *'SILENT INSTALLATION'* (Seite [44\)](#page-49-1)).

#### INTERAKTIVE INSTALLATION

- *Schritt 1:* Entpacken Sie die gelieferte ZIP-Datei an eine beliebige Stelle (z.B. c:\temp oder /tmp) mit ausreichendem Plattenplatz (ca. 2 GB) auf dem Zielrechner. Sollte der Zielrechner keine Programme zum Entpacken aufweisen, so entpacken Sie die Datei zunächst auf einem Windows System und kopieren dann die entpackten Dateien auf Ihr Zielsystem:
  - install\_\*.\* (Skriptdatei)
  - installer.groovy
  - package
- *Schritt 2:* Starten Sie eine Shell oder einen Kommandoprompt und wechseln in das Verzeichnis, in dem die Skriptdatei liegt.
- *Schritt 3:* Starten Sie die Skriptdatei.
- *Schritt 4:* Das interaktive Installationsprogramm wird gestartet und fragt nach dem Pfad, in dem POST Batch installiert werden soll. Der Pfad muss absolut sein. Sollte der Pfad nicht absolut sein, so wird die Frage wiederholt.
- *Schritt 5:* Anschließend überprüft das Installationsprogramm, ob unter dem angegebenen Pfad bereits POST Batch installiert ist. Ist dies der Fall, so wird nachgefragt, ob ein Upgrade durchgeführt werden soll. Wollen Sie ein Upgrade durchführen, so beantworten Sie die Frage mit "Y" und lesen dazu bitte das Unterkapitel "UPGRADE".
- *Schritt 6:* Wenn Sie die Frage nach einem Upgrade verneint haben oder noch keine Version installiert war, so wird nun nach den HTTP Ports gefragt:
  - Webserver-Port
  - Backend-Port
  - GUI-Port
- *Schritt 7:* Wird die Frage nach SOAP bejaht, so wird der SOAP Service installiert und eingerichtet. Der Installer wird dafür einen freien HTTP-Port brauchen.

*Schritt 8:* Dann fragt der Installer nach der Sicherheitsoption. Folgende Optionen sind vorgesehen:

- none: Security wird abgeschlatet.
- basic: Basic-Authentication wird im Webserver aktiviert.
- keycloak: Keycloak wird installiert und eingerichtet. Die Installation von Keycloak benötigt zwei weitere HTTP-Ports.

*Schritt 9:* Mit der Option keycloak haben Sie die Möglichkeit, HTTPS zu aktivieren.

*Schritt 10:* Wird HTTPS gewünscht, so wird nun nach dem HTTPS-Port und dem Rechnernamen gefragt.

*Schritt 11:* Das Installationsprogramm gibt jetzt eine Zusammenfassung der Installationsparameter aus und fragt, ob diese korrekt sind. Wird die Frage bejaht, so wird die Installation gestartet. Dies kann einige Minuten dauern.

#### UPGRADE

**Achtung:** Aufgrund der umfassenden Neuerungen und Erweiterungen für die Version 12.0 ist ein Upgrade von früheren Versionen auf diese Version nicht möglich! Von der Version 12.0 ab werden für alle zukünftigen Versionen wieder Upgrades wie gewohnt unterstützt.

Wenn bereits eine POST Batch Version installiert ist, können Sie bei Erhalt einer neuen Version auf einfache Weise ein Upgrade durchführen. Dazu starten Sie, wie oben beschrieben, das Installationsskript und geben das Installationsverzeichnis an, in dem die alte Version installiert ist. Das Installationsprogramm stellt dann fest, dass dort bereits eine Installation vorhanden ist.

Eventuell sind vor oder nach dem Upgrade noch einige manuelle Schritte notwendig. Wenn dies der Fall ist, dann gibt der Installer entsprechende Informationen aus und ermöglicht durch eine Rückfrage den Abbruch des Upgrades/der Installation.

Der Installer fragt weiter nach, ob ein Upgrade durchgeführt werden soll. Wenn Sie diese Frage mit "Y" beantworten, so wird das Installationsprogramm in den Verzeichnissen bin, client, doc, install, lib, runtime, examples, internal die neuen Versionen der Dateien auspacken. Die Verzeichnisse config, data, logs werden durch Upgrade nicht(!) verändert, d.h. Ihre Konfigurationen, Referenzdaten und Logdateien werden nicht gelöscht oder verändert.

#### SILENT INSTALLATION

Bei der "Silent Installation" wird das Installationsskript mittels einer Konfigurationsdatei gesteuert. Die Parameter in der Konfigurationsdatei sind in der Tabelle [6.1:](#page-51-0) *['Parameter der Konfigurationsdatei für die](#page-51-0) ["Silent Installation](#page-51-0)*"*'* beschrieben.

Wird in der Konfigurationsdatei der Parameter "DOUPGRADE" gesetzt, so wird versucht, ein Upgrade durchzuführen. Wenn dies nicht möglich ist, wird die Installation abgebrochen. Bei einem Upgrade können keine Installationsparameter verändert werden. Die Parameter für Ports oder Ähnliches werden ignoriert.

Folgende Punkte sind zu beachten:

- Pfade müssen absolut angegeben werden.
- Pfade mit Leerzeichen müssen in Anführungszeichen gesetzt werden.
- <span id="page-51-0"></span>— In Pfaden unter Windows ist der Backslash ('\') als '\\' anzugeben.

| Parameter            | Wertebereich                | Beschreibung                                                                                                                                                                                     |
|----------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TOLERANT_HOME        | Absoluter Pfad              | Pfad, unter dem die Software installiert werden<br>soll.                                                                                                                                         |
| TLCONFIG             | Absoluter Pfad              | Pfad, unter dem die Konfigurationsdateien abge<br>legt                                                                                                                                           |
| TLDATA               | Absoluter Pfad              | Pfad, unter dem die Referenzdaten abgelegt                                                                                                                                                       |
| TLLOGS               | Absoluter Pfad              | Pfad, unter dem die Protokolldateien abgelegt                                                                                                                                                    |
| TLPROTOCOLS          | Absoluter Pfad              | Pfad, unter dem Kurzprotokolle abgelegt                                                                                                                                                          |
| TLTMP                | Absoluter Pfad              | Pfad, unter dem temporäre abgelegt                                                                                                                                                               |
| DOUPGRADE            | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so wird ge<br>Y<br>prüft, ob ein Upgrade überhaupt möglich ist.<br>Falls ja, wird der Upgrade ausgeführt, ansons<br>ten wird die Installation abgebrochen. |
| JRE                  | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so wird die<br>Y<br>JAVA Laufzeitumgebung installiert                                                                                                      |
| BACKEND              | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so wird das<br>Y<br>Backend installiert                                                                                                                    |
| GUI                  | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so wird die<br>Y<br>GUI installiert                                                                                                                        |
| NGINX                | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so wird der<br>Y<br>Webserver (NGINX) installiert                                                                                                          |
| SOAP                 | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so wird der<br>Y<br>SOAP-Service installiert                                                                                                               |
| POPULATE             | Y, N                        | Wird dieser Parameter auf<br>gesetzt, so werden<br>Y<br>Beispielkonfigurationen für den Service in das<br>TLCONFIG Verzeichnis kopiert                                                           |
| SECURITY_OPTION      | none,<br>basic,<br>keycloak | Gibt die gewünschte Sicherheitsoption an                                                                                                                                                         |
| BACKEND_SERVICE_PORT | 165535                      | Der HTTP Port für den Backend-Service                                                                                                                                                            |
| GUI_SERVICE_PORT     | 165535                      | Der HTTP Port für den GUI-Service                                                                                                                                                                |
| SOAP_SERVICE_PORT    | 165535                      | Der HTTP Port für den SOAP-Service                                                                                                                                                               |
| NGINX_PORT           | 165535                      | Der HTTP Port für den Webserver                                                                                                                                                                  |

Tabelle 6.1: Parameter der Konfigurationsdatei für die "Silent Installation"

Hier ein Beispiel für eine Konfigurationsdatei:

```
1 DOUPGRADE =Y
2 BACKEND=Y
3 BACKEND_SERVICE_PORT =8991
4 FHS=N
5 GUI=Y
6 GUI_SERVICE_PORT =8992
7 JRE=Y
8 KC_HTTP_PORT =8994
9 KC_STOP_PORT =8995
10 NGINX=Y
11 NGINX_PORT =8999
12 POPULATE=Y
13 SECURITY_OPTION =none
14 SOAP=N
15 SOAP_SERVICE_PORT =8993
16 TLCONFIG =/ home/tolerant/tpost/config
17 TLDATA =/ home/tolerant/tpost/data
18 TLLOGS =/ home/tolerant/tpost/logs
19 TLPROTOCOLS =/ home/tolerant/tpost/ protocols
20 TLTMP =/ home/tolerant/tpost/tmp
```

21 TOLERANT\_HOME =/ home/tolerant/tpost

Die Installation wird wie folgt vorgenommen:

*Schritt 1:* Entpacken Sie die gelieferte ZIP-Datei an eine beliebige Stelle (z.B. c:\temp oder /tmp) mit ausreichendem Plattenplatz (ca. 2 GBytes) auf dem Zielrechner. Sollte der Zielrechner keine Programme zum Entpacken aufweisen, so entpacken Sie die Datei zunächst auf einem Windows System und kopieren dann die entpackten Dateien auf Ihr Zielsystem:

- install\_\*.\* (Skriptdatei)
- installer.groovy
- package
- *Schritt 2:* Starten Sie eine Shell oder einen Kommandoprompt und wechseln in das Verzeichnis, in der die Skriptdatei install\_\*.\* liegt.
- *Schritt 3:* Kopieren Sie die Konfigurationsdatei in dieses Verzeichnis.

*Schritt 4:* Starten Sie die Skriptdatei mit:

install\_ \*.\* -s <Name der Konfigurationsdatei >

<span id="page-53-0"></span>*Schritt 5:* Die Installation läuft jetzt automatisch ohne Interaktion ab.

## **6.4 DEINSTALLATION**

Zur Deinstallation sind nur wenige Schritte nötig.

*Schritt 1:* Sichern Sie eventuelle Konfigurationsdateien und andere Daten aus den Verzeichnissen config und data.

*Schritt 2:* Löschen Sie das komplette Installationsverzeichnis.

<span id="page-54-0"></span>Die Deinstallation ist nun abgeschlossen.

## **6.5 VERZEICHNISSTRUKTUR**

Nach der Installation erhalten Sie folgende Verzeichnisstruktur:

| Verzeichnis | Beschreibung                                                                     |
|-------------|----------------------------------------------------------------------------------|
| bin         | Skripte                                                                          |
| config      | Konfigurationsdateien                                                            |
| data        | Hier werden Daten abgelegt                                                       |
| doc         | Hier finden Sie die Dokumentation                                                |
| examples    | Hier finden Sie die Beispielkonfigurationen                                      |
| install     | Installationsskripte                                                             |
| internal    | Interne Konfigurationsdateien                                                    |
| lib         | Bibliotheken                                                                     |
| logs        | Logdateien                                                                       |
| protocols   | Hier werden Kurzprotokolldateien abgelegt                                        |
| runtime     | Benötigte Laufzeitkomponenten (Java)                                             |
| static      | Verzeichnis für statische Teile der GUI (bspw. eigene CSS Konfigurationen, etc.) |
| tmp         | Verzeichnis für temporäre Dateien                                                |

<span id="page-54-1"></span>Das tmp-Verzeichnis wird von den laufenden Services verwendet. Vor Löschen des tmp-Verzeichnisses stellen Sie bitte sicher, dass der POST Batch gestoppt ist.

## **6.6 WEITERE SCHRITTE**

Damit der POST Batch Ihre Adressen überprüfen und verbessern kann, sind noch folgende Schritte notwendig:

*Schritt 1:* Referenzdaten einspielen. Dies ist in Kapitel *['Referenzdaten installieren/aktualisieren'](#page-148-1)* (Seite [141\)](#page-148-1) beschrieben.