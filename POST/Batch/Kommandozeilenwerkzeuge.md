
**12**

## <span id="page-154-0"></span>**Kommandozeilenwerkzeuge**

Im folgenden Kapitel werden einige der Werkzeuge beschrieben, die mit POST Batch ausgeliefert werden.

Sie enden (bis auf einige Aufrufe des **service.exe/.sh** Werkzeugs) mit einer einheitlichen Bewertung der ausgeführten Aktion / des ausgeführten Kommandos. Die Bewertung beinhaltet die Punkte

- Fehlertext
- Fehlercode
- Laufzeit

und ist nach dem folgenden Muster aufgebaut:

```
1 Processing successful (RC: 0, Elapsed time: 05.041 s)
```

Oder z.B. im Fehlerfall:

```
1 ERROR: Wrong usage , mandatory option 'operation ' missing (RC: 1, time: 0.010 seconds)
```

| Wert | Bedeutung                                                                       |
|------|---------------------------------------------------------------------------------|
| 0    | Ok                                                                              |
| 1    | Bedienungsfehler (ungültige Anzahl/Art der übergebenen Optionen oder Argumente) |
| 2    | Allgemeiner Fehler                                                              |

Tabelle 12.1: Bedeutung der Returncodes in den Kommandozeilenwerkzeugen

| Wert | Bedeutung                                                                         |  |
|------|-----------------------------------------------------------------------------------|--|
| 3    | Datei nicht gefunden oder keine Schreib-/Leserechte                               |  |
| 4    | Maximale Anzahl an Fehlern erreicht (Stapelverarbeitung)                          |  |
| 5    | Datenbankfehler                                                                   |  |
| 6    | Falsche / nicht auffindbare Konfiguration                                         |  |
| 7    | Warnungen in der Konfiguration                                                    |  |
| 8    | Fehler in der Konfiguration                                                       |  |
| 9    | Ungültige / nicht auffindbare Lizenzdatei                                         |  |
| 12   | Fehler in der Abbauphase                                                          |  |
| 13   | Nicht alle Zeilen der Eingabe wurden erfolgreich verarbeitet (Stapelverarbeitung) |  |
|      |                                                                                   |  |

## <span id="page-155-0"></span>**12.1 TOLERANT LIZENZ ÜBERPREUFEN**

Mit dem Kommandozeilenwerkzeug postLicense.sh/bat kann man eine vorliegende Lizenzdatei auf Validität und Inhalt prüfen, bevor POST Batch gestartet wird.

Das Werkzeug kann mit folgenden Parametern gestartet werden:

- --batch | --service: Bei Angabe einer dieser Parameter sucht das Tool automatisch im Konfigurationsverzeichnis der Installation nach einer entsprechenden Lizenzdatei (postService.lic, postBatch.lic oder der generischen post.lic)
- --batch | --service --licenseFile <Pfad zur Lizenzdatei>: Prüft eine konkret angegebene Lizenzdatei
- --batch | --service <Pfad zur Konfigurationdatei>: Prüft vorhandene Lizenzen gegen konkrete Ausprägungen einer Konfigurationsdatei
- --batch | --service <Pfad zur Konfigurationdatei> --licenseFile <Pfad zur Lizenzdatei>: Prüft eine konkret angegebene Lizenzdatei gegen Ausprägungen einer angegebenen Konfigurationsdatei

Beispiel für das Ergebnis einer Lizenzprüfung (hier am Beispiel der eingebetteten Lizenz die seit Version 11.1 in POST Batch enthalten ist):

|\_ \_/ \_ \| | | \_\_| \_ \ /\_\ | \| |\_ \_| | \_ \\_\_\_ \_\_| |\_ | || (\_) | |\_\_| \_|| / / \_ \| .' | | | | \_/ \_ (\_-< \_| |\_| \\_\_\_ /| \_\_\_\_|\_\_\_|\_|\_\/\_/ \\_\\_|\\_| |\_| |\_| \\_\_\_/\_\_/\\_\_| Version 12.0.56042 , Copyright (c) 2025 TOLERANT Software GmbH & Co KG Commandline Parameters : ======================= Check Mode: service Current License information (EMBEDDED): ================================================================= Customer: TOLERANT Support Product: POST Project: empty Valid from: 2025 -06 -10 Valid to: unlimited Volume: 10.000 Reference volume: 10.000 Rate: 1.000 Period: 24h Max product version: unlimited Max project count: 2 IP address: unlimited License version: unlimited Modules: BATCH , SERVICE WARN : Current license is an embedded license. Please contact our support to get a productive license. License file is ok Processing successful (RC: 0, Elapsed time: 0 ,157 sec) tlbuild@vm -berio :~/ Post/tpost -12.0 -56042/ bin\$

