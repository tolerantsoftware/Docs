
**8**

## **Konfigurationsdatei**

<span id="page-62-0"></span>Die Konfiguration von POST Batch wird in einer XML Datei abgelegt. Der logische Aufbau der Datei entspricht einer Baumstruktur und ist hierarchisch organisiert. Die XML-Datei beinhaltet folgende Teile:

- *Elemente*, deren Auszeichnung mittels eines passenden Paars aus Start-Tag (<tag>) und Ende-Tag (</tag>) erfolgen kann
- *Attribute*, die an ein Start-Tag angehängt werden (Attribut-Name="Attribut-Wert"). Sie dienen als Zusatzinformationen der Elemente.
- *Kommentare*, die rein der Kommentierung dienen und nicht ausgewertet werden (<!—- Kommentar-Text —->)

Beachten Sie folgendes: Der Kopfbereich der XML Datei fängt immer mit dem gleichen Text an:

```
1 <?xml version="1.0" encoding="UTF -8"?>
2 <!DOCTYPE configuration SYSTEM "config.dtd" >
```

<span id="page-62-1"></span>Die Konfigurationsdatei muss gemäß der Document Type Definition (DTD), die in der Datei doc/config.dtd hinterlegt ist, definiert sein. Der in der XML-Datei angegebene Pfad zur der DTD-Datei ist für POST Batch unerheblich. Die DTD beschreibt die Struktur und Reihenfolge der Tags innerhalb der XML Datei.

## **8.1 KONVENTIONEN**

#### IDENTIFIER/REFERENZEN

In der Konfiguration haben viele Elemente (*tags*) ein Attribut *id*, das als Identifier dient. Dieser Identifier muss innerhalb des umschließenden Elements eindeutig sein. Wird das Element von einem anderen Element referenziert, so geschieht dies mittels eines Attributes, das auf *Ref* endet und als Wert den Identifier enthält. Ein Beispiel:

```
1 <csvFileInput id="csvInput -1" filepath="$TLDATA/data.txt" separator ="|"
2 codepage="ISO -8859 -1" escape="\" startline ="2" >
3 ...
4 </ csvFileInput >
5
6 <inputFieldmap id="inputFieldmap -1">
7 <inputFieldmapItem id="item -1" inputRef="csvFileInput -1" ...
8 </ inputFieldmap >
```

Hier wird ein <csvFileInput>-Element mit dem Identifier "csvFileInput-1" definiert.

Soll auf dieses verwiesen werden, bspw. in einem <inputFieldmapItem>-Element, so wird als Attribut inputRef="csvFileInput-1" verwendet. Damit wird definiert, dass das Element <inputFieldmapItem> als Eingabedatei die in dem Element <csvFileInput> mit der *id* "csvFileInput-1" definierte Datei verwenden soll.

#### DATEINAMEN

Werden Dateinamen in der Konfiguration angegeben, so können diese absolute oder relative Pfade beinhalten.

Absolute Pfade werden als solche behandelt und nicht verändert.

Ist ein Pfad relativ, so ist er immer relativ zum Ausführungsverzeichnis.

Will man auf Dateien zugreifen, die im Installationsverzeichnis von POST Batch liegen, so erreicht man dies, indem man den Pfad mit \$TOLERANT\_HOME beginnt. Diese Variable gibt es für alle Plattformen in genau dieser Schreibweise, d.h. selbst unter Windows ist **nicht** %TOLERANT\_HOME% zu schreiben!

Andere Systemumgebungsvariablen (Environmentvariablen, etc.) können in der Konfigurationsdatei ebenfalls verwendet werden, wobei unabhängig vom Betriebssystem immer \$ als Präfix verwendet werden muss.

#### VERSIONIERUNG

Die XML-Dateien haben eine Versionsnummer, die angibt, mit welcher Produktversion eine XML-Datei kompatibel ist. Diese Nummer ist als Attribut des umschließenden Tags <configuration> angegeben und heißt version. Dies ist keine Versionsnummer für die Konfiguration an sich, sondern nur eine produktinterne Nummer für die Syntax der Konfigurationsdatei. Bitte verändern Sie diese nicht.

#### DATEI postBatchExample.xml

Bei der Auslieferung erhalten Sie eine Schablone, die ein Gerüst und genaue Erläuterungen für eine Konfigurationsdatei vorgibt. Sie ist in der Datei templ\_post\_batch.xml hinterlegt. Sie finden das komplette Beispiel im config/templates Verzeichnis Ihrer Installation. Die Einstellungen lassen sich mit einem einfachen Texteditor verändern.

Die Konfigurationsdatei lässt sich in folgende Rubriken unterteilen:

- 1. Laufzeitkonfiguration
  - a) Datenbanken
  - b) Lizenzschlüssel
  - c) Referenzdatenbanken
  - d) Validierungs-Engine
- 2. Projektkonfiguration
  - a) Datenbanken
  - b) Synonymdatei
  - c) Definition einer Ausnahmedatei
  - d) Beschreibung der Eingabedatei
  - e) Beschreibung der Ausgabedatei
  - f ) Beschreibung der Eingabefelder
  - g) Beschreibung der Ausgabefelder
  - h) Profileinstellungen
  - i) Definition mehrerer Ausgabedateien
  - j) Bedeutung und Zuweisung der Eingabefelder
  - k) Definition generierter Ausgabefelder

l) Bedeutung und Zuweisungen der Ausgabefelder

Jeder Abschnitt wird nun im Folgenden getrennt betrachtet, wobei am Ende eines jeden Abschnitts ein kleines Codebeispiel angehängt ist.

## <span id="page-65-0"></span>**8.2 LAUFZEITKONFIGURATION**

Im einmal vorkommenden Element der <postRuntime> stellen Sie die Werte passend für Ihre Systemumgebung ein. Innerhalb dieses Elements können mehrere Elemente des Typs <unlockCode>, <referenceDatabase> und <postEngine> vorkommen.

<span id="page-65-1"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default        | Bedeutung                                                                                                                                                                                                               |
|---------------------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette              | ID zur Referenzierung dieses Elements                                                                                                                                                                                   |
| engineVersion •           | V6*,<br>V5                | V6: Verwendung der Validierungs<br>Engine mit der Version 6 (empfohlen).<br>V5: Verwendung der Validierungs-Engine mit<br>der Version 5 (analog zu den Produktversio<br>nen < 11.0).                                    |
| hotSwapping               | Y<br>N*                   | Legt fest ob der Hot Swap Modus aktiviert<br>werdern soll (V6_ONLY, siehe Kapitel<br>'Re<br>(Seite<br>ferenzdaten installieren/aktualisieren'<br>141))                                                                  |
| cacheSize •               | NONE,<br>SMALL,<br>LARGE* | Größe des verwendeten Zwischenspeichers.<br>NONE: Kein Zwischenspeicher verwenden<br>SMALL: Kleinen Zwischen<br>speicher verwenden<br>LARGE: Großer Zwischenspeicher, mehr<br>Speicherverbrauch.                        |
| threadCount •             | Zahl 1128<br>(Default=1)  | Anzahl der Threads, die eine postalische Prü<br>fung durchführen. Die Threadzahl sollte die<br>Anzahl der Prozessorkerne nicht überschrei<br>ten. Bei einem Dualcore Prozessor sind also<br>maximal 2 Threads sinnvoll. |

#### Tabelle 8.1: Konfigurationselement <postRuntime>

| Attribut<br>• Pflichtfeld | Werte<br>* Default          | Bedeutung                                                                                                                                                                                                                                                         |
|---------------------------|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| queueSize •               | Zahl 110000<br>(Default=10) | Maximale Anzahl der Datensätze, die in der<br>Prozesswarteschlange eingereiht werden. Der<br>Wert sollte nicht geringer ausfallen, als die<br>Anzahl der Threads.                                                                                                 |
| maxMemoryUsageMB •        | Zahl<br>(Default=3584)      | Maximale Größe des temporär verwendeten<br>Speichers (RAM) während des Abgleichs. Der<br>verwendete Speicher wirkt sich unmittelbar<br>auf die Performance aus.                                                                                                   |
| abortPort                 | Zahl oder "OFF"             | Ein Socketport, der im Batchlauf benutzt<br>werden kann, um über diesen den Batch<br>lauf zu beenden, wenn das magische Wort<br>"TOLERANT"<br>geschickt wird. Soll kein Port<br>verwendet werden, so sollte man hier "OFF"<br>eingeben. Der Standardwert ist 8979 |

<span id="page-66-0"></span>Das Element <proxySetting> enthält die Proxy-Einstellungen für ein <postEngine>-Element. Dieser Abschnitt ist optional, kann aber mehrmals vorkommen.

| Attribut<br>• Pflichtfeld | Werte<br>* Default                | Bedeutung                                              |
|---------------------------|-----------------------------------|--------------------------------------------------------|
| id •                      | Zeichenkette                      | ID zur Referenzierung dieses Elements                  |
| name                      | Zeichenkette                      | Name für die Anzeige in der GUI                        |
| proxyHost •               | Zeichenkette                      | Name des Proxy-Servers                                 |
| proxyPort •               | Zahl 165535<br>(Default=80)       | Port des Proxy-Servers                                 |
| proxyProtocol •           | HTTP*,<br>HTTPS,<br>FTP,<br>SOCKS | Das Protokoll, das vom Proxy-Server verwen<br>det wird |
| proxyUser                 | Zeichenkette                      | User-Name                                              |
| proxyPassword             | Zeichenkette                      | Passwort                                               |

#### Tabelle 8.2: Konfigurationselement <proxySetting>

<span id="page-66-1"></span>Das Element <unlockCode> ist zuständig für Ihre Unlockcodes (oder Lizenzschlüssel). Es kann mehrmals vorkommen.

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                             |
|---------------------------|--------------------|---------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Elements |
| key •                     | Zeichenkette       | Lizenzschlüssel                       |

Tabelle 8.3: Konfigurationselement <unlockCode>

<span id="page-67-0"></span>Das Element <referenceDatabase> enthält Einstellungen zu den Referenzdaten. Es kann mehrmals vorkommen.

| Attribut<br>• Pflichtfeld | Werte<br>* Default                  | Bedeutung                                                                                                                                                              |
|---------------------------|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette                        | ID zur Referenzierung dieses Elements                                                                                                                                  |
| country •                 | ALL<br>oder<br>ISO3166 alpha-3-Code | Das Attribut gibt an, welches Land von<br>den folgenden Einstellungen betroffen ist.<br>ALL: alle Länder.                                                              |
| preloading •              | FULL*,<br>PARTIAL,<br>NONE          | Daten aus der Datenbank können vor der<br>Prüfung in den Speicher geladen werden.<br>FULL: Alle Daten laden;<br>PARTIAL: Partielle Ladung;<br>NONE: Keine Daten laden. |

#### Tabelle 8.4: Konfigurationselement <referenceDatabase>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                                                                                                                                                                                                          | Bedeutung                                                                                                       |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| type •                    | BATCH_INTERACTIVE,<br>FASTCOMPLETION,<br>CERTIFIED,<br>GEOCODING,<br>GEOCODING<br>ARRIVAL_POINT,<br>GEOCODING_PAR<br>CEL_CENTROID,<br>GEOCODING_ROOFTOP,<br>SUPPLEMENTARY,<br>ADDRESS_CO<br>DE_LOOKUP,<br>GEO_ONLY,<br>STAB,<br>AGS,<br>CAMEO,<br>LIST,<br>GEOCODETOADDRESS | Art der Referenzdaten. Siehe auch Abschnitt<br>(Seite<br>'Referenzdaten installieren/aktualisieren'<br>141)     |
| filepath •                | Pfad                                                                                                                                                                                                                                                                        | Verzeichnis der Referenzdaten. Der Pfad der<br>Referenzdatendatei ist relativ zum Installa<br>tionsverzeichnis. |

