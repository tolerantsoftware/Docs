# Administration

## Referenzdaten installieren/aktualisieren

Sie erhalten die Referenzdaten (siehe auch Abschnitt [Referenzdaten](/POST/Batch/Konzepte.md#referenzdaten)) zum Download. Die Referenzdaten sind mit ZIP gepackt. Entpacken Sie diese Dateien vor der Nutzung.

Stellen Sie sicher, das kein POST BatchProzess läuft.

- 1. Für Version 5 der Validierungs-Engine kopieren Sie die Referenzdaten in die von Ihnen konfigurierten Verzeichnisse für die Referenzdaten und überschreiben Sie dabei die alten Referenzdateien.
- 2. Für Version 6 der Validierungs-Engine kopieren Sie die Länderdaten in eines der beiden Verzeichnisse `$TLDATA/FileSetA oder $TLDATA/FileSetB`. Jeweils eines der beiden Verzeichnisse wird aktiv von der Engine verwendet. Welches das akiv verwendete Verzeichnis ist, lässt sich in der Datei `FileSetsInfo.json` nachlesen, welche sich ebenfalls im Ordner `$TLDATA` befindet. Bsp.:
```json
{ " FileSetAState ": "InUse", " FileSetBState ": "Unused" }
```

Handelt es sich um Referenzdaten für Länder, die Sie bisher nicht in der Konfiguration aufgeführt hatten, so tragen Sie die neuen Dateien bitte in der Konfiguration nach (siehe Abschnitt [Konfiguration ergänzen](/POST/Batch/Administration.md#konfiguration-ergänzen)).


#### Referenzdaten zeitgesteuert aktualisieren (derzeit nur für version 5 der adressvalidierungs-engine)

Die Referenzdaten können zeitgesteuert von einem entfernten Server geladen und automatisch ausgetauscht werden, während der POST Batch läuft. Hierfür muss das Attribut <updateRefDataSchedule> in *<postRuntime>* konfiguriert werden.

Zu den definierten Zeitpunkten werden alle Einträge im Bereich `<postRuntime>` vom Typ `<referenceUpdate>` durchgelesen und deren Länderdateien vom konfigurierten entfernten Server aktualisiert. Ist die Aktualisierung der Referenzdaten erfolgreich, so wird eine Rekonfiguration der Runtime des Services automatisch ausgeführt. Für jede Länderdatei kann eine Prüfsummen-Datei mit dem Namen `<Remote-Dateiname>.md5` auf der Quellseite deponiert werden. Findet der POST Batch die Prüfsummen-Datei, so wird diese verwendet, um die heruntergeladene Länderdatei zu validieren. Hat die bestehende Länderdatei den gleichen Checksum-Wert wie die Remote-Datei, so wird diese nicht ausgetauscht. Die Prüfsummen-Datei muss den MD5-Wert der entpackten Remote-Datei enthalten. Hier ist auch zu beachten, dass bei korrupter Checksum-Datei kein Update stattfindet.

Die URL hat folgenden allgemeinen Aufbau: `<scheme>://<server>:<port>/pfad/dateiname`.

- **scheme:** Folgende Übertragungsprotokolle werden unterstützt: SFTP, FTP, FTPS, HTTP und HTTPS. Falls kein Scheme in der URL angegeben ist, wird die URL als Dateipfad interpretiert.
- **server:** Servername
- **port:** Serverport. Wird der Defaultwert des Protokollports verwendet (bspw. 80 für HTTP, etc.), so kann man diesen in der URL weglassen.
- **pfad:** Pfad des Remote-Verzeichnisses. Der Pfad kann folgende Platzhalter beinhalten: {country} und {type}. Diese Platzhalter werden durch ihre im Element referenceUpdate angegebenen Werte ersetzt.
- **dateiname:** Name der Remote-Datei. Der Name kann folgende Platzhalter beinhalten: {country} und {type}. Diese Platzhalter werden durch ihre im Element referenceUpdate angegebenen Werte ersetzt. Wird ein Asterisk (\*) im definierten Namen verwendet, so werden alle Dateien, deren Namen dem Muster entsprechen, aufgelistet und nur die neueste wird genommen.

Das planmäßige Aktualisieren der Referenzdaten kann nicht verwendet werden, um neue Dateien zu installieren, sondern es werden immer nur die bisher vorhandenen Referenzdateien aktualisiert. Für die Unterstützung eines neuen Landes bzw. eines neuen Referenzdatentyps muss daher einmalig die Referenzdatei manuell installiert werden.

<span id="page-149-0"></span>Beim automatischen Aktualisieren der Referenzdaten wird der letzte Zustand in der Datei "PostRef-DataUpdateHistory.json" protokolliert.

## Konfiguration ergänzen

### Lizenzen/Unlockcodes nachtragen

Eventuell benötigen Sie für zusätzliche Länderdaten weitere Lizenzen (V6) oder Unlockcodes (V5), die im config Verzeichnis abgelegt oder in der Konfiguration eingetragen werden müssen.

Dazu muss im Bereich *<postRuntime>* ein neuer *<unlockCode>*-Eintrag angelegt werden.

Bitte beachten Sie, dass ein Unlockcode erst aktiviert wird, wenn entweder der Service neu gestartet wird, oder der Service neu konfiguriert wurde. Nutzen Sie dazu das folgende Kommando:

```bat
cd <Installationsverzeichnis>\bin
service.exe backend --endpoint "operations " --function "reconfigure.service"
```

Bitte beachten Sie, dass während der Rekonfiguration der Service kurzfristig keine Anfragen entgegennimmt.

Ein Unlockcode ist in der Regel für eine bestimmte Zeit gültig und muss dann erneuert werden. Wird der Code nicht erneuert, gibt die Adressvalidierung bei der nächsten Abfrage nach Ablauf der Gültigkeit einen Fehler aus.

## Unlockcodes/Lizenzen verwalten

POST Batch verwendet Unlockcodes (V5) oder Lizenzdateien (V6) für die Lizenzierung der Länderdaten. Zur Verwendung lesen Sie bitte Abschnitt [Konfiguration ergänzen](/POST/Batch/Administration.md#konfiguration-ergänzen). Um die verwendeten Unlockcodes oder Lizenzdateien zu überprüfen, können Sie das Engine-Unlockcodes-Tool einsetzen. Dieses wird über die Kommandozeile aufgerufen und benötigt den Pfad zu Ihrer aktuellen Konfigurationsdatei als Parameter. Das Werkzeug gibt alle in der Konfigurationsdatei hinterlegten oder in der Lizenzdatei aufgelisteten Lizenzschlüssel aus. Zudem wird in der Benutzeroberfläche die Gültigkeit der Unlockcodes/Lizenzschlüssel direkt angezeigt.

<!-- tabs:start -->

### **Linux**

Öffnen Sie ein Terminalfenster und tippen Sie folgende Kommandos:

```sh
cd <Installationsverzeichnis>/bin
./postEngineUnlockCodes.sh <filename>
```

### **Windows**

Öffnen Sie einen Windows Kommandoprompt (cmd) und tippen Sie folgende Kommandos:

```cmd
cd <Installationsverzeichnis>\bin
postEngineUnlockCodes.bat <filename>
```

<!-- tabs:end -->

#### Beispiel-Ausgabe

Das Werkzeug gibt kann entweder die Inhalte der Lizenzdatei (V6), oder alle in der Konfigurationsdatei hinterlegten Lizenzschlüssel ("unlockcodes", V5) ausgeben . Hier ein Beispiel für V5 Unlockcodes:

```log
***********************************************************
* TOLERANT Post License Tool (Version : 11.0.48888)
* Copyright (C) 2022 TOLERANT Software GmbH & Co. KG
***********************************************************

Code                                                    Type            Exp. Date       Status
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx                    VALIDATION      2024 -03 -09    OK
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx                    GEO_STANDARD    2022 -04 -18    EXPIRED

Country     Code                                        Type            Exp. Date       Status
DEU         xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx        VALIDATION      2024 -03 -09    OK
DEU         xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx        GEO_STANDARD    2022 -04 -18    EXPIRED
```

## Lizenzen installieren/aktualisieren

Für die Aktualisierung einer Lizenz bekommen Sie eine neue Lizenzdatei geliefert, in welcher der neue Lizenzstring enthalten ist. Öffnen Sie die erhaltene Datei und fügen Sie den Inhalt in Ihre bisherige Lizenzdatei ein. Bitte achten Sie dabei darauf, den neuen Schlüssel in eine neue Zeile einzutragen. Eine Lizenzdatei muss einen der folgenden Namen haben:

- `postService.lic`
- `postBatch.lic`
- `post.lic` (generische Lizenz)

Dabei werden die spezifischeren Lizenzdateien (falls vorhanden) immer der generischen Lizenzdatei vorgezogen.

Ist die alte Lizenz noch gültig, so wird der neue Lizenzschlüssel zu folgenden Zeitpunkten neu eingelesen:

1. Beim Start eines Batchlaufs

Wenn die Gültigkeitszeiträume in den Lizenzen sich überschneiden, so wird die Lizenz mit dem am längsten gültigen Endedatum verwendet, sofern diese bereits gültig ist (`validFrom` liegt vor dem aktuellen Datum).

In Abschnitt [TOLERANT Lizenz überprüfen](/POST/Batch/Kommandozeilenwerkzeuge.md#tolerant-lizenz-Überprüfen) wird beschrieben, wie sich die Informationen über die aktuell verwendete Lizenz auslesen lassen.