Tabelle [12.2:](#page-156-0) *['Einstellungsmöglichkeiten einer Lizenz'](#page-156-0)* beschreibt die Attribute und deren möglichen Auspräungen die in einer Lizenz enthalten sein können.

<span id="page-156-0"></span>

| Tabelle 12.2:<br>Einstellungsmöglichkeiten einer Lizenz |                        |                                                      |  |  |
|---------------------------------------------------------|------------------------|------------------------------------------------------|--|--|
| Attribut                                                | Ausprägungen           | Beschreibung                                         |  |  |
| Customer                                                | z.B. TOLERANT Software | Name des Firmenkunden                                |  |  |
| Product                                                 | MATCH,POST,            | TOLERANT Produkt für das die Lizenz gilt             |  |  |
|                                                         | NAME,BANK,MOVE         |                                                      |  |  |
| Project                                                 | z.B. Mueller IT        | Name des Kundenprojekts                              |  |  |
| Valid from                                              | z.B. 2025-10-04        | Gültigkeitsstartdatum                                |  |  |
| Valid to                                                | z.B. 2027-10-04        | Gültigkeitsenddatum, (kann maximal 24 Monate nach    |  |  |
|                                                         |                        | liegen)<br>Valid From                                |  |  |
| Volume¹                                                 | z.B. 100000            | Maximales Anzahl von Serviceanfragen (Service), oder |  |  |
|                                                         |                        | Einträgen in der Eingabedatei (Batch)                |  |  |

<sup>1</sup> Gilt pro Projekt

| Attribut            | Ausprägungen                      | Beschreibung                                                                                                                              |
|---------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Reference volume²   | z.B. 1000000                      | Maximales Größe des Match Indexes (Service) oder Ein<br>trägen in der Referenzdatei (Batch)                                               |
| Rate³               | z.B. 1000                         | Maximales Anzahl von Serviceanfragen (Service), oder<br>Einträgen in der Eingabedatei (Batch) pro Zeiteinheit<br>(siehe<br>Period)        |
| Period              | z.B. 24h                          | Zeitspanne in der das in<br>festgelegte Kontingent<br>Rate<br>zur Verfügung steht. Nach Ablauf wird der Zähler wie<br>der zurück gesetzt. |
| Max product version | z.B. 11.2                         | Höchste Version des Produkts, für das die Lizenz gültig<br>ist                                                                            |
| Max project count   | z.B. 4                            | Anzahl an Projekten die in der Konfiguration vorhan<br>den sein dürfen                                                                    |
| IP address          | z.B. 67.101.12.27                 | IP-Adresse des Hosts (wird derzeit nicht geprüft)                                                                                         |
| License version     | z.B. 1.1                          | Technische Version der Lizenz                                                                                                             |
| Modules             | SERVICE,BATCH,<br>BULK, ANONYMIZE | Erlaubte Produktkomponenten für die vorliegende Li<br>zenz                                                                                |
|                     |                                   |                                                                                                                                           |

## <span id="page-157-0"></span>**12.2 KONFIGURATION KONVERTIEREN**

Mit dem Kommandozeilenwerkzeug postConfigConverter kann man eine Service Konfigurationsdatei in eine Batch Konfigurationsdatei umwandeln oder umgekehrt.

Das Tool befindet sich noch in einem experimentellen Zustand. Es kann zurzeit nicht sichergestellt werden, dass die generierte Konfiguration direkt auf Anhieb nutzbar ist, weil manche Einstellungen nicht einfach konvertiert werden können. Das Tool erkennt, ob es sich bei der Eingabedatei um eine Batch- oder Service Konfiguration handelt und konvertiert die Konfiguration dann automatisch in den jeweils anderen Typ.

Das Werkzeug hat die folgenden Parameter (Pflichtparameter sind fett gedruckt):

- --configfile <configfile>: Die Konfigurationsdatei, die umgewandelt werden soll.
- --outputfile <outputfile>: Der Dateiname für die konvertierte Konfigurationsdatei.
- --projectid <projectid>: Das Projekt, das konvertiert werden soll.
- --outputid <outputid>: Ist die Eingangsdatei eine Batchkonfiguration und verwendet diese einen Outputswitch, so ist anzugeben, für welche Ausgabedatei eine Serviceausgabe erzeugt werden soll.

<sup>3</sup> Gilt pro Projekt

<sup>2</sup> Gilt pro Projekt

1

<span id="page-158-0"></span>Nach dem Aufruf wird aus der Eingangsdatei eine neue Konfigurationsdatei erstellt. Die Originaldatei bleibt unverändert.

## **12.3 ÜBERPRÜFUNG DER KONFIGURATION**

Da sich bei Änderung der Konfiguration von Hand leicht Fehler einschleichen, gibt es ein Kommandozeilen-Werkzeug, um eine Konfiguration auf Korrektheit zu überprüfen. Das Werkzeug wird mit den in Tabelle [12.3:](#page-158-1) *['Parameter Konfigurationsüberprüfung'](#page-158-1)* aufgeführten Parametern aufgerufen.

| Parameter               | Beschreibung                                                                                  |  |
|-------------------------|-----------------------------------------------------------------------------------------------|--|
| -batch                  | Die Konfigurationsdatei enthält Konfigurationen für einen Batch<br>lauf.                      |  |
| <filename></filename>   | Die zu überprüfende Konfigurationsdatei.                                                      |  |
| <projectid></projectid> | Optional. Bei Verwendung wird nur die Konfiguration für das ange<br>gebene Projekt überprüft. |  |

<span id="page-158-1"></span>Tabelle 12.3: Parameter Konfigurationsüberprüfung

Das Werkzeug gibt alle gefundenen Fehler und Warnungen auf der Konsole aus. Folgendes zeigt ein Beispiel:

```
2 _____ ___ _ ___ ___ _ _ _ _____ ___ _
3 |_ _/ _ \| | | __| _ \ /_\ | \| |_ _| | _ \___ __| |_
4 | || (_) | |__| _|| / / _ \| .' | | | | _/ _ (_-< _|
5 |_| \___ /| ____|___|_|_\/_/ \_\_|\_| |_| |_| \___/__/\__|
6
7 Version 12.0.56190 , Copyright (c) 2025 TOLERANT Software GmbH & Co KG
8
9 Commandline Parameters :
10 =======================
11 Check Mode: batch
12 XML File: config/ testPostBatchConfig1 .xml
13
14
15 Runtime
16
17 nothing to report
18
19 Project Id: postProject -1
20
21 ERROR : config/ testPostBatchConfig1 .xml :13: "/opt/tolerant/input/input.csv" does not exist
22 ERROR : config/ testPostBatchConfig1 .xml :23: "/opt/tolerant/output/output.csv" cannot be
       written
23
24 ERROR: configuration file has errors (RC: 8, Elapsed time: 1 ,821 sec)
```

#### AUFRUF UNTER WINDOWS

Geben Sie an einem Windows Kommandoprompt (cmd) die folgenden Kommandos ein:

- 1 cd <Installationsverzeichnis >
- 2 bin\ postCheck .bat -batch <filename > [<projectId >]

Listing 12.1: Programmstart Windows

#### AUFRUF UNTER LINUX

Öffnen Sie ein Terminalfenster und geben Sie folgende Kommandos ein:

```
1 cd <Installationsverzeichnis >
```

```
2 bin/ postCheck .sh -batch <filename > [<projectId >]
```

Listing 12.2: Programmstart Linux/Solaris

## <span id="page-159-0"></span>**12.4 DATEIPRÜFUNG**

Mit dem Kommandozeilenwerkzeug postFileTest kann man auf einfache Weise überprüfen, ob eine Synonymdatei korrekt funktioniert und die Transliterationen gewünschte Ergebnisse liefern.

Zusätzlich kann man den Inhalt der Eingabedatei anhand von Spaltentyp und Länge auf Gültigkeit überprüfen.

Das Kommando kann wie folgt gestartet werden:

- 1 cd <Installationsverzeichnis >
- 2 bin\ postFileTest .bat

Listing 12.3: Starte Kommando

Das Werkzeug hat die folgenden Parameter (Pflichtparameter sind fett gedruckt):

- --input: Die Datei, auf der die Synonyme und/oder die Normalisierung angewendet werden soll
- --codeset: Die Kodierung der Eingabedatei (bspw. "ISO8859-1")
- --sep: Das Trennzeichen der einzelnen Spalten
- --encl: Zeichen für den Einschluss einer Spalte (bspw. das doppelte Hochkomma)
- --esc: Maskierungszeichen für Sonderzeichen (bspw. das doppelte Hochkomma)
- --startline: Nummer (Zählung beginnt mit 1) der ersten Zeile, die keine Kopfzeile mehr ist
- --output: Die Datei, in der die Ergebnisse gespeichert werden sollen. Es werden die gleichen Parameter für codepage, Trennzeichen und weiter verwendet, wie bei der Eingabedatei.

- --defaults: Eine Komma-separierte Liste, bei der für jede Spalte ein Standardwert vergeben werden kann, wenn die Spalte in der Eingabedatei leer ist (analog zu defaultValue in der Konfigurationsdatei)
- --trim: Eine Komma-separierte Liste, bei der für jede Spalte angegeben werden kann, ob der Spaltenwert der Eingabedatei getrimmt werden soll. Möglich sind:
  - N: Nicht trimmen
  - L: Links trimmen
  - R: Rechts trimmen
  - B: Rechts und links trimmen
  - M: Mehrfach auftretende Leerzeichen auf ein einzelnes reduzieren
  - LM: Links und mittig trimmen
  - RM: Rechts und mittig trimmen
  - A: Mehrfache, führende und schließende Leerzeichen entfernen
- --synfile: Der Pfad zu einer Synonymdatei
- --syncs: Die codepage der Synonymdatei
- --syncase: Wenn "true" angegeben wird, dann werden bei der Synonymersetzung Groß- und Kleinschreibung berücksichtigt
- --synlist: Eine Komma-separierte Liste, bei der für jede Spalte angegeben werden kann, welche Synonymgruppe verwendet werden soll
- --translitlist: Eine Komma-separierte Liste, bei der für jede Spalte angegeben werden kann, welche Transliterationsregel verwendet werden soll. Neben vordefinierten Transliterationen (s. *[Transliterations-Identifier](#page-229-0)* (Seite [222\)](#page-229-0)) können Dateien, die Transliterationsregeln enthalten, eingegeben werden. Solche Eingaben müssen das Format "file::<Dateipfad>" haben.
- --length: Eine Komma-separierte Liste, bei der für jede Spalte angegeben werden kann, welche Länge der Spalteninhalt haben darf und die Behandlung beim Überschreiten der Länge. Die zwei Informationen sind für jede Spalte zusammengesetzt und mit einem "|" Zeichen getrennt.

Das Kommando kann mit einer kürzeren alternativen Parameterliste gestartet werden. Die oben beschriebenen Parameter werden dann intern ermittelt.

Die Pflichtparameter sind fett gedruckt:

— --optiontype: Steuert die Parameterauswertung. Setzen Sie den Wert dieses Parameters auf config, falls Sie die alternative Parameterliste mit Auswertung der Konfigurationsdatei nutzen möchten.

- --configfile: Der Pfad zu der Konfigurationsdatei, deren Eingabedatei mit dem Kommandozeilenwerkzeug geprüft werden soll
- --mode: Typ des Produkts, das mit der Konfigurationsdatei konfiguriert ist. Möglich sind:
  - service
  - batch
- --projectid: Das Projekt, dessen Eingabedatei überprüft werden soll
- --inputid: Die zu prüfende Eingabedatei

Wenn das Kommando gestartet wird, werden abhängig von den gewählten Optionen die Eingabedatei gelesen, die Synonyme ersetzt und die Transliteration durchgeführt. Das Ergebnis wird in die Ausgabedatei geschrieben. Weiterhin wird auf der Konsole ausgegeben, welche Synonyme, Transliterationen wie oft verwendet wurden.

Das Kommando kann auch direkt aus der TOLERANT GUI für eine existierende Konfiguration gestartet werden, dabei wird die Parameterliste automatisch aus der Konfiguration erzeugt.

## <span id="page-161-0"></span>**12.5 SUPPORTWERKZEUGE**

#### SUPPORTDATEI ERSTELLEN

Für den Fall, dass ein Problem auftritt, welches nur über den TOLERANT Support gelöst werden kann, kann mit Hilfe des Kommandozeilenwerkzeugs support.bat (oder support.sh) eine ZIP-Datei erstellt werden, in der alle relevanten Dateien (Logdateien, Konfigurationsfiles, etc.) gesammelt werden.

Bitte beachten Sie, dass die Logdateien, je nach Einstellung und Produkt, sensible Kundendaten enthalten können (s. Kapitel *['Sicherheitshinweise'](#page-196-0)* (Seite [189\)](#page-196-0)).

Das Kommando kann wie folgt gestartet werden:

```
1 cd <Installationsverzeichnis >
2 bin\support.bat
```

Das Werkzeug hat die folgenden Parameter:

- -?: Ausgabe der möglichen Kommandozeilenparameter
- -noconfig: Es werden keine Konfigurationsdaten gesammelt
- -nofileinfos: Es werden keine Informationen über Zugriffsrechte des Besitzers und der Gruppe und die Verzeichnisstruktur gesammelt
- -depth [number]: Es werden Informationen über Zugriffsrechte des Besitzers und der Gruppe und die Verzeichnisstruktur basierend auf der angegebenen Tiefe gesammelt

- -logrange [hours]: Die Logdateien werden basierend auf der angegebenen Stundenanzahl beschränkt gesammelt
- -logsize [MB]: Die Logdateien werden basierend auf der angegebenen Anzahl an MB beschränkt gesammelt
- -show: Die ermittelten Daten werden nur ausgegeben, es wird keine ZIP-Datei erzeugt
- -version: Es wird nur die Versionsinformation ermittelt

Wenn die Option -show nicht verwendet wird, dann wird eine ZIP-Datei erstellt. Das Werkzeug gibt den Namen der ZIP-Datei am Ende aus. Diese Datei kann dann an den Support verschickt werden.

Wenn die Option -depth [number] gesetzt ist, werden Informationen zur Verzeichnisstruktur, Zugriffsrechten und Besitzer der Dateien innerhalb der Installation gesammelt. Auf dem Betriebssystem Linux werden zusätzlich zum Besitzer der Datei auch die Gruppenzugriffsrechte angegeben.

Durch [number] kann die Tiefe bestimmt werden. Der Standardwert beim Ausführen von support.bat ist -depth 5. Bei -depth 0 werden die Daten zur Verzeichnisstruktur und den Zugriffsrechten ohne Beschränkung ermittelt.

Beim Ausführen von support.bat werden standardmäßig alle Logdateien berücksichtigt. Sind die optionalen Parameter -logrange [hours] oder -logsize [MB] gesetzt, so werden die Logdateien zeit- oder größenbasiert eingeschränkt gesammelt. Bei Angabe der Zahl Null mit diesen Parametern werden wie im Standardfall alle Logdateien gesammelt.

Bei -logrange [hours] werden die Logdateien gefiltert nach den Dateien, die sich in den letzten Stunden [hours] verändert haben oder innerhalb der Zeitspanne entstanden sind.

Bei -logsize [MB] werden Logdateien absteigend nach dem Änderungsdatum sortiert und bis zum Erreichen der angegebenen Megabyte-Grenze eingesammelt. Die Megabyte-Grenze ist eine weiche Grenze. Es werden keine Logdateien abgeschnitten. Falls die Grenze beim Hinzufügen einer Datei überschritten wurde, wird die komplette Datei hinzugefügt und anschließend das Filtern gestoppt.

Ausgenommen aus diesem Filterprozess sind Logdateien zur Installation (install\*.log und checksum\*.txt), da diese in der Regel außerhalb der zeit- oder größenbasierten Grenzen liegen, aber wichtige Informationen für den Support enthalten können.

#### INSTALLATION ÜBERPRÜFEN

Während der Installation wird eine Datei mit den Prüfsummen aller installierten Dateien erstellt. Möchte man feststellen, ob seit der Installation Dateien verändert, gelöscht oder hinzugefügt wurden, so kann dies mit dem Werkzeug checkinstallation.bat (oder checkinstallation.sh) überprüft werden. Das Kommando kann wie folgt gestartet werden:

1 cd <Installationsverzeichnis >

2 bin\ checkinstallation .bat

Das Werkzeug prüft dann die Prüfsummen aller Dateien in den Installationsverzeichnissen und gibt eine Übersicht über die hinzugefügten, gelöschten und veränderten Dateien aus.

Durch die zusätzliche Option --installed wird nicht gegen die Prüfsummen verglichen, die nach der Installation angelegt wurden, sondern gegen die Prüfsummen, die im Installationspaket für alle Dateien enthalten sind. Bei diesem Vergleich werden auch Dateien als geändert erkannt, die durch den Installationsprozess verändert wurden.