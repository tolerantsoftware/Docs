
**7**

## **Starten des Programms**

<span id="page-56-0"></span>Im folgenden Abschnitt wird erklärt, wie Sie einen Batchlauf durch einen Kommandozeilenaufruf starten können. Der Aufruf erfolgt durch das Ausführen des abhängig vom Betriebssystem vorgesehenen Skripts.

## <span id="page-56-1"></span>**7.1 PARAMETER**

<span id="page-56-2"></span>Unabhängig von der Plattform auf der POST Batch ausgeführt wird, werden folgende Parameter zum Aufruf benötigt:

| Parameter                 | Beschreibung                          |  |  |  |
|---------------------------|---------------------------------------|--|--|--|
| <filename>.xml</filename> | Die auszuführende Konfigurationsdatei |  |  |  |
| <projectid></projectid>   | Der Name des zu verwendenden Projekts |  |  |  |
| <profileid></profileid>   | Der Name des zu verwendenden Profils  |  |  |  |

<span id="page-56-3"></span>Tabelle 7.1: Parameter Batchlauf

## **7.2 STARTEN DES PROGRAMMS**

#### AUFRUF UNTER WINDOWS

Das Skript zum Ausführen des Batchlaufs lautet unter Windows postBatch.bat und findet sich im Ordner *\bin* des Installationsverzeichnisses. Öffnen Sie einen Windows Kommandoprompt (cmd) und tippen Sie folgende Kommandos:

1 cd <Installationsverzeichnis >

2 bin\ postBatch .bat <configFilename > <projectId > <profileId >

Listing 7.1: Programmstart Windows

AUFRUF UNTER LINUX

Das Skript zum Ausführen des Batchlaufs lautet auf Unixsystemen postBatch.sh und findet sich im Ordner */bin* des Installationsverzeichnisses. Öffnen Sie ein Terminalfenster und tippen Sie folgende Kommandos:

1 cd <Installationsverzeichnis >

2 bin/ postBatch .sh <configFilename > <projectId > <profileId >

Listing 7.2: Programmstart Linux/Solaris

## <span id="page-57-0"></span>**7.3 FEHLERAUSGABE**

Alle gefundenen Fehler und Warnungen werden auf der Konsole ausgegeben. Diese können nichtgefundene Dateien, fehlerhafte Konfigurationen (analog zu postCheck.sh und postCheck.bat) und vieles mehr beinhalten. Ein Beispiel für die Ausgabe:

```
1
2 _____ ___ _ ___ ___ _ _ _ _____ ___ _
3 |_ _/ _ \| | | __| _ \ /_\ | \| |_ _| | _ \___ __| |_
4 | || (_) | |__| _|| / / _ \| .' | | | | _/ _ (_-< _|
5 |_| \___ /| ____|___|_|_\/_/ \_\_|\_| |_| |_| \___/__/\__|
6
7 Version 12.0.56190 , Copyright (c) 2025 TOLERANT Software GmbH & Co KG
8
9
10 Configuration Problems:
11 =======================
12
13 ERROR: config/ testPostBatchConfig1 .xml :13: "/opt/tolerant/input/input.csv" does not exist
14 ERROR: config/ testPostBatchConfig1 .xml :23: "/opt/tolerant/output/output.csv" cannot be written
15
16 ERROR: 140 - configure : Configuration error (RC =6)
17
18 ERROR: Errors in configuration , (RC: 6, Elapsed time: 1 ,856 sec)
```

| Returncode | Beschreibung                                                                                                 |  |  |  |  |
|------------|--------------------------------------------------------------------------------------------------------------|--|--|--|--|
| 0          | Der Batchlauf wurde erfolgreich beendet.                                                                     |  |  |  |  |
| 1          | Die Parameter in der Kommandozeile waren falsch.                                                             |  |  |  |  |
| 2          | Die Konfigurationsdatei für den Batchlauf enthielt Fehler.                                                   |  |  |  |  |
| 3          | Eine Eingabedatei konnte nicht gelesen werden.                                                               |  |  |  |  |
| 4          | Es wurde die definierte Anzahl von Fehlern in der Eingabedatei<br>erreicht. Der Batchlauf wurde abgebrochen. |  |  |  |  |
| 5          | Sonstiger Fehler.                                                                                            |  |  |  |  |
|            |                                                                                                              |  |  |  |  |