Das Element <postEngine> enthält die Einstellungen zu Adressvalidierungs-Engines (siehe Abschnitt *['Cloud-Service für Adressvalidierung'](#page-33-0)* (Seite [26\)](#page-33-0)) Dieser Abschnitt ist optional, kann aber mehrmals vorkommen.

<span id="page-68-0"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                      |
|---------------------------|--------------------|------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Elements (siehe<br>Abschnitt<br>(Seite<br>78)).<br>'Post-Profile' |
| name                      | Zeichenkette       | Name für die Anzeige in der GUI                                                                |
| description               | Zeichenkette       | Beschreibung der Konfiguration (nur für<br>Dokumentationszwecke)                               |

Tabelle 8.5: Konfigurationselement <postEngine>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                 | Bedeutung                                                                                                                                              |
|---------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| engineType •              | ADCLOUD,<br>TOLERANTCLOUD,<br>HERE | Typ der Engine                                                                                                                                         |
| maxRequestsPerAddress     | Zahl 0300<br>(Default=1)           | Maximale Anzahl der Abfragen, die pro Post<br>Abfrage an diese Engine geschickt werden<br>können (maxRequestsPerAddress = 0 bedeu<br>tet "unbegrenzt") |
| transactionPool           | ANY,<br>PRODUCTION*,<br>TEST       | Art des genutzten Transaction-Pools (Pflicht<br>für Engine vom Typ ADCLOUD)                                                                            |
| login                     | Zeichenkette                       | Login-Name (Pflicht für Engine vom Typ AD<br>CLOUD)                                                                                                    |
| password                  | Zeichenkette                       | Passwort (Pflicht für Engine vom Typ AD<br>CLOUD). Das Passwort kann verschlüsselt<br>sein (siehe Abschnitt<br>(Seite<br>190)).<br>'Passwörter'        |
| proxySettingRef           | Zeichenkette                       | Verweise auf eine Proxy-Einstellung. Die re<br>ferenzierte Proxy-Einstellung muss in der<br>Laufzeitkonfiguration vorkommen.                           |

Die Verbindung zu einem REST-Service kann durch das Element <postEngineRestEndPoint> definiert werden. Ein <postEngineRestEndPoint> kann eine Liste von <parameter> haben. Diese Liste ist spezifisch für die jeweilige Engine.

<span id="page-69-0"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                                                                                                    |
|---------------------------|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Elements                                                                                                                                                                        |
| serviceUrl •              | URL                | URL des REST-Services. Der Abfrageteil der<br>URL bildet sich aus Parameter-Werte-Paaren,<br>die mit einem "+"-Zeichen getrennt sind. Die<br>Paare selbst werden durch ein Gleichheitszei<br>chen verbunden. |

#### Tabelle 8.6: Konfigurationselement <postEngineRestEndPoint>

Für die serviceURL können die folgenden Werte vergeben werden:

— https://eu-av.informaticadaas.com/AV6/v1

#### — https://us-av.informaticadaas.com/AV6/v1

Weiterhin kann die Verbindung zu einem SOAP-Service (für Legacy Version V4) durch das Element <postEngineSoapEndPoint> definiert werden. Dieser Abschnitt ist optional.

<span id="page-70-1"></span>

| Tabelle 8.7:<br>Konfigurationselement<br><postenginesoapendpoint></postenginesoapendpoint> |                    |                                       |
|--------------------------------------------------------------------------------------------|--------------------|---------------------------------------|
| Attribut<br>• Pflichtfeld                                                                  | Werte<br>* Default | Bedeutung                             |
| id •                                                                                       | Zeichenkette       | ID zur Referenzierung dieses Elements |
| serviceName •                                                                              | Zeichenkette       | Name des SOAP-Service                 |
| serviceUrl •                                                                               | URL                | URL des SOAP-Service                  |
| wsdlUrl •                                                                                  | URL                | URL zu dem WSDL-File des Services     |

Nachfolgend finden Sie eine Beispielkonfiguration für die Basiseinstellungen zur Laufzeitumgebung, Unlockcodes, Referenzdatenbank und Postengine.

```
1 <postRuntime id="engine1" cacheSize ="LARGE" queueSize ="10" threadCount ="2" maxMemoryUsageMB ="
       2048">
2 <proxySetting id="proxy -settings" proxyHost ="127.0.0.1" proxyPort ="8080" proxyProtocol ="HTTP
         " proxyUser ="XXX" proxyPassword ="XXX" />
3 <referenceDatabase id="db-DEBI" country="DE" preloading ="NONE" type="BATCH_INTERACTIVE"
         filepath="data" />
4 <postEngine id="PENGINE1" description ="extra post engine" engineType ="ADCLOUD"
         transactionPool ="PRODUCTION" login="loginname" password="***" proxySettingRef ="proxy -
         settings">
5 <postEngineRestEndPoint id="ad-cloud" serviceUrl ="https://..." />
6 </ postEngine >
7 </ postRuntime >
```

<img src="data:image/jpeg;base64,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" alt="" style="max-width: 100%;">

## <span id="page-70-0"></span>**8.3 PROJEKTKONFIGURATION**

<span id="page-70-2"></span>Die Projektkonfiguration beschreibt die Ein- und Ausgangsparameter des Batchlaufs für ein spezielles Projekt. Jeder POST Batchlauf kann eine eigene Projektkonfiguration verwenden und somit eigene Einund Ausgangsfelder definieren. Innerhalb der Projektkonfiguration werden die Synonymdaten, die Einund Ausgangsdateien, die Eingangs- und Ausgangsfelder, die Profile und die Bedingungen, nach denen entschieden wird, welcher Output verwendet wird, definiert.

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                                                                                                                                            |
|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette       | Projekt-ID. Diese ID muss beim Aufruf des<br>Batch-Programms verwendet werden, um<br>dieses Projekt auszuwählen.<br>Achtung: Erlaubte Zeichen: Zahlen, Buch<br>staben (A-Z), Unterstrich (_) Bindestrich (-)<br>und Punkt (.).                       |
| title                     | Zeichenkette       | Projekttitel des Projektes (nur für Doku<br>mentationszwecke)                                                                                                                                                                                        |
| author                    | Zeichenkette       | Ersteller der Konfiguration (nur für Doku<br>mentationszwecke).                                                                                                                                                                                      |
| description               | Zeichenkette       | Beschreibung der Konfiguration (nur für<br>Dokumentationszwecke).                                                                                                                                                                                    |
| overwriteResults          | Y*<br>N            | Sollen die Ergebnisdateien überschrieben<br>werden, falls Sie schon existieren?                                                                                                                                                                      |
| rejectFiles               | Y*<br>N            | Soll für die Eingabedatei eine Datei mit dem<br>Namen *.reject angelegt werden, in der alle<br>Zeilen der Eingabe abgelegt werden, die<br>nicht korrekt gelesen werden konnten, weil<br>beispielsweise die Anzahl der Spalten nicht<br>gestimmt hat? |
| testmode                  | Y<br>N*            | Legt fest, ob nur eine bestimmte Anzahl von<br>Testsätzen verarbeitet werden soll.                                                                                                                                                                   |
| nrTestRecords             | Zahl               | Anzahl der Testsätze. Setzt voraus, dass test<br>mode=Y gesetzt ist. Beispiel: Wenn der Para<br>meter den Wert 1000 annimmt, werden aus<br>allen Verarbeitungsdateien die ersten 1000<br>Datensatz für Batchlauf herangezogen.                       |
| maxBatchErrors            | Zahl               | Anzahl der Fehler, bzw. Strukturabweichun<br>gen, die erlaubt sind, bevor der Batchprozess<br>abbricht. Die fehlerhaften Datensätze wer<br>den für die Weiterverarbeitung ignoriert.                                                                 |
| warnOnErrors              | Y<br>N*            | Steuert, ob der Returncode (13) bei Fehlern<br>zurückgeliefert wird (Tabelle<br>'Bedeutung der<br>Returncodes in den Kommandozeilenwerkzeu<br>(Seite<br>147)).<br>gen'                                                                               |

Tabelle 8.8: Konfigurationselement <postProject>

Nachfolgend finden Sie eine Beispielkonfiguration für die Projektkonfiguration.

```
1 <postProject author="Max Mustermann" description ="Post Batch für CRM" id="postProject -1"
2 title="Customer Relationship Management" >
3 ...
4 </ postProject >
```

```
Listing 8.2: Beispiel Projektkonfiguration
```

## <span id="page-72-0"></span>**8.4 DATENBANKEN**

Im Bereich <database> kann eine Verbindung zu einer Datenbank definiert werden. Das Element wird als Unterelement von <postProject> definiert.

<span id="page-72-1"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                          | Bedeutung                                                                                                                                                                                                     |
|---------------------------|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id•                       | Zeichenkette                                                | ID zur Referenzierung dieses Abschnitts                                                                                                                                                                       |
| name•                     | Zeichenkette                                                | Name des Elements                                                                                                                                                                                             |
| type•                     | H2*<br>SQLITE<br>ROCKS<br>ORACLE<br>POSTGRESQL<br>SQLSERVER | Datenbanktyp                                                                                                                                                                                                  |
| filepath                  | Zeichenkette                                                | Pfad der Datenbankdatei (für H2 ist die En<br>dung<br>wegzulassen) (Nur embedded<br>.h2.db<br>Datenbanken wie H2, RocksDB oder SQLite.<br>Bei RocksDB ist lediglich ein Pfad zu einem<br>Ordner erforderlich) |
| cacheSize                 | Cachegröße in Megabyte.<br>Default ist 100 MB               | Größe des Caches für die Datenbank (nur H2,<br>RocksDB oder SQLite)                                                                                                                                           |
| databaseName              | Zeichenkette                                                | Name der Datenbank (nicht nötig für embed<br>ded Datenbanken)                                                                                                                                                 |
| hostName                  | Zeichenkette                                                | Name des Rechners, auf dem die Datenbank<br>läuft (nicht nötig für embedded Datenban<br>ken)                                                                                                                  |

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| portNumber                | Zahl               | Port, auf dem der Datenbank-Server regis<br>triert ist (nicht nötig für embedded Daten<br>banken)                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| user•                     | Zeichenkette       | Benutzername für die Anmeldung an die<br>Datenbank                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| password                  | Zeichenkette       | Das Passwort für den Datenbank-Benutzer.<br>Ist das Passwort unverschlüsselt, so ist ein<br>Hashzeichen(#) voranzustellen, ansonsten<br>wird das Passwort als verschlüsselt angese<br>hen (siehe auch Abschnitt<br>(Seite<br>'Passwörter'<br>190)).                                                                                                                                                                                                                                                                                            |
| maxBulkSize               | Zahl (Default 100) | Datenbank-Zugriffe werden ggf. gebündelt<br>ausgeführt, um Overhead in der Kommuni<br>kation einzusparen. Dieses Attribut gibt die<br>maximale Anzahl von Einzel-Operationen an,<br>die zusammengefasst werden<br>¹.                                                                                                                                                                                                                                                                                                                           |
| tablePrefix               | Zeichenkette       | Ein Präfix, das vor den Namen jeder Daten<br>banktabelle gesetzt wird.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| url                       | Zeichenkette       | Voll qualifizierte JDBC-URL zu einer Daten<br>bank. Für RocksDB Datenbanken wird das<br>Attribut nicht unterstützt. Für eine Oracle<br>Datenbank funktioniert die URL nur in Kom<br>bination mit den Attributen<br>und<br>user<br>pass<br>word, für H2 mit<br>user. Für eine SQL Server<br>Datenbank können Benutzername und Pass<br>wort zusätzlich zu der URL gesetzt werden<br>oder direkt in der URL ergänzt werden. Ei<br>ne voll qualifizierte URL ersetzt die Attribute<br>databaseName,<br>hostName,<br>und<br>portNumber<br>filepath. |

Nachfolgend finden Sie ein Beispiel für die Konfiguration einer POSTGRESQL Datenbank.

1 <database id="db-1" name="myDatabase" type="POSTGRESQL" databaseName ="trd" hostName="localhost " user="trd\_user" password="#trd\_pw" />

Listing 8.3: Beispielkonfiguration einer POSTGRESQL Datenbankverbindung

<sup>1</sup> Im Falle einer SQLite-Datenbank wird empfohlen, den Parameter maxBulkSize auf 1 zu setzen. Hierdurch werden Datenbankverbindungen schneller freigegeben.

Nachfolgend finden Sie ein Beispiel für eine Konfiguration einer H2 Datenbank.

1 <database id="db-2" name="myDatabase" type="H2" user="sa" filepath="\$TLDATA/originalData" /> Listing 8.4: Beispielkonfiguration einer H2 Datenbankverbindung

## <span id="page-74-0"></span>**8.5 SYNONYMDATEI**

Mit dem <synonymFile> Abschnitt wird definiert, wo die Synonyme hinterlegt sind. Dieser Abschnitt ist optional. Wird keine Synonymdatei definiert, so gibt es einen Fehler, wenn einzelne Eingabe- oder Ausgabefelder auf eine Synonymliste verweisen.

<span id="page-74-2"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                  | Bedeutung                                                                                                                           |
|---------------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette                                                                        | ID zur Referenzierung dieses Abschnitts                                                                                             |
| filepath •                | Filename mit Pfad                                                                   | Pfad zu der Synonymdatei.                                                                                                           |
| codepage                  | z.B. ISO8859-1*, UTF-8,<br>etc. (s.<br>Unterstützte Code<br>(Seite<br>221))<br>sets | Dies definiert die in der Synonymdatei ver<br>wendete Zeichenkodierung.                                                             |
| caseSensitive •           | Y<br>N*                                                                             | Y = Groß-/Kleinschreibung soll bei der<br>Synonymersetzung berücksichtigt werden.                                                   |
| allowEmptyResults •       | Y*<br>N                                                                             | Y = Ergebnis der Synonymersetzung kann<br>leer sein, N = Ist das Ergebnis der Ersetzung<br>leer, so wird der Originalwert behalten. |

#### Tabelle 8.10: Konfigurationselement <synonymFile>

<span id="page-74-1"></span>Der Inhalt der Synonymdatei und die verschiedenen Ersetzungsmodi sind in Abschnitt *['Synonyme'](#page-23-0)* (Seite [16\)](#page-23-0) beschrieben.

## **8.6 AUSNAHMEDATEI**

Im optional einmal vorkommenden Element der <addressExceptionsFile> können Sie eine Datei konfigurieren, in der eine Ausnahmeliste von Adressen definiert ist, die nicht überprüft werden sollen. Eine Beschreibung der Nutzung der Ausnahmedatei finden Sie in Abschnitt *['Ausnahmedatei'](#page-26-0)* (Seite [20\)](#page-26-0) beschrieben.

<span id="page-75-1"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                  | Bedeutung                                                                                                                                                          |
|---------------------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette                                                                        | ID zur Referenzierung dieses Abschnitts                                                                                                                            |
| codepage                  | z.B. ISO8859-1*, UTF-8,<br>etc. (s.<br>Unterstützte Code<br>(Seite<br>221))<br>sets | Dies definiert die in der Ausnahmedatei ver<br>wendete Zeichenkodierung                                                                                            |
| filepath •                | Dateiname mit Pfad                                                                  | Pfad und Name der Ausnahmedatei                                                                                                                                    |
| separator •               | Zeichen, z.B. ';'                                                                   | In der Ausnahmedatei verwendetes Trennzei<br>chen zwischen Feldern. Soll ein Tabulatorzei<br>chen verwendet werden, so benutzen Sie die<br>folgende Notation '\t'. |

#### Tabelle 8.11: Konfigurationselement <addressExceptionsFile>

Nachfolgend finden Sie eine Beispielkonfiguration für die Basiseinstellungen zur Ausnahmedatei.

1 <addressExceptionsFile id="addressException" filepath="exceptions.csv" separator =";" codepage= "ISO -8859 -1" />

Listing 8.5: Beispiel Ausnahmedatei

## <span id="page-75-0"></span>**8.7 EINGABEFELDER (DATEI)**

Im Abschnitt <csvFileInput> werden Eingabedateien und ihre Eigenschaften für die Batch-Verarbeitung konfiguriert.

<span id="page-75-2"></span>

| Tabelle 8.12:<br>Konfigurationselement<br><csvfileinput></csvfileinput> |              |                                         |
|-------------------------------------------------------------------------|--------------|-----------------------------------------|
| Attribut<br>Werte<br>Bedeutung<br>• Pflichtfeld<br>* Default            |              |                                         |
| id •                                                                    | Zeichenkette | ID zur Referenzierung dieses Abschnitts |
| filepath •                                                              | Zeichenkette | Pfad der Eingabedatei                   |

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                                        | Bedeutung                                                                                                                                                                                                                                                       |
|---------------------------|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| separator •               | Zeichen, z.B. ';'                                                                                         | In der Eingabedatei verwendetes Trennzei<br>chen zwischen Feldern. Soll ein Tabulatorzei<br>chen verwendet werden, so benutzen Sie die<br>folgende Notation: '\t'.                                                                                              |
| enclosure                 | Zeichen, z.B. '"'                                                                                         | Texterkennungszeichen. Kann in der Eingabe<br>datei paarweise vor und nach dem Feldtext<br>verwendet werden. Sollte verwendet werden,<br>falls das Trennzeichen innerhalb des Feldtex<br>tes vorkommen kann.                                                    |
| escape                    | Zeichen, z.B. '\'                                                                                         | Escapesymbol. Sollte vor Trennzeichen oder<br>Texterkennungszeichen verwendet werden,<br>falls diese innerhalb des Feldtextes vorkom<br>men.                                                                                                                    |
| codepage                  | z.B. ISO8859-1, UTF<br>8, etc. (s.<br>Unterstütz<br>(Seite<br>221))<br>te Codesets<br>(Default=ISO8859-1) | In der Eingabedatei verwendete Zeichenko<br>dierung.                                                                                                                                                                                                            |
| startline                 | Zahl (Default: 1)                                                                                         | Nummer der ersten Zeile mit Nutzdaten (be<br>ginnend bei 1). Dies kann benutzt werden,<br>um Kopfzeilen zu überspringen.<br>startLine<br>kann auf '0' gesetzt werden, dies ist gleich<br>bedeutend mit '1', d.h. die Datei wird ab der<br>ersten Zeile gelesen. |

<span id="page-76-0"></span>Ein Abschnitt von <csvFileInput> beinhaltet einen oder mehrere Einträge vom Typ <csvInput-Field>, welche die eigentlichen Felder mit ihren Eigenschaften repräsentieren.

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                       |
|---------------------------|--------------------|-----------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Feldes                             |
| name                      | Zeichenkette       | Name des Feldes, diesen können Sie beliebig<br>vergeben         |
| defaultValue              | Zeichenkette       | Vorgabewert, falls das Feld in der Eingabe<br>nicht gefüllt ist |

#### Tabelle 8.13: Konfigurationselement <csvInputField>

| Attribut<br>• Pflichtfeld | <b>Werte</b><br>* Default                                                                        | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------------------|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| synonymList               | Zeichenkette                                                                                     | Name einer Synonymliste. Muss in der Syn-<br>onymdatei vorkommen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| transLitIdent             | Zeichenkette                                                                                     | Ein Bezeichner, der die vorzunehmende<br>Transliteration definiert                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| trimType                  | N=None<br>L=Left<br>R=Right<br>B=Both*<br>M=Middle<br>LM=Left-Middle<br>RM=Right-Middle<br>A=All | Entfernen von Leerzeichen (deaktiviert, links<br>rechts, beidseitig (links und rechts), mittig<br>(mehrfach auftretende Leerzeichen werden<br>auf ein einzelnes reduziert), links und mittig<br>rechts und mittig, sowie generelles Entfernen<br>von mehrfachen, führenden und schließen-<br>den Leerzeichen)                                                                                                                                                                                                                                                                    |
| type                      | C=Characters*,<br>D=Date,<br>L=List                                                              | Bestimmt den Eingabetyp des Feldes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| pattern                   | Zeichenkette                                                                                     | Je nach Typ (type) wird hier das Mus-<br>ter der Eingabewerte angegeben.<br>'C': keine Auswirkung;<br>'D': eine durch Komma getrennte Liste<br>von Datumsformaten, wobei jedes For-<br>mat einen Präferenzwert haben kann,<br>z.B. "dd.MM.yyyy (100), yyyy/MM/dd (80)"<br>(mehr Infos unter https://docs.oracle.<br>com/javase/8/docs/api/java/time/<br>format/DateTimeFormatter.html);<br>'L': kommaseparierte Liste von<br>erlaubten Eingabewerten, z.B.<br>"Berlin, Hamburg, München".<br>Anpassungen entsprechend Attribut<br>trimType erfolgen <i>vor</i> der Verarbeitung. |
| length                    | Zahl (Default ist 0)                                                                             | Die maximale Länge eines Eingabefeldes. Der<br>Wert 'O' bedeutet, dass keine Längenprüfung<br>stattfindet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

| Attribut<br>• Pflichtfeld | Werte<br>* Default     | Bedeutung                                                                                                                                                                                                                                                     |
|---------------------------|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| lengthErrorAction         | NONE<br>ERROR*<br>CLIP | Die Behandlung, wenn die eingestellte Län<br>ge des Feldes überschritten wird. Bei ERROR<br>wird ein Fehler ausgegeben, bei CLIP wird<br>der Wert des Eingabefeldes auf die eingestell<br>te Länge abgeschnitten und bei NONE gibt es<br>keine Einschränkung. |

Beispiel:

| 1 |  | <csvfileinput id="csvFileInput -1"></csvfileinput>                   |                                                                                |
|---|--|----------------------------------------------------------------------|--------------------------------------------------------------------------------|
| 2 |  | <csvinputfield id="csvInputField -2" name="street"></csvinputfield>  |                                                                                |
| 3 |  | <csvinputfield id="csvInputField -3" name="houseno"></csvinputfield> |                                                                                |
| 4 |  | <csvinputfield id="csvInputField -4" name="zipcode"></csvinputfield> |                                                                                |
| 5 |  |                                                                      | <csvinputfield id="csvInputField -5" name="city" trimtype="B"></csvinputfield> |
| 6 |  | <csvinputfield id="csvInputField -6" name="country"></csvinputfield> |                                                                                |
| 7 |  |                                                                      |                                                                                |

Listing 8.6: Beispiel Konfiguration der Eingabefelder

## <span id="page-78-0"></span>**8.8 EINGABEFELDER (DATENBANK)**

Im Abschnitt <databaseInput> werden Eingabetabellen in Datenbanken, und ihre Eigenschaften für die Batch-Verarbeitung konfiguriert.

<span id="page-78-1"></span>

| Tabelle 8.14:<br>Konfigurationselement<br><databaseinput></databaseinput> |                    |                                                                                                                          |
|---------------------------------------------------------------------------|--------------------|--------------------------------------------------------------------------------------------------------------------------|
| Attribut<br>• Pflichtfeld                                                 | Werte<br>* Default | Bedeutung                                                                                                                |
| id •                                                                      | Zeichenkette       | ID zur Referenzierung dieses Abschnitts                                                                                  |
| databaseRef •                                                             | Zeichenkette       | Verweis auf eine im Abschnitt<br>'Datenbanken'<br>(Seite<br>65) definierte Datenbank, die Eingabe<br>tabelle beinhaltet. |
| tableName •                                                               | Zeichenkette       | Der Name der Eingabetabelle. Für SQL<br>Datenbanken kann auch der Name einer<br>Sicht (View) eingegeben werden.          |

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| orderBy                   | Zeichenkette       | Der Spaltenname bzw. eine durch Komma<br>getrennte Liste der Spaltennamen, nach<br>denen die Datensätze beim Lesen aus der<br>Eingabetabelle sortiert werden. Die Sor<br>tierungsreihenfolge kann in Kombination<br>mit den Spaltennamen durch "DESC", "ASC",<br>"NULLS FIRST", "NULLS LAST" eingestellt wer<br>den. Für SQL Server sind "NULLS FIRST",<br>"NULLS LAST" nicht unterstüzt. |
| whereClause               | Zeichenkette       | Eine Filterbedingung für die Daten in der<br>Eingabetabelle. Bei der Referenzierung einer<br>SQL-Datenbank muss die definierte Bedin<br>gung einer validen WHERE-Clause im SQL<br>Kontext des jeweiligen Datenbank-Typs ent<br>sprechen.                                                                                                                                                  |

Ein Abschnitt von <databaseInput> beinhaltet einen oder mehrere Einträge vom Typ <databaseInputField>, welche die eigentlichen Spalten in der Eingabetabelle mit ihren Eigenschaften repräsentieren.

<span id="page-79-0"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                          |
|---------------------------|--------------------|--------------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Feldes                                |
| name •                    | Zeichenkette       | Name einer Tabellenspalte                                          |
| defaultValue              | Zeichenkette       | Vorgabewert, falls das Feld in der Eingabe<br>nicht gefüllt ist    |
| synonymList               | Zeichenkette       | Name einer Synonymliste. Muss in der Syn<br>onymdatei vorkommen.   |
| transLitIdent             | Zeichenkette       | Ein Bezeichner, der die vorzunehmende<br>Transliteration definiert |

#### Tabelle 8.15: Konfigurationselement <databaseInputField>

| Attribut<br>• Pflichtfeld | <b>Werte</b><br>* Default                                                                        | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|---------------------------|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| trimType                  | N=None<br>L=Left<br>R=Right<br>B=Both*<br>M=Middle<br>LM=Left-Middle<br>RM=Right-Middle<br>A=All | Entfernen von Leerzeichen (deaktiviert, links,<br>rechts, beidseitig (links und rechts), mittig<br>(mehrfach auftretende Leerzeichen werden<br>auf ein einzelnes reduziert), links und mittig,<br>rechts und mittig, sowie generelles Entfernen<br>von mehrfachen, führenden und schließen-<br>den Leerzeichen)                                                                                                                                                                                                                                                                  |
| type                      | C=Characters*,<br>D=Date,<br>L=List                                                              | Bestimmt den Eingabetyp des Feldes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| pattern                   | Zeichenkette                                                                                     | Je nach Typ (type) wird hier das Mus-<br>ter der Eingabewerte angegeben.<br>'C': keine Auswirkung;<br>'D': eine durch Komma getrennte Liste<br>von Datumsformaten, wobei jedes For-<br>mat einen Präferenzwert haben kann,<br>z.B. "dd.MM.yyyy (100), yyyy/MM/dd (80)"<br>(mehr Infos unter https://docs.oracle.<br>com/javase/8/docs/api/java/time/<br>format/DateTimeFormatter.html);<br>'L': kommaseparierte Liste von<br>erlaubten Eingabewerten, z.B.<br>"Berlin, Hamburg, München".<br>Anpassungen entsprechend Attribut<br>trimType erfolgen <i>vor</i> der Verarbeitung. |
| length                    | Zahl (Default ist 0)                                                                             | Die maximale Länge eines Eingabefeldes. Der<br>Wert 'O' bedeutet, dass keine Längenprüfung<br>stattfindet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| lengthErrorAction         | NONE<br>ERROR*<br>CLIP                                                                           | Die Behandlung, wenn die eingestellte Län-<br>ge des Feldes überschritten wird. Bei ERROR<br>wird ein Fehler ausgegeben, bei CLIP wird<br>der Wert des Eingabefeldes auf die eingestell-<br>te Länge abgeschnitten und bei NONE gibt es<br>keine Einschränkung.                                                                                                                                                                                                                                                                                                                  |

Beispiel:

1 <databaseInput id="dbInput-1" databaseRef="db-1" tableName="input">

#### KAPITEL 8. KONFIGURATIONSDATEI **TOLERANT** Software

```
2 <databaseInputField id="databaseInputField -2" name="street" />
3 <databaseInputField id="databaseInputField -3" name="houseno" />
4 <databaseInputField id="databaseInputField -4" name="zipcode" />
5 <databaseInputField id="databaseInputField -5" name="city" trimType="B" />
6 <databaseInputField id="databaseInputField -6" name="country" />
7 </ databaseInput >
```

<img src="data:image/jpeg;base64,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" alt="" style="max-width: 100%;">

## <span id="page-81-0"></span>**8.9 AUSGABEFELDER (DATEI)**

Hier teilen Sie dem Programm mit, welche Felder in der Ausgabe erscheinen. Das eigentliche Mapping, also die Zuweisung der definierten Felder, wird an dieser Stelle noch nicht vorgenommen. Ein Abschnitt von <csvFileOutput> beinhaltet immer einen oder mehrere Einträge von <csvOutputField>, welche die eigentlichen Felder repräsentieren.

<span id="page-81-2"></span><span id="page-81-1"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                                         | Bedeutung                                                                                                                                                                             |
|---------------------------|------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette                                                                                               | ID zur Referenzierung dieses Abschnitts                                                                                                                                               |
| filepath •                | Zeichenkette                                                                                               | Pfad der Ausgabedatei                                                                                                                                                                 |
| separator •               | Zeichen, z.B. ';'                                                                                          | In der Ausgabedatei verwendetes Trennzei<br>chen zwischen Feldern. Soll ein Tabulatorzei<br>chen verwendet werden, so benutzen Sie die<br>folgende Notation '\t'.                     |
| enclosure                 | Zeichen, z.B. '"'                                                                                          | Texterkennungszeichen. Wird vor und nach<br>dem Feldtext ausgegeben, falls definiert. Soll<br>te verwendet werden, falls das Trennzeichen<br>innerhalb des Feldtextes vorkommen kann. |
| escape                    | Zeichen, z.B. '\'                                                                                          | Escapesymbol. Wird vor Trennzeichen oder<br>Texterkennungszeichen verwendet, falls diese<br>innerhalb des Feldtextes vorkommen.                                                       |
| codepage                  | z.B. ISO8859-1*, UTF<br>8, etc. (s.<br>Unterstütz<br>(Seite<br>221))<br>te Codesets<br>(Default=ISO8859-1) | In der Ausgabedatei verwendete Zeichenko<br>dierung                                                                                                                                   |
| headline                  | Y*<br>N                                                                                                    | Steuert, ob eine Kopfzeile (mit Hilfe der<br><csvoutputfield>-Elemente) erzeugt wird<br/>oder nicht</csvoutputfield>                                                                  |

| Attribut<br>• Pflichtfeld | <b>Werte</b><br>* Default                                                                        | Bedeutung                                                                                                                                                                                                                                                                                                       |
|---------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id•                       | Zeichenkette                                                                                     | Referenz des Feldes                                                                                                                                                                                                                                                                                             |
| name                      | Zeichenkette                                                                                     | Name des Feldes. Wird der Name weggelas-<br>sen, so wird der Name des ersten internen<br>Feldes genommen, das auf dieses Ausgabe-<br>feld kopiert wird. Die Namen können zur<br>Generierung der Kopfzeile der Ausgabe her-<br>angezogen werden.                                                                 |
| defaultValue              | Zeichenkette                                                                                     | Vorgabewert, falls das Feld nicht gefüllt ist                                                                                                                                                                                                                                                                   |
| synonymList               | Zeichenkette                                                                                     | Name einer Synonymliste. Muss in der Syn-<br>onymdatei vorkommen.                                                                                                                                                                                                                                               |
| transLitIdent             | Zeichenkette                                                                                     | Ein Bezeichner, der die vorzunehmende<br>Transliteration definiert                                                                                                                                                                                                                                              |
| trimType                  | N=None<br>L=Left<br>R=Right<br>B=Both*<br>M=Middle<br>LM=Left-Middle<br>RM=Right-Middle<br>A=All | Entfernen von Leerzeichen (deaktiviert, links,<br>rechts, beidseitig (links und rechts), mittig<br>(mehrfach auftretende Leerzeichen werden<br>auf ein einzelnes reduziert), links und mittig,<br>rechts und mittig, sowie generelles Entfernen<br>von mehrfachen, führenden und schließen-<br>den Leerzeichen) |
| delimiter                 | Zeichenkette (Default ist<br>" "(Leerzeichen)).                                                  | Werden dem Ausgabefeld mehrere Werte in<br>der Outputmap zugewiesen, so gibt das Attri-<br>but "delimiter" an, welche Zeichen zwischen<br>den Werten ausgegeben werden sollen. Die<br>Zeichenkette kann leer sein.                                                                                              |
| type                      | C=Characters*,<br>D=Date                                                                         | Bestimmt den Ausgabetyp des Feldes.                                                                                                                                                                                                                                                                             |
| pattern                   | Zeichenkette                                                                                     | Je nach Typ ( <i>type</i> ) wird hier das Mus-<br>ter der Ausgabewerte angegeben.<br>'C': keine Auswirkung<br>'D': das Datumsformat z.B. "dd.MM.yyyy"<br>(mehr Infos unter http://docs.oracle.<br>com/javase/6/docs/api/java/text/<br>SimpleDateFormat.html)                                                    |

| Tabelle 8.17: Konfigurationselement <csv0utputfield></csv0utputfield> |
|-----------------------------------------------------------------------|
|                                                                       |

| Attribut<br>• Pflichtfeld | Werte<br>* Default   | Bedeutung                                                                                                                                                                                                                |
|---------------------------|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| length                    | Zahl (Default ist 0) | Die maximale Länge eines Ausgabefeldes.<br>Der Wert '0' bedeutet, dass keine Längen<br>prüfung stattfindet. Beim Überschreiten der<br>Länge wird der Wert des Ausgabefeldes auf<br>die eingestellte Länge abgeschnitten. |

Beispiel:

| 1  | <csvfileoutput id="csvFileOutput -1"></csvfileoutput> |                                                                                           |
|----|-------------------------------------------------------|-------------------------------------------------------------------------------------------|
| 2  |                                                       | <csvfileoutputfield id="csvFileOutputField -2"></csvfileoutputfield>                      |
| 3  |                                                       | <csvfileoutputfield id="csvFileOutputField -3" name="city"></csvfileoutputfield>          |
| 4  |                                                       | <csvfileoutputfield id="csvFileOutputField -4" name="zipcode"></csvfileoutputfield>       |
| 5  |                                                       | <csvfileoutputfield id="csvFileOutputField -5" name="building"></csvfileoutputfield>      |
| 6  |                                                       | <csvfileoutputfield id="csvFileOutputField -6" name="street"></csvfileoutputfield>        |
| 7  |                                                       | <csvfileoutputfield id="csvFileOutputField -7" name="number"></csvfileoutputfield>        |
| 8  |                                                       | <csvfileoutputfield id="csvFileOutputField -9" name="subcity"></csvfileoutputfield>       |
| 9  |                                                       | <csvfileoutputfield id="csvFileOutputField -10" name="latitude"></csvfileoutputfield>     |
| 10 |                                                       | <csvfileoutputfield id="csvFileOutputField -11" name="longitude"></csvfileoutputfield>    |
| 11 |                                                       | <csvfileoutputfield id="csvFileOutputField -12" name="quality"></csvfileoutputfield>      |
| 12 |                                                       | <csvfileoutputfield id="csvFileOutputField -13" name="short_report"></csvfileoutputfield> |
| 13 |                                                       | <csvfileoutputfield id="csvFileOutputField -14" name="returncode"></csvfileoutputfield>   |
| 14 |                                                       |                                                                                           |
|    |                                                       |                                                                                           |

Listing 8.8: Beispielkonfiguration der Ausgabefelder

## <span id="page-83-0"></span>**8.10 AUSGABEFELDER (DATENBANK)**

Hier teilen Sie dem Programm mit, welche Felder in der Ausgabe in einer Datenbanktabelle erscheinen. Das eigentliche Mapping, also die Zuweisung der definierten Felder, wird an dieser Stelle noch nicht vorgenommen. Ein Abschnitt von <databaseOutput> beinhaltet immer einen oder mehrere Einträge von <databaseOutputField>, welche die eigentlichen Felder repräsentieren.

<span id="page-83-1"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                    |
|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Abschnitts                                                                                      |
| databaseRef •             | Zeichenkette       | Verweis auf eine im Abschnitt<br>'Datenbanken'<br>(Seite<br>65) definierte Datenbank, die die Ein<br>gabetabelle beinhaltet. |
| tableName •               | Zeichenkette       | Name der Datenbanktabelle für die Ausgabe.                                                                                   |

Tabelle 8.18: Konfigurationselement <databaseOutput>

#### **TOLERANT** Software

| Attribut<br>• Pflichtfeld | <b>Werte</b><br>* Default | Bedeutung                                                                           |
|---------------------------|---------------------------|-------------------------------------------------------------------------------------|
| truncateTable             | Y<br>N*                   | Y = Der Inhalt der Ausgabetabelle wird vor<br>erneuter Batch-Verarbeitung gelöscht. |

#### Tabelle 8.19: Konfigurationselement <databaseOutputField>

<span id="page-84-0"></span>

| • Pflichtfeld | <b>Werte</b><br>* Default                                                                        | Bedeutung                                                                                                                                                                                                                                                                                                       |
|---------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id•           | Zeichenkette                                                                                     | ID zur Referenzierung dieses Feldes                                                                                                                                                                                                                                                                             |
| name •        | Zeichenkette                                                                                     | Name einer Tabellenspalte                                                                                                                                                                                                                                                                                       |
| defaultValue  | Zeichenkette                                                                                     | Vorgabewert, falls das Feld nicht gefüllt ist                                                                                                                                                                                                                                                                   |
| synonymList   | Zeichenkette                                                                                     | Name einer Synonymliste. Muss in der Syn-<br>onymdatei vorkommen.                                                                                                                                                                                                                                               |
| transLitIdent | Zeichenkette                                                                                     | Ein Bezeichner, der die vorzunehmende<br>Transliteration definiert                                                                                                                                                                                                                                              |
| trimType      | N=None<br>L=Left<br>R=Right<br>B=Both*<br>M=Middle<br>LM=Left-Middle<br>RM=Right-Middle<br>A=All | Entfernen von Leerzeichen (deaktiviert, links,<br>rechts, beidseitig (links und rechts), mittig<br>(mehrfach auftretende Leerzeichen werden<br>auf ein einzelnes reduziert), links und mittig,<br>rechts und mittig, sowie generelles Entfernen<br>von mehrfachen, führenden und schließen-<br>den Leerzeichen) |
| delimiter     | Zeichenkette (Default ist<br>""(Leerzeichen)).                                                   | Werden dem Ausgabefeld mehrere Werte in<br>der Outputmap zugewiesen, so gibt das Attri-<br>but "delimiter" an, welche Zeichen zwischen<br>den Werten ausgegeben werden sollen. Die<br>Zeichenkette kann leer sein.                                                                                              |
| type          | C=Characters*,<br>D=Date                                                                         | Bestimmt den Ausgabetyp des Feldes.                                                                                                                                                                                                                                                                             |

| Attribut<br>• Pflichtfeld | Werte<br>* Default   | Bedeutung                                                                                                                                                                                                                                             |
|---------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| pattern                   | Zeichenkette         | Je nach Typ (type) wird hier das Mus<br>ter der Ausgabewerte angegeben.<br>'C': keine Auswirkung<br>'D': das Datumsformat z.B. "dd.MM.yyyy"<br>(mehr Infos unter<br>http://docs.oracle.<br>com/javase/6/docs/api/java/text/<br>SimpleDateFormat.html) |
| length                    | Zahl (Default ist 0) | Die maximale Länge eines Ausgabefeldes.<br>Der Wert '0' bedeutet, dass keine Längen<br>prüfung stattfindet. Beim Überschreiten der<br>Länge wird der Wert des Ausgabefeldes auf<br>die eingestellte Länge abgeschnitten.                              |

Beispiel:

| 1  | <databaseoutput databaseref="db-1" id="dbOutput -1" truncatetable="Y"></databaseoutput>      |
|----|----------------------------------------------------------------------------------------------|
| 2  | <databaseoutputfield id="databaseOutputField -3" name="city"></databaseoutputfield>          |
| 3  | <databaseoutputfield id="databaseOutputField -4" name="zipcode"></databaseoutputfield>       |
| 4  | <databaseoutputfield id="databaseOutputField -5" name="building"></databaseoutputfield>      |
| 5  | <databaseoutputfield id="databaseOutputField -6" name="street"></databaseoutputfield>        |
| 6  | <databaseoutputfield id="databaseOutputField -7" name="number"></databaseoutputfield>        |
| 7  | <databaseoutputfield id="databaseOutputField -9" name="subcity"></databaseoutputfield>       |
| 8  | <databaseoutputfield id="databaseOutputField -10" name="latitude"></databaseoutputfield>     |
| 9  | <databaseoutputfield id="databaseOutputField -11" name="longitude"></databaseoutputfield>    |
| 10 | <databaseoutputfield id="databaseOutputField -12" name="quality"></databaseoutputfield>      |
| 11 | <databaseoutputfield id="databaseOutputField -13" name="short_report"></databaseoutputfield> |
| 12 | <databaseoutputfield id="databaseOutputField -14" name="returncode"></databaseoutputfield>   |
| 13 |                                                                                              |

Listing 8.9: Beispielkonfiguration der Ausgabefelder

## <span id="page-85-0"></span>**8.11 POST-PROFILE**

Jede postalische Validierung benötigt noch zusätzliche Angaben über die Art der Prüfung. Diese Angaben werden in einem (Post-)Profil gespeichert. Ein Projekt kann mehrere Profile enthalten.

<span id="page-85-1"></span>Mit der Einführung der Version 6 der Validierungs-Engine (ab Produktversion 11.0) wurden einige Profilparameter abgelöst, während einige neue dazu kamen. Im Folgenden sind einige Parameter mit "V5\_ONLY" bzw. "V6\_ONLY" gekennzeichnet, um dies zu verdeutlichen. Profilparameter, die nicht zur Version der Validierungs-Engine passen, erzeugen beim Überprüfung der Konfiguration eine Warnung, und werden inhaltlich ignoriert.

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                | Bedeutung                                                                                                                                                                                                                                     |
|---------------------------|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      |                                                   | Profil-ID. Diese ID muss beim Aufruf des<br>Batch-Programms verwendet werden, um<br>dieses Profil anzusprechen.                                                                                                                               |
| matchingAlternatives      | NONE,<br>SYNONYMS_ONLY,<br>ARCHIVES_ONLY,<br>ALL* | Ermöglicht die Unterdrückung<br>von Abgleichsalternativen.<br>ALL: Abgleich gegen Adress<br>Synonyme und Archive;<br>NONE: Kein Abgleich gegen<br>Adress-Synonyme und Archive;<br>SYNONYMS_ONLY: Nur Synonyme;<br>ARCHIVES_ONLY: Nur Archive. |

Tabelle 8.20: Konfigurationselement <postProfile>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                                             | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------------------|----------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| matchingScope             | LOCALITY_LEVEL,<br>STREET_LEVEL,<br>DELIVERYPOINT_LEVEL,<br>AUTO,<br>ALL*,<br>PROVINCE_LEVEL,<br>ZIPCODE_LEVEL | Prüft die Adresse nur bis zu<br>einer bestimmten Ebene.<br>LOCALITY_LEVEL: Ortsebene;<br>STREET_LEVEL: Straßenebene;<br>DELIVERYPOINT_LEVEL:<br>Hausnummer und Gebäude<br>AUTO: Validierung einer Adresse mit<br>matchingScopeFrom. Gibt dies kein<br>ausreichend gutes Ergebnis, so wird der<br>nächste Level probiert, bis<br>matchingS<br>erreicht wird. Dies kann benutzt<br>copeTo<br>werden, wenn bspw. die Hausnummer<br>nicht bekannt ist, oder eventuell in der<br>angegebenen Straße nicht existiert. Beachten<br>Sie, dass die Benutzung von AUTO zu einer<br>Verschlechterung der Performance führen<br>kann, da die Adresse eventuell zwei- oder<br>dreimal validiert wird. Die Reihenfolge<br>der Level ist ALL, DELIVERYPOINT_LEVEL,<br>STREET_LEVEL und zuletzt LOCALITY_LEVEL.<br>PROVINCE_LEVEL: Der Provinzlevel kann<br>nur im Zusammenhang mit dem Modus<br>'LIST' verwendet werden, dann werden zu<br>einer Suchanfrage die möglichen Provinzen/-<br>Bundesländer ausgegeben. Für die anderen<br>Modi wird dieser Scope nicht unterstützt.<br>ZIPCODE_LEVEL: Dieser Scope kann im Mo<br>dus 'LIST' verwendet werden, dann werden<br>Orte mit Postleitzahl geliefert. Für die ande<br>ren Modi wird dieser Scope nicht unterstützt. |
| matchingScopeFrom         | ALL,<br>DELIVERYPOINT_LEVEL,<br>STREET_LEVEL*                                                                  | Wenn<br>auf AUTO steht, dann<br>matchingScope<br>gibt dies den ersten Level an, der probiert<br>werden soll.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| matchingScopeTo           | DELIVERYPOINT_LEVEL,<br>STREET_LEVEL,<br>LOCALITY_LEVEL*                                                       | Wenn<br>auf AUTO steht, dann<br>matchingScope<br>gibt dies den letzten Level an, der probiert<br>werden soll.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

| Attribut<br>• Pflichtfeld      | Werte<br>* Default             | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|--------------------------------|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| optimizationLevel<br>(V5_ONLY) | NARROW,<br>STANDARD*,<br>WIDE  | Bestimmt das Verhältnis zwischen<br>Verarbeitungsgeschwindigkeit und Qua<br>lität der verifizierten Ausgabe. Für die<br>Engine Version V6 wird automatisch<br>ein optimierter Mittelweg gewählt.                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                |                                | STANDARD: Mittelweg zwischen Per<br>formance und Qualität der Ergebnisse.<br>NARROW: Wenig tolerant, liefert aber<br>eine hohe Verarbeitungsperformance.<br>WIDE: Liefert qualitativ und quan<br>titativ die besten Ergebnisse, da<br>alle möglichen Elemente analysiert<br>und unabhängig überprüft werden.<br>Wenn Sie die Optimierungsstufe auf<br>NARROW festlegen, erfordert die Adressüber<br>prüfung minimale Verarbeitungsressourcen<br>im Vergleich zur Optimierungsstufe WIDE.<br>Die Engine Version V6 wählt hier auto<br>matisch einen optimierten Mittelweg, das<br>Attribut ist für diese Version obsolet. |
| defaultCountry                 | ISO3166 alpha-3 Länder<br>code | Hier kann ein Ländercode (ISO3166 alpha-3)<br>angegeben werden, der benutzt wird, wenn<br>in der <inputfieldmap>kein post.Country*<br/>Feld vorkommt.</inputfieldmap>                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

| Attribut<br>• Pflichtfeld        | Werte<br>* Default                                                                                                                       | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| countryList                      | Zeichenkette von ISO<br>3166 alpha-2 oder alpha<br>3 Codes getrennt durch<br>Kommata. Weiterhin ist<br>'*' am Ende der Liste er<br>laubt | Die countryList kann verwendet werden, um<br>Eingabedateien zu verarbeiten, bei denen<br>Daten aus mehreren Ländern vorkommen<br>und diese nicht immer ausgefüllt sind. Die<br>Reihenfolge in dem Attribut gibt an, in wel<br>cher Reihenfolge die Länder überprüft wer<br>den sollen. Als letzter Eintrag in der Liste<br>kann '*' verwendet werden. Sind bspw. die<br>Referenzdaten für 'DE','FR','NLD','TUR' hin<br>terlegt, in der Liste ist aber nur "DE,FR,*"<br>angegeben, so werden im Falle, dass das Län<br>derfeld nicht gesetzt ist, zuerst 'DE', dann 'FR'<br>und dann 'NLD' und 'TUR' angewendet. Der<br>Stern gilt als Wildcard für alle Länder, in der<br>alphabetischen Reihenfolge. Mehr Details da<br>zu finden Sie in Abschnitt<br>'Länderermittlung'<br>(Seite<br>14). |
| countryFirstMatch                | Y(=Yes)<br>N(=No)*                                                                                                                       | Ist eine countryList angegeben, so steuert<br>"countryFirstMatch"<br>die Vorgehensweise.<br>Ist der Wert 'Y', so wird sofort abgebrochen,<br>wenn eine gültige Adresse für ein Land ge<br>funden wurde. Ist der Wert 'N', so werden<br>alle Länder überprüft und nur die besten<br>Ergebnisse werden zurückgegeben. Mehr<br>Details dazu finden Sie in Abschnitt<br>'Länder<br>(Seite<br>14).<br>ermittlung'                                                                                                                                                                                                                                                                                                                                                                                |
| countryBest                      | Y(=Yes)<br>N(=No)*                                                                                                                       | Wenn countryFirstMatch eingeschaltet ist<br>und keine gültige Adresse gefunden wurde,<br>dann wird die Adresse mit der besten Bewer<br>tung eines der Länder zurückgegeben, aber<br>keine Liste.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| countryNameLanguage<br>(V6_ONLY) | ISO3166 alpha-3 Länder<br>code                                                                                                           | Steuert die Übersetzung des post.Country<br>Feldes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| formatWithCountry<br>(V6_ONLY)   | N(=No)*<br>Y(=Yes)                                                                                                                       | Legt fest, für eine Adresse (beispielsweise in<br>post.AddressComplete) am Ende das land mit<br>ausgegeben wird. Ersetzt inputFormatWith<br>Country und outputFormatWithCountry der<br>V5 Adressvalidierungs-Engine                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

| Attribut<br>• Pflichtfeld           | Werte<br>* Default                                                                                                                 | Bedeutung                                                                                                                                                                                                                                                              |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inputFormatType<br>(V5_ONLY)        | ALL*,<br>ADDRESS_ONLY,<br>WITH_ORGANIZATION,<br>WITH_CONTACT,<br>WITH_ORGANIZA<br>TION_CONTACT,<br>WITH_ORGANIZATION<br>DEPARTMENT | Wenn die Felder post.AddressLine* oder<br>post.AddressComplete als Eingangsdaten<br>verwendet werden, kann hierüber angege<br>ben werden, ob die Adresszeile Organisation<br>und/oder Kontaktperson enthalten. Wird für<br>V6 nicht mehr benötigt.                     |
| inputFormatDelimiter                | CRLF (V5_ONLY)*,<br>LF (V5_ONLY),<br>CR (V5_ONLY),<br>SEMICOLON,<br>COMMA,<br>TAB,<br>PIPE (V5_ONLY),<br>SPACE (V5_ONLY)           | Wenn die Felder post.AddressLine* oder<br>post.AddressComplete als Eingangsdaten<br>verwendet werden, kann hierüber angege<br>ben werden, welches Zeichen innerhalb der<br>Adresszeile als Trennzeichen benutzt werden<br>soll. Für die V6 ist der Defaultwert "COMMA" |
| inputFormatWithCountry<br>(V5_ONLY) | N(=No)*<br>Y(=Yes)                                                                                                                 | Wenn die Felder post.AddressLine* oder<br>post.AddressComplete als Eingangsdaten ver<br>wendet werden, kann hierüber angegeben<br>werden, ob das Land Teil der Adresszeile ist.<br>Wird abgelöst von<br>in<br>formatWithCountry<br>V6.                                 |
| formatAddressComplete               | FORMATTED<br>ADDRESS_LINE*<br>SINGLE_LINE                                                                                          | Wenn das Feld post.AddressComplete in den<br>Eingangsdaten verwendet wird, kann hier<br>über angegeben werden, ob das Feld eine<br>vollständige Adresszeile beinhaltet.                                                                                                |
| outputFormatType<br>(V5_ONLY)       | ALL*,<br>ADDRESS_ONLY,<br>WITH_ORGANIZATION,<br>WITH_CONTACT,<br>WITH_ORGANIZA<br>TION_CONTACT,<br>WITH_ORGANIZATION<br>DEPARTMENT | Wenn die Felder post.AddressLine* oder<br>post.AddressComplete als Ausgangsdaten<br>verwendet werden, kann hierüber angege<br>ben werden, ob die Adresszeile Organisation<br>und/oder Kontaktperson enthalten soll. Wird<br>für V6 nicht mehr benötigt.                |

#### KAPITEL 8. KONFIGURATIONSDATEI **TOLERANT** Software

| Attribut<br>• Pflichtfeld            | Werte<br>* Default                                                                                                                                                          | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| outputFormatDelimiter                | CRLF (V5_ONLY)*,<br>LF (V5_ONLY),<br>CR (V5_ONLY),<br>SEMICOLON,<br>COMMA,<br>TAB,<br>PIPE (V5_ONLY),<br>SPACE                                                              | Wenn die Felder post.AddressLine* oder<br>post.AddressComplete als Ausgangsdaten<br>verwendet werden, kann hierüber angege<br>ben werden, welches Zeichen innerhalb der<br>Adresszeile als Trennzeichen benutzt werden<br>soll. Für die V6 ist der Defaultwert "SEMIKO<br>LON".                                                                                                                                                                              |
| outputFormatWithCountry<br>(V5_ONLY) | N(=No)*<br>Y(=Yes)                                                                                                                                                          | Wenn die Felder post.AddressLine* oder<br>post.AddressComplete als Ausgangsdaten ver<br>wendet werden, kann hierüber angegeben<br>werden, ob das Land Teil der Adresszeile ist.<br>Wird abgelöst von<br>in<br>formatWithCountry<br>V6.                                                                                                                                                                                                                       |
| decimalSeparator                     | COMMA<br>LOCALE<br>PERIOD*                                                                                                                                                  | Der angegebene Dezimalseparator wird ver<br>wendet, um Nachkommastellen bei Fließ<br>kommazahlen (bspw. Geo-Koordinaten) ab<br>zutrennen. Es kann entweder der Separator<br>fest eingestellt sein, also COMMA oder PERI<br>OD, oder es wird der Wert genommen, der<br>durch die Laufzeitumgebung (LOCALE) fest<br>gelegt wird. Bei Windows sind dies die Regio<br>naleinstellungen und bei LINUX oder UNIX<br>die Werte von<br>oder<br>LC_NUMERIC.<br>LC_ALL |
| mode •                               | BATCH,<br>INTERACTIVE,<br>FASTCOMPLETION,<br>FASTCOMPLETION_EXT,<br>PARSE,<br>CERTIFIED,<br>LIST,<br>STABLIST,<br>AGSLIST,<br>STABSEARCH,<br>AGSSEARCH,<br>GEOCODETOADDRESS | Die verschiedenen Modi sind im Abschnitt<br>(Seite<br>10) erklärt.<br>'Validierungs- und Suchmodi'                                                                                                                                                                                                                                                                                                                                                           |

| Attribut<br>• Pflichtfeld            | <b>Werte</b><br>* Default                                                                         | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|--------------------------------------|---------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| casingType                           | NATIVE*,<br>UPPER,<br>LOWER,<br>MIXED,<br>NOCHANGE                                                | Eine Adresse wird entweder gemäß den Lan-<br>desregeln (NATIVE) ausgegeben, oder in<br>Großschreibung (UPPER), komplett in Klein-<br>schreibung (LOWER), gemischt (MIXED) oder<br>ohne Änderungen (NOCHANGE)                                                                                                                                                                                                                                                                                                                |
| elementAbbreviation                  | N(=No)*<br>Y(=Yes)                                                                                | Sollen die Ausgaben für Straße, Ort verkürzt<br>werden? Es werden, sofern in den Referenz-<br>daten vorhanden, die landesüblichen Abkür-<br>zungen verwendet.                                                                                                                                                                                                                                                                                                                                                               |
| standardizeInvalidAddre              | sses N(=No)*<br>Y(=Yes)                                                                           | Soll eine nicht gültige Adresse trotzdem nor-<br>miert werden?                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| maxResultCount                       | 1- <i>N</i> , wobei <i>N</i> von der<br>verwendeten Post-<br>Engine abhängig ist<br>(Default: 20) | Die maximale Ergebnisgröße einer Suche.<br>Mit der internen Engine ist es zurzeit nicht<br>möglich, mehr als 100 Resultate zu erhalten.                                                                                                                                                                                                                                                                                                                                                                                     |
| rangesToExpand                       | NONE*,<br>ALL,<br>ONLY_WITH_VA-<br>LID_ITEMS                                                      | Wenn für ein Land individuelle Hausnum-<br>mern in den Referenzdaten vorliegen (bspw.<br>United Kingdom), dann kann man im Fast<br>Completion Modus hiermit erzwingen, dass<br>die einzelnen Ergebnisse für einzelne Haus-<br>nummern ausgegeben werden. "ALL" er-<br>zwingt die Expandierung der Hausnummern-<br>bereiche für alle Adressen, bei denen einzel-<br>ne Hausnummern für den Bereich vorliegen<br>"ONLY_WITH_VALID_ITEMS" expandiert nur,<br>wenn die einzelnen Hausnummern in den<br>Referenzdaten vorliegen. |
| restrictOutputToAddress <sup>¬</sup> | ГуреN(=No)*<br>Y(=Yes)                                                                            | Ist "Y" konfiguriert, dann werden je nach<br>"AddressType" verschiedene Felder in der<br>Ausgabe unterdrückt. Beispielsweise wird<br>für den "AddressType" "L" (Großempfänger)<br>die Straße und Hausnummer im Ergebnis<br>unterdrückt. Diese Einstellungen gelten nur<br>für Deutschland.                                                                                                                                                                                                                                  |

#### KAPITEL 8. KONFIGURATIONSDATEI

#### **TOLERANT** Software

| Attribut<br>• Pflichtfeld       | <b>Werte</b><br>* Default    | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                         |
|---------------------------------|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| improveAddressType<br>(V5_ONLY) | N(=No)*<br>Y(=Yes)           | Ist "Y" eingestellt, dann wird bei Adressen,<br>bei denen der Adresstyp nicht korrekt be-<br>stimmt werden kann, eine weitere Suche<br>im Modus FASTCOMPLETION durchgeführt.<br>Dazu müssen die FASTCOMPLETION Daten<br>vorliegen und konfiguriert sein. Diese Ein-<br>stellung verringert die Geschwindigkeit und<br>ist momentan nur dafür ausgelegt, bessere<br>Ergebnisse bei Großempfänger-Adressen zu<br>liefern.           |
| correctPseudoHouseNoF           | Ranges<br>N(=No)<br>Y(=Yes)* | Ist dieses Flag eingeschaltet, so wird bei<br>Hausnummern untersucht, ob eventuell<br>ein unzulässiger Hausnummernbereich (bei-<br>spielsweise 7-1) angegeben wurde. Ist dies<br>der Fall, so wird das Minuszeichen durch<br>einen Schrägstrich ("/") ersetzt. Dies ge-<br>schieht immer dann, wenn der Zahlenwert<br>hinter dem Minus kleiner oder gleich dem<br>vorderen Zahlenwert ist.                                        |
| splitHouseNoExtensions          | s N(=No)<br>Y(=Yes)*         | Ist dieses Flag eingeschaltet, so wird bei<br>Hausnummern untersucht, ob diese einen<br>Zusatz enthalten (bspw. "A" oder "App.<br>203"). Dieser wird dann vor der Adress-<br>validierung entfernt und erst im An-<br>schluss wieder angehängt. Zurzeit funk-<br>tioniert dies nur für deutsche Adressen.<br>Achtung!: Wird dies verwendet, so fehlt<br>der Hausnummernzusatz, wenn man<br>post.AddressLine als Ausgabe verwendet. |

| Attribut<br>• Pflichtfeld   | Werte<br>* Default                                                         | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|-----------------------------|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| storeHouseNoExtension       | SEPARATE_FIELD<br>APPEND_WITH_SPACE<br>APPEND<br>APPEND_ASININPUT*<br>OMIT | Ist splitHouseNoExtensions eingeschaltet, so<br>wird hiermit gesteuert, was mit dem Haus<br>nummernzusatz passieren soll. Wird OMIT<br>verwendet, so wird der Hausnummernzu<br>satz gelöscht. Wird SEPARATE_FIELD ver<br>wendet, so wird der Hausnummernzusatz<br>in "post.NumberExt"<br>gespeichert, ansonsten<br>wird der Hausnummernzusatz wieder an die<br>Hausnummer gehängt ("post.Number"<br>Feld):<br>Bei APPEND_WITH_SPACE mit einem Leerzei<br>chen, bei APPEND ohne Trennzeichen und<br>bei APPEND_ASININPUT, so wie es im Input<br>an der Hausnummer hing. |
| uniqueOutput                | N(=No)*<br>Y(=Yes)                                                         | Bei der Ausgabe werden doppelte Einträge<br>vermieden, wenn<br>auf "Y"<br>ge<br>uniqueOutput<br>setzt ist. Doppelte Einträge können entste<br>hen, wenn beispielsweise nicht alle Ausga<br>befelder ausgegeben werden, die Adressen<br>sich aber in einem nicht ausgegebenen Feld<br>unterscheiden würden.                                                                                                                                                                                                                                                             |
| additionalData<br>(V6_ONLY) | NO*<br>FOR_ALL_ELEMENTS<br>FOR_POSTAL_RELE<br>VANT_ELEMENTS                | Gibt an, ob zusätzliche Daten<br>für verifizierte oder korrigierte<br>Adressen zurückgeliefert werden.<br>Wird "FOR_POSTAL_RELEVANT_ELE<br>MENTS" gewählt, werden Zusatzda<br>ten ermittelt solange sie für die po<br>stalische Zustellung relevant sind.<br>Für "FOR_ALL_ELEMENTS werden alle ver<br>fügbaren Zusatzdaten ermittelt (z.B. "Flats"<br>für englische Adressen).                                                                                                                                                                                         |
| maxStreetLength             | 20-∞<br>(Default 0)                                                        | Überschreitet der Straßenname einer vali<br>dierten Adresse die konfigurierte Länge, so<br>wird die Adresse nochmal mit<br>elementAb<br>auf 'ON' gesetzt validiert (keine<br>breviation<br>Beschränkung mit '0')                                                                                                                                                                                                                                                                                                                                                       |

| Attribut<br>• Pflichtfeld       | Werte<br>* Default                                                                                                                          | Bedeutung                                                                                                                                                                                                                                                                                                                                  |
|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| maxCityLength                   | 20-∞<br>(Default 0)                                                                                                                         | Überschreitet der Stadtname einer validier<br>ten Adresse die konfigurierte Länge, so wird<br>die Adresse nochmals mit<br>elementAbbre<br>auf 'ON' gesetzt validiert (keine Be<br>viation<br>schränkung mit '0')                                                                                                                           |
| minResultPercentage             | Zahl zwischen 0 und 100<br>(Default=75.0)                                                                                                   | Gibt an, wie ähnlich die korrigierte Adresse<br>der Originaladresse sein muss, um noch als<br>gültig angesehen zu werden                                                                                                                                                                                                                   |
| specialReturnCodes              | N(=No)*<br>Y(=Yes)                                                                                                                          | Wenn eingeschaltet, werden spezielle Return<br>codes (s. Abschnitt<br>(Seite<br>120)) verwendet,<br>''<br>wenn die Hausnummernprüfung fehlschlägt<br>oder keine Referenzdaten für Hausnummern<br>oder Straßen vorliegen                                                                                                                    |
| preferredLanguage<br>(V5_ONLY)  | DATABASE*<br>ENGLISH<br>ALTERNATIVE_1<br>ALTERNATIVE_2<br>ALTERNATIVE_3<br>PRESERVE_INPUT                                                   | Die bevorzugte Ausgabesprache für Länder<br>mit mehreren Sprachen. Bei PRESERVE_IN<br>PUT wird die eingegebene Sprache bevor<br>zugt, ansonsten eine der angegebenen. Für<br>Engine V6 bitte<br>nut<br>preferredLanguages<br>zen.                                                                                                          |
| preferredLanguages<br>(V6_ONLY) | Zeichenkette                                                                                                                                | Komma separierte Liste der bevorzugten<br>Ausgabesprachen (drei Buchstaben nach ISO<br>639)                                                                                                                                                                                                                                                |
| preferredScript                 | DATABASE*<br>POSTAL_ADMIN_PREF<br>POSTAL_ADMIN_ALT<br>LATIN<br>LATIN_1<br>LATIN_ALT<br>ASCII_SIMPLIFIED<br>ASCII_EXTENDED<br>PRESERVE_INPUT | Das bevorzugte Schriftsystem für die Ausga<br>be. Bei PRESERVE_INPUT wird das Schriftsys<br>tem der Eingabe bevorzugt, ansonsten wird<br>mithilfe einer einfachen Transliteration das<br>angegebene verwendet. Es wird allerdings<br>nicht gewährleistet, dass die Ausgabe nur aus<br>Buchstaben des angegebenen Schriftsystem<br>besteht. |
| rcMode<br>(V5_ONLY)             | STRICT*<br>NORMAL<br>OLD<br><modus-name></modus-name>                                                                                       | Bestimmt die Berechnung des Return-Codes.<br>Die verschiedenen Berechnungsmodi sind im<br>Abschnitt<br>(Seite<br>'Ermittlung der Returncodes'<br>29) erklärt.                                                                                                                                                                              |

| Attribut<br>• Pflichtfeld        | Werte<br>* Default                                                                                               | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| correctCityLevel                 | N(=No)<br>Y(=Yes)*                                                                                               | Wenn eingeschaltet und falls Ortsname und<br>Bundesland übereinstimmen, werden alle<br>Ortsnamens-Bestandteile um eine Ebene<br>hochgerückt (nur für China relevant)                                                                                                                                                                                                                                                                                                                                           |
| invalidOnMissingRefData          | N(=No)<br>Y(=Yes)*                                                                                               | Wenn dieses Attribut auf 'Y' gesetzt ist, dann<br>wird eine Adresse auch dann als ungültig<br>angesehen, wenn es keine Referenzdaten<br>für diese Adresse gibt (bspw. es gibt keine<br>Hausnummernbereiche in den Referenz<br>daten). Ist dies nicht gewünscht, so muss<br>man hier 'N' setzen. Dieser Parameter ver<br>ändert nur<br>post.ReturnCode, aber nicht<br>den<br>post.ShortReport.                                                                                                                  |
| keepWrongInfo                    | Zeichenkette von Return<br>Codes getrennt durch<br>Kommata. Weiterhin ist<br>'*' am Anfang der Liste<br>erlaubt. | Der Parameter kann verwendet werden, um<br>Fehlercodes zu definieren, bei denen die Ein<br>gabewerte in die Ausgabefelder kopiert wer<br>den sollen. Das Zeichen '*' repräsentiert alle<br>Fehlercodes. Über '-' können zwei Listen de<br>finiert werden. Die vordere Liste beinhaltet<br>die eingeschlossenen Werte, während die<br>hintere die ausgeschlossenen repräsentiert.<br>Weiterhin steht das '.' für eine beliebige Ziffer.<br>Z.B. '*-26,6.'<br>alle Returncodes bis auf 0, 26,<br>⇔<br>61 und 62. |
| globalPreferredDescriptor        | DATABASE*<br>LONG<br>SHORT<br>PRESERVE_INPUT                                                                     | Gibt vor, wie die Felder<br>post.*PostDesc<br>und<br>post.*PreDesc, (z.B.<br>post.StreetPreDesc) befüllt werden<br>sollen (nur für einige Länder verfügbar, z.B.<br>Straßenkennzeiochnung "Calle" in Spanien).                                                                                                                                                                                                                                                                                                 |
| dualAddressPriority<br>(V5_ONLY) | POSTAL_ADMIN*<br>DELIVERY_SERVICE<br>STREET                                                                      | Beinhaltet die Eingabe Straßen- und Post<br>fachinformation, so bestimmt der Parameter,<br>ob die Adresse als Straßenadresse, Postfach<br>adresse oder länderspezifisch validiert wer<br>den soll. Wird in V6 noch nicht unterstützt.                                                                                                                                                                                                                                                                          |

#### KAPITEL 8. KONFIGURATIONSDATEI **TOLERANT** Software

| Attribut<br>• Pflichtfeld            | Werte<br>* Default                                   | Bedeutung                                                                                                                                                                                                                                                                                            |
|--------------------------------------|------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| matchingExtendedArchive<br>(V5_ONLY) | N(=No)*<br>Y(=Yes)                                   | Wenn für ein Land historische Adressen in<br>den Referenzdaten vorliegen (bspw. Südko<br>rea und Japan), dann kann man bei der Va<br>lidierung hiermit erzwingen, dass die aktu<br>ellen Adressen zurückgeliefert werden. Für<br>V6 benutzen Sie bitte stattdessen das Feld<br>matchingAlternatives. |
| searchRadius<br>(V6_ONLY)            | Natürliche Zahl zwi<br>schen 10-200 (Default:<br>50) | Nur relevant für Modus GEOCODE_TO_AD<br>DRESS: In welchem Umkreis sollen Adressen<br>gesucht werden? Der Geocode dient als Zen<br>trum des Suchradius, um den ein Suchbe<br>reich in Metern gespannt wird                                                                                            |

Beispiel:

```
1 <postProfile id="postProfile -2"
2 matchingAlternatives ="NONE" matchingScope ="DELIVERYPOINT_LEVEL" optimizationLevel ="WIDE"
         mode="BATCH" />
```

Listing 8.10: Beispiel Konfiguration des Post-Profiles

Mit dem optionalen <postEnginemap> Abschnitt wird definiert, mit welchen Ländern die in der Laufzeitkonfiguration konfigurierten Adressvalidierungs-Engines genutzt werden. Fehlt dieser Abschnitt, so wird nur die interne Engine verwendet.

<span id="page-97-0"></span>

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                       |  |
|---------------------------|--------------------|-------------------------------------------------------------------------------------------------|--|
| id •                      | Zeichenkette       | Interne ID, muss gesetzt sein                                                                   |  |
| name                      | Zeichenkette       | Name für die Anzeige in der GUI                                                                 |  |
| postEngineRef •           | Zeichenkette       | Verweis auf eine Adressvalidierungs-Engine.<br>Muss in der Laufzeitkonfiguration vorkom<br>men. |  |

#### Tabelle 8.21: Konfigurationselement <postEnginemap>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                                                                                                                                                                                                                                                                               | Bedeutung                                                                                                                                                                           |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| countryList •             | Zeichenkette von alpha<br>3 Codes getrennt durch<br>Kommata. Die Verwen<br>dung von '*' am Anfang<br>der Liste ist nur dann<br>erlaubt, wenn kein Wild<br>card in der countryList<br>des Profils vorkommt.<br>Die Verwendung von '-'<br>ist nur einmal erlaubt,<br>um Länder auszuschlie<br>ßen. | Wird der Eingabewert des Landes in dieser<br>Liste gefunden, so wird die referenzierte En<br>gine für die Adressvalidierung genutzt. An<br>sonsten wird die interne Engine benutzt. |

Beispiel:

1 <postEngineMap id="id1" postEngineRef ="PENGINE1" countryList ="\* - GBR, ITA" />

Listing 8.11: Beispiel Konfiguration des Post-Engine-Mappings

## <span id="page-98-0"></span>**8.12 FELDZUWEISUNGEN FÜR EINGABEFELDER**

<span id="page-98-1"></span>Jedes Eingabefeld kann einem oder mehreren internen Feldern zugewiesen werden. Dies geschieht über den Abschnitt <inputFieldmap>.

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                   |
|---------------------------|--------------------|-------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Abschnitts                     |
| mode                      | BATCH              | Zusätzliche Information zur Verwendung der<br>Feldzuweisung |

#### Tabelle 8.22: Konfigurationselement <inputFieldmap>

<span id="page-98-2"></span>Jeder <inputFieldmap>-Abschnitt kann ein oder mehrere <inputFieldmapItem>-Elemente enthalten, die angeben, welche Eingabefelder den internen Feldern zugewiesen werden.

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                           |
|---------------------------|--------------------|-----------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Felds                                                                  |
| inputRef •                | Zeichenkette       | Verweis auf den Eingabeabschnitt                                                                    |
| inputFieldRef             | Zeichenkette       | Verweis auf ein Eingabefeld                                                                         |
| internalFieldName         | Zeichenkette       | Verweis auf ein internes Prüffeld (s. Kapitel<br>(Seite<br>99))<br>'Adressfelder und Rückgabewerte' |
| internalFieldRef          | Zeichenkette       | Verweis auf ein Match-Feld²                                                                         |

| Tabelle 8.23: | Konfigurationselement | <inputfieldmapitem></inputfieldmapitem> |
|---------------|-----------------------|-----------------------------------------|
|               |                       |                                         |

#### Beispiel:

```
1 <inputFieldmap id="inputFieldmap -1">
2 <inputFieldmapItem id="inputFieldmapItem -2" inputFieldRef ="csvInputField -2" inputRef="
         csvFileInput -1"
3 internalFieldName ="post.Street" />
4 <inputFieldmapItem id="inputFieldmapItem -3" inputFieldRef ="csvInputField -3" inputRef="
         csvFileInput -1"
5 internalFieldName ="post.Street" />
6 <inputFieldmapItem id="inputFieldmapItem -4" inputFieldRef ="csvInputField -4" inputRef="
         csvFileInput -1"
7 internalFieldName ="post.PostalCode" />
8 <inputFieldmapItem id="inputFieldmapItem -5" inputFieldRef ="csvInputField -5" inputRef="
         csvFileInput -1"
9 internalFieldName ="post.Locality" />
10 <inputFieldmapItem id="inputFieldmapItem -6" inputFieldRef ="csvInputField -6" inputRef="
         csvFileInput -1"
11 internalFieldName ="post.Country" />
12 </ inputFieldmap >
```

Listing 8.12: Beispiel Feldzuweisung der Eingabefelder

<span id="page-99-0"></span>Wie aus dem Beispiel ersichtlich wird, ist es möglich, mehrere Eingabefelder auf dasselbe interne Feld zu kopieren. Dazu muss das interne Feld nur mehrmals in der <inputFieldmap> auftauchen. Die Eingangswerte werden dann durch ein Leerzeichen getrennt hintereinander in das Prüffeld kopiert.

<sup>2</sup> Nur TOLERANT Match

## **8.13 GENERIERTE AUSGABEFELDER**

Jedes Projekt kann kein, ein oder mehrere <generatedFields>-Abschnitte verwenden. Der Abschnitt *['Generierte Ausgabefelder'](#page-31-0)* (Seite [24\)](#page-31-0) beschreibt das Konzept der generierten Felder.

<span id="page-100-1"></span>

| Tabelle 8.24:<br>Konfigurationselement<br><generatedfield></generatedfield> |                                       |                                                                                                                                               |
|-----------------------------------------------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Attribut<br>• Pflichtfeld                                                   | Werte<br>* Default                    | Bedeutung                                                                                                                                     |
| id •                                                                        | Zeichenkette                          | ID zur Referenzierung dieses Felds                                                                                                            |
| name•                                                                       | Zeichenkette                          | Name des Ausgabefeldes                                                                                                                        |
| maxFields                                                                   | Natürliche Zahl (Stan<br>dard: 1)     | Anzahl der Ausgabefelder. Wenn das Tem<br>plate mehrere Ausgabezeilen erzeugt, dann<br>wird die Ausgabe auf maximal maxFields be<br>schränkt. |
| filePath•                                                                   | Dateipfad                             | Pfad zu der Templatedatei                                                                                                                     |
| codepage                                                                    | Zeichenkette (Standard:<br>ISO8859-1) | Zeichenkodierung, die in der Templatedatei<br>verwendet wird                                                                                  |
| type                                                                        | FREEMARKER•<br>GROOVY<br>JAVASCRIPT   | Art des Templates                                                                                                                             |
| inputfields                                                                 | Zeichenkette mit Komma<br>separiert   | Felder, die das Template intern verwendet.                                                                                                    |

## <span id="page-100-0"></span>**8.14 FELDZUWEISUNGEN FÜR AUSGABEFELDER**

Jedem Feld der Ausgabe kann ein Eingabefeld oder ein internes Ergebnisfeld zugeordnet werden, damit seine Datenherkunft klar bestimmt ist. Wird einem Ausgabefeld nichts zugeordnet, so bleibt es leer.

<span id="page-100-2"></span>

| Tabelle 8.25:<br>Konfigurationselement<br><outputfieldmap></outputfieldmap> |                    |                                                         |
|-----------------------------------------------------------------------------|--------------------|---------------------------------------------------------|
| Attribut<br>• Pflichtfeld                                                   | Werte<br>* Default | Bedeutung                                               |
| id •                                                                        | Zeichenkette       | ID zur Referenzierung dieses Abschnitts                 |
| outputRef •                                                                 | Zeichenkette       | Verweis auf das zu verwendende<br><*Output>-<br>Element |

Jeder <outputFieldmap>-Abschnitt kann ein oder mehrere <outputFieldmapItem>-Elemente enthalten.

<span id="page-101-1"></span>

| Tabelle 8.26:<br>Konfigurationselement<br><outputfieldmapitem></outputfieldmapitem> |                    |                                                                                                         |
|-------------------------------------------------------------------------------------|--------------------|---------------------------------------------------------------------------------------------------------|
| Attribut<br>• Pflichtfeld                                                           | Werte<br>* Default | Bedeutung                                                                                               |
| id •                                                                                | Zeichenkette       | ID zur Referenzierung dieses Felds                                                                      |
| inputRef                                                                            | Zeichenkette       | Verweis auf<br>Element<br><*Input>                                                                      |
| inputFieldRef                                                                       | Zeichenkette       | Verweis auf ein Eingangsfeld. Dieses Attri<br>but muss in Zusammenhang mit<br>inputRef<br>gesetzt sein. |
| internalFieldName                                                                   | Zeichenkette       | Verweis auf ein internes Prüffeld (s. Kapitel<br>(Seite<br>99))<br>'Adressfelder und Rückgabewerte'     |
| outputFieldRef •                                                                    | Zeichenkette       | Verweis auf das Ausgabefeld                                                                             |

Es gilt, dass mehrere Zuweisungen an ein Ausgangsfeld wie eine Verkettung interpretiert werden.

Die Attribute inputRef und inputFieldRef müssen zusammen verwendet werden. Dann wird in das Ausgangsfeld der Wert aus der Eingabe geschrieben (Die Synonymersetzung auf den Input wird nicht ausgegeben). Ist stattdessen das Attribut internalFieldName gesetzt, so wird der Wert eines internen Prüffelds ausgegeben. internalFieldName und inputFieldRef können **nicht** zusammen verwendet werden.

Beispiel:

```
1 <outputFieldmap id="outputFieldmap -1" outputRef ="csvFileOutput -1">
2 <outputFieldmapItem id="outputFieldmapItem -1" outputFieldRef ="csvOutputField -1" inputRef="
         csvFileInput -1"
3 inputFieldRef ="csvInputField -1" />
4 <outputFieldmapItem id="outputFieldmapItem -2" outputFieldRef ="csvOutputField -2" inputRef="
         csvFileInput -1"
5 inputFieldRef ="csvInputField -2" />
6 <outputFieldmapItem id="outputFieldmapItem -3" outputFieldRef ="csvOutputField -7"
         internalFieldName ="post.PostalCode" />
7 <outputFieldmapItem id="outputFieldmapItem -4" outputFieldRef ="csvOutputField -8"
         internalFieldName ="post.Locality" />
8 <outputFieldmapItem id="outputFieldmapItem -5" outputFieldRef ="csvOutputField -9"
         internalFieldName ="post.Street" />
9 </ outputFieldmap >
```

<span id="page-101-0"></span>Listing 8.13: Beispiel Feldzuweisung der Ausgabefelder

## **8.15 MEHRERE AUSGABEDATEIEN**

Innerhalb eines Projekts besteht die Möglichkeit, abhängig von den Inhalten interner Felder und von Eingabefeldern mit Hilfe des <outputSwitch>-Elements unterschiedliche <outputFieldmap>-Elemente anzusteuern. Bei Nichtverwendung des <outputSwitch>-Elements wird die erste zur Verfügung stehende <outputFieldmap> verwendet.

Um dem Programm mitzuteilen, welche <outputFieldmap> verwendet werden soll, benutzt man das Element <outputSwitch>. Darin können 1-n <outputTo>-Elemente definiert werden, welche jeweils eine outputFieldmap referenzieren. Die in ihnen definierten <condition>-Elemente legen 1-n Bedingungen fest, bei deren Zutreffen das <outputTo>-Element und damit die outputFieldmap ausgewählt wird. Eine Bedingung trifft dann zu, wenn der Wert des angegebenen Feldes dem Wert im value-Attribut entspricht, wobei Groß-/Kleinschreibung nicht unterschieden wird. Diese Bedingungen sind im Normalfall ODER-verknüpft. Mithilfe einer <condition>, welche den Wert des Attributs default auf true gesetzt hat, kann ein default-<outputTo>-Element erstellt werden, welches alle Datensätze einer <outputFieldmap> zuweist, die keine Bedingung eines vorhergehenden <condition>-Elements erfüllen konnten. Ist für diesen Fall kein default-<outputTo>-Element gesetzt, werden die ausgesteuerten Datensätze in keine Ausgabedatei geleitet und lediglich im log-File vermerkt.

<condition>-Elemente können geschachtelt werden. Hat ein <condition>-Element das Attribut type auf "OR" gesetzt, so sind die darin enthaltenen Bedingungen ODER-verknüpft. Hat ein <condition>- Element das Attribut type auf "AND" gesetzt, so sind die darin enthaltenen Bedingungen UNDverknüpft. Ist das Attribut type nicht gesetzt (oder explizit auf "VALUE") gesetzt, so kann diese Bedingung keine geschachtelten Elemente enthalten und stellt eine reinen Test auf Gleichheit dar. Ist type auf "REGEXP" gesetzt, so wird der Wert mit einem regulären Ausdruck verglichen.

<span id="page-102-0"></span>

| Tabelle 8.27:<br>Konfigurationselement<br><outputswitch></outputswitch> |                    |                               |  |
|-------------------------------------------------------------------------|--------------------|-------------------------------|--|
| Attribut<br>• Pflichtfeld                                               | Werte<br>* Default | Bedeutung                     |  |
| id •                                                                    | Zeichenkette       | Referenz für diesen Abschnitt |  |

Das Element <outputSwitch> dient zur Kapselung der einzelnen Bedingungen, es enthält lediglich eine ID zur Benennung.

<span id="page-102-1"></span>

| Tabelle 8.28:<br>Konfigurationselement<br><outputto></outputto> |                    |                     |  |
|-----------------------------------------------------------------|--------------------|---------------------|--|
| Attribut<br>• Pflichtfeld                                       | Werte<br>* Default | Bedeutung           |  |
| id •                                                            | Zeichenkette       | Referenz des Feldes |  |

#### KAPITEL 8. KONFIGURATIONSDATEI **TOLERANT** Software

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                             |
|---------------------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| outputFieldmapRef •       | Zeichenkette       | Verweis auf die outputFieldmap, welche ver<br>wendet wird, falls die Bedingungen in diesem<br><outputto>-Element zutreffen</outputto> |

<span id="page-103-0"></span>Das Element <outputTo> repräsentiert einen ansteuerbaren Output. Es enthält Unterelemente, die die eigentlichen Bedingungen, die zur Wahl dieses Outputs führen, beinhalten.

| Attribut<br>• Pflichtfeld | Werte<br>* Default | Bedeutung                                                                                                                                                             |
|---------------------------|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id •                      | Zeichenkette       | ID zur Referenzierung dieses Felds                                                                                                                                    |
| inputRef                  | Zeichenkette       | Verweis auf<br>Element<br>csvFileInput                                                                                                                                |
| inputFieldRef             | Zeichenkette       | Verweis auf ein Eingangsfeld. Dieses Attri<br>but muss in Zusammenhang mit<br>inputRef<br>gesetzt sein.                                                               |
| internalFieldName         | Zeichenkette       | Verweis auf ein internes Prüffeld. Es kann<br>entweder das Attributpaar inputRef/input<br>FieldRef verwendet werden, oder internal<br>FieldName, aber niemals beides. |

#### Tabelle 8.29: Konfigurationselement <condition>

| Attribut<br>• Pflichtfeld | Werte<br>* Default                   | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value                     | Zeichenkette                         | Wert (oder regulärer Ausdruck), mit dem das<br>angegebene Eingabefeld oder interne Feld<br>verglichen wird. Wird ein regulärer Ausdruck<br>verwendet, so ist zu beachten, dass manche<br>Zeichen nicht verwendet werden können,<br>weil diese entweder im XML nicht erlaubt<br>sind, oder von POST Batch als spezielle Zei<br>chen interpretiert werden. Die folgenden<br>Zeichen können daher nicht genutzt werden: |
|                           |                                      | —<br>Kleinerzeichen (<)                                                                                                                                                                                                                                                                                                                                                                                              |
|                           |                                      | —<br>Größerzeichen (>)                                                                                                                                                                                                                                                                                                                                                                                               |
|                           |                                      | —<br>Kaufmanns-Und (&)                                                                                                                                                                                                                                                                                                                                                                                               |
|                           |                                      | —<br>Dollarzeichen (\$)                                                                                                                                                                                                                                                                                                                                                                                              |
|                           |                                      | Anführungszeichen (' oder<br>abhängig<br>—<br>"<br>davon, welche Anführungszeichen für<br>das Attribut verwendet werden. )                                                                                                                                                                                                                                                                                           |
| default                   | Zeichenkette                         | Dieses Attribut kennzeichnet das übergeord<br>nete<br><outputto>-Element als Default.</outputto>                                                                                                                                                                                                                                                                                                                     |
| type                      | OR   AND   VALUE*  <br>REGEXP  EMPTY | Gibt an, ob diese Bedingung eine Gruppe von<br>Bedingungen umschließt, oder ein einfacher<br>Vergleich (VALUE), ein regulärer Ausdruck<br>(REGEXP) ist oder der Wert leer sein muss<br>(EMPTY).                                                                                                                                                                                                                      |

In den <condition>-Elementen werden die Bedingungen, die zur Wahl eines <outputTo>-Elements führen, angegeben. Dafür wird ein Abgleich zwischen dem Wert des Attributs value, und den in den Eingangsdaten, bzw. errechneten Daten enthaltenen Werten gemacht. Interne Daten werden mit internalFieldName referenziert, während Werte aus den Eingangsdaten unter der Angabe von inputRef und inputFieldref referenziert werden. Das Attribut default legt fest, dass der dem <condition>-Element zugehörige Output verwendet wird, falls keine der vorher genannten Bedingungen zutrifft.

Das folgende Beispiel zeigt eine <condition> mit drei verschiedenen Möglichkeiten für die Wahl des Outputs. Im ersten wird eine UND-Verknüpfung benutzt, um nur Datensätze auszugeben, in denen die Stadt "Berlin" heißt und die Straße "Hauptstraße". Im zweiten Fall werden alle Datensätze ausgegeben,

in denen die Stadt entweder "Stuttgart" oder "Karlsruhe" heißt. Im dritten Fall werden alle restlichen Daten ausgegeben.

Beispiel:

```
1 <outputSwitch id="switch -1">
2 <outputTo id="outputTo -1" outputFieldmapRef ="outputFieldmap -1">
3 <condition id="condition -group1" type="AND">
4 <condition id="condition -1city" internalFieldName ="post.City" value="Berlin" />
5 <condition id="condition -1street" internalFieldName ="post.Street" value="
                  Hauptstraße" />
6 </ condition >
7 </outputTo >
8 <outputTo id="outputTo -2" outputFieldmapRef ="outputFieldmap -2">
9 <condition id="condition -2" internalFieldName ="post.City" value="Stuttgart" />
10 <condition id="condition -3" internalFieldName ="post.City" value="Karlsruhe" />
11 </outputTo >
12 <outputTo id="outputTo -3" outputFieldmapRef ="outputFieldmap -default">
13 <condition id="condition -4" default="Y" />
14 </outputTo >
15 </ outputSwitch >
```

Listing 8.14: Beispielkonfiguration für mehrere Ausgabedateien