<span id="page-58-1"></span>

| Tabelle 7.2: | Batchlauf Returncodes |
|--------------|-----------------------|
|              |                       |

Detailliertere Fehlermeldungen können Sie den Protokollierungsdateien entnehmen, die vom Batchlauf angefertigt und je nach Konfiguration im Ordner logs des Installationsverzeichnisses abgelegt wurden.

<span id="page-58-0"></span>Die Tabelle [7.2:](#page-58-1) *['Batchlauf Returncodes'](#page-58-1)* listet die verschiedenen möglichen Fehlercodes auf.

## **7.4 AUSGABE BEI ERFOLGREICHEM LAUF**

Der Batchlauf läuft vom Aufruf bis zur Verarbeitung des letzten Datensatzes unterbrechungsfrei durch. Nach dem Start wird zunächst eine Übersicht über die eingelesene Konfiguration mit den wichtigsten Einstellungen und Feldern ausgegeben. Der Fortschritt der Verarbeitung wird nun mit einem Balken visualisiert und statistische Daten zum Verlauf der Programmausführung werden ausgegeben. Einen Auszug hiervon können Sie folgendem Block entnehmen.

```
1 ReturnCodes :
2 0 : 9859 (98.0%) OK.
3 10 : 21 (0.0%) Cityname wrong or missing.
4 13 : 24 (0.0%) Postcode wrong.
5 20 : 15 (0.0%) Streetname wrong or missing.
6 28 : 80 (0.0%) Streetname wrong in citylist.
7 30 : 1 (0.0%) POBox wrong or missing.
8
9 ProcessStatus :
10 V4 : 5526 (55.0 %) Verified - Input data correct
11 V3 : 321 (3.0 %) Verified - Input data correct - elements standardised or input contains
        outdated names
12 V2 : 535 (5.0 %) Verified - Input data correct but some elements could not be verified
        because of incomplete reference data
13 C4 : 3051 (30.0 %) Corrected - all (postally relevant) elements have been checked
14 C3 : 426 (4.0 %) Corrected - but some elements could not be checked
15 I4 : 128 (1.0 %) Data could not be corrected completely , but is very likely to be
        deliverable - single match
16 I3 : 3 (0.0 %) Data could not be corrected completely , but is very likely to be
        deliverable - multiple matches
17 I2 : 9 (0.0 %) Data could not be corrected , but there is a slim chance that the address
         is deliverable
```

| Shortreport :                                                                                | OK     | Normed                                           | Changed |       | Determine  Archive | Invalid | Unchecked  Error |      |
|----------------------------------------------------------------------------------------------|--------|--------------------------------------------------|---------|-------|--------------------|---------|------------------|------|
| ++++++++                                                                                     | 6576   | 0                                                | 3424    | 0     | 0                  | 0       | 0                | 0    |
| CountryCode                                                                                  | 65.76% | 0.0%                                             | 34.24%  | 0.0%  | 0.0%               | 0.0%    | 0.0%             | 0.0% |
| ++++++++                                                                                     | 9029   | 738                                              | 0       | 0     | 0                  | 0       | 0                | 0    |
| PostCode                                                                                     | 90.29% | 7.38%                                            | 0.0%    | 0.0%  | 0.0%               | 0.0%    | 0.0%             | 0.0% |
| ++++++++                                                                                     | 0      | 0                                                | 0       | 0     | 0                  | 0       | 10000            | 0    |
| CityName                                                                                     | 0.0%   | 0.0%                                             | 0.0%    | 0.0%  | 0.0%               | 0.0%    | 100.0%           | 0.0% |
| ++++++++                                                                                     | 9879   | 0                                                | 112     | 0     | 0                  | 0       | 9                | 0    |
| SubCityName                                                                                  | 98.79% | 0.0%                                             | 1.12%   | 0.0%  | 0.0%               | 0.0%    | 0.09%            | 0.0% |
| ++++++++                                                                                     | 8910   | 0                                                | 961     | 2     | 0                  | 0       | 51               | 0    |
| StreetName                                                                                   | 89.1%  | 0.0%                                             | 9.61%   | 0.02% | 0.0%               | 0.0%    | 0.51%            | 0.0% |
| ++++++++                                                                                     | 8      | 0                                                | 0       | 0     | 0                  | 0       | 9991             | 0    |
| HouseNumber                                                                                  | 0.08%  | 0.0%                                             | 0.0%    | 0.0%  | 0.0%               | 0.0%    | 99.91%           | 0.0% |
| ++++++++                                                                                     | 0      | 0                                                | 0       | 0     | 0                  | 0       | 10000            | 0    |
| POBox                                                                                        | 0.0%   | 0.0%                                             | 0.0%    | 0.0%  | 0.0%               | 0.0%    | 100.0%           | 0.0% |
| ++++++++                                                                                     | 0.0%   | 0                                                | 0       | 0     | 0                  | 0       | 0                | 0    |
| Organisation  0                                                                              |        | 0.0%                                             | 0.0%    | 0.0%  | 0.0%               | 0.0%    | 0.0%             | 0.0% |
| ++++++++<br>Lines read:<br>Lines written:<br>Errors in input:<br>Processed in:<br>Lines/sec: |        | 10.001<br>10.000<br>0<br>51 ,484 sec<br>194 ,255 |         |       |                    |         |                  |      |
| Finished batch at 15.11.2010 11:12:03<br>Batch took 00:00:54                                 |        |                                                  |         |       |                    |         |                  |      |

I1 : 1 (0.0 %) Data could not be corrected and is pretty unlikely to be delivered .

<span id="page-59-0"></span>Es werden sowohl die auftretenden ReturnCodes, ProzessStatus als auch der sogenannte ShortReport ausgegeben. Letzterer weist einzelnen Addressbestandteilen unterschiedliche Wertungen zu. Diese werden über alle geprüften Daten aggregiert und mit ihrer prozentualen Verteilung ausgegeben. Die folgende Tabelle zeigt die Bedeutungen der Wertungen.

| Wertung   | Bedeutung                                                      |  |  |
|-----------|----------------------------------------------------------------|--|--|
| OK        | Inhalt der Eingabe geprüft und in Ordunung                     |  |  |
| Normed    | Inhalt der Eingabe geprüft, Schreibweise<br>normiert           |  |  |
| Changed   | Inhalt der Eingabe geprüft, Schreibweise<br>korrigiert         |  |  |
| Determine | Inhalt der Eingabe konnnte nicht ermittelt werden              |  |  |
| Archive   | Inhalt der Eingabe als Archiveintrag identifiziert und ersetzt |  |  |
| Invalid   | Inhalt der Eingabe geprüft,<br>fehlerhaft oder nicht ermittel  |  |  |
|           | bar                                                            |  |  |
| Unchecked | Inhalt nicht analysiert                                        |  |  |
| Error     | Mehrdeutiger / fehlerhaft formatierter Inhalt                  |  |  |

<span id="page-60-0"></span>

| Tabelle 7.3: | Wertungen ShortReport |
|--------------|-----------------------|
|              |                       |

## **7.5 TESTMODUS**

Um eine Batchkonfiguration nur mit wenigen Zeilen der Eingabe zu testen, ist es möglich, den Testmodus in der Konfiguration zu aktivieren. Alternativ kann man diesen aber auch direkt von der Kommandozeile aus aktivieren. Durch die Option --test und die Angabe einer Zeilenzahl n wird der Batchlauf nur für die ersten n Zeilen durchgeführt. Die Einstellungen in der Konfiguration (testmode und nrTestRecords) werden dabei ignoriert.

Beispielsweise ist der Aufruf unter Windows wie folgt:

1 cd <Installationsverzeichnis >

2 bin\ postBatch .bat <configFilename > <projectId > <profileId > --test <lines >

Listing 7.3: Programmstart Windows mit Testoption

oder unter Linux/Solaris:

1 cd <Installationsverzeichnis >

2 bin/ postBatch .sh <configFilename > <projectId > <profileId > --test <lines >

Listing 7.4: Programmstart Linux/Solaris mit Testoption
