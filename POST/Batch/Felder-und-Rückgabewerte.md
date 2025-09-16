**10**

## <span id="page-144-0"></span>**Felder und Rückgabewerte für statistische Bezirke (StaB) und Gemeindeschlüssel (AGS)**

Mit POST Batch ist es möglich, den sogenannten statistischen Bezirk (StaB) und den amtlichen Gemeindeschlüssel (AGS) für deutsche Adressen zu ermitteln. Dafür werden fünf weitere interne Felder bereitgestellt. Für die Ermittlung der statistischen Bezirke und der amtliche Gemeindeschlüssel werden zusätzliche Referenzdateien benötigt.

<span id="page-144-1"></span>

| Feldname    | Richtung<br>E=Eingang, A=Ausgang | Bedeutung                                                                     |
|-------------|----------------------------------|-------------------------------------------------------------------------------|
| stab.StaB   | E / A                            | Der statistische Bezirk                                                       |
| stab.AGS    | –/ A                             | Der amtliche Gemeindeschlüssel, der aus der StaB<br>Datenbank ermittelt wurde |
| stab.Status | –/ A                             | Der Status der StaB Ermittlung/Suche                                          |
| stab.Source | –/ A                             | Die Datenquelle für die StaB Ermittlung                                       |
| ags.AGS     | E / A                            | Der amtliche Gemeindeschlüssel                                                |
| ags.Status  | –/ A                             | Der Status der AGS Ermittlung/Suche                                           |

| Tabelle 10.1:<br>Liste der Felder für statische Bezirke und amtliche Gemeindeschlüssel |
|----------------------------------------------------------------------------------------|
|----------------------------------------------------------------------------------------|

Wird die StaB-Ermittlung durchgeführt, so gibt es noch weitere Werte in post.ShortReport an

Position 7 (DeliveryService):

- D: StaB gefunden
- E: StaB nicht gefunden
- F: keine StaB Referenzdaten
- 0: Wert war ok und wurde unverändert übernommen
- 2: Wert wurde korrigiert

Bei der Ermittlung des AGS wird post.ShortReport nicht verändert.

## **STAB.SOURCE**

Das Feld stab.Source gibt die Quelle der Daten für den gefundenen StaB aus.

| Wert | Bedeutung                                           |
|------|-----------------------------------------------------|
| C    | Ortsgenaue Daten von TDS                            |
| O    | Ortsgenaue Daten aus anderen Quellen                |
| G    | Ortsgenaue Daten vom statistischen Bundesamt (G100) |
| S    | Straßengenaue Daten von TDS                         |

#### <span id="page-145-0"></span>Tabelle 10.2: Werte des Feldes stab.Source

## **STAB.STATUS**

Das Feld stab.Status gibt einen Status zur StaB Ermittlung.

| Wert | Bedeutung                                                                                                          |
|------|--------------------------------------------------------------------------------------------------------------------|
| 0    | Ermittlung über Postleitzahl, Ort, Straße, Hausnummer                                                              |
| 1    | Ermittlung über Postleitzahl, Straße, Hausnummer (ohne Ort)                                                        |
| 2    | Ermittlung über Postleitzahl, Ort, Straße (ohne Hausnummer)                                                        |
| 3    | Ermittlung über Postleitzahl, Straße (ohne Ort, Hausnummer)                                                        |
| 4    | Ermittlung über Postleitzahl, Ort, Straße ging schief                                                              |
| 5    | Ermittlung über Postleitzahl, Straße ging schief                                                                   |
| 6    | Ermittlung über Postleitzahl, Ort, Straße, Hausnummer mehrdeutig                                                   |
| 7    | Ermittlung über Postleitzahl, Straße, Hausnummer (ohne Ort) mehrdeutig                                             |
| 8    | Ermittlung über Postleitzahl, Ort, Straße (ohne Hausnummer) mehrdeutig                                             |
| 9    | Ermittlung über Postleitzahl, Straße (ohne Ort, Hausnummer) mehrdeutig                                             |
| 50   | Ermittlung über Postleitzahl, Ort (ohne Straße) (Quelle: C)                                                        |
| 51   | Ermittlung über Postleitzahl, Ort (ohne Straße), aber vorherige Prüfung über Straße<br>ging schief (Quelle: C)     |
| 52   | Ermittlung über Postleitzahl, Ort nicht eindeutig                                                                  |
| 53   | Ermittlung über Postleitzahl, Ort nicht gefunden                                                                   |
| 60   | Ermittlung über Postleitzahl, Ort (ohne Straße) (Quelle: G)                                                        |
| 61   | Ermittlung über Postleitzahl, Ort (ohne Straße), aber vorherige Prüfung über Straße<br>ging schief (Quelle: G)     |
| 70   | Ermittlung über Postleitzahl, Ort (ohne Straße) (Quelle: O)                                                        |
| 71   | Ermittlung über Postleitzahl, Ort (ohne Straße), aber vorherige Prüfung über Straße<br>ging schief (Quelle: O)     |
| 100  | Ermittlung über Postleitzahl, Ort (Postfachadresse) (Quelle: C)                                                    |
| 101  | Ermittlung über Postleitzahl, Ort (Postfachadresse), aber vorherige Prüfung über Straße<br>ging schief (Quelle: C) |
| 102  | Ermittlung über Postleitzahl, Ort (Postfachadresse) nicht eindeutig                                                |
| 103  | Ermittlung über Postleitzahl, Ort (Postfachadresse) nicht gefunden                                                 |
| 110  | Ermittlung über Postleitzahl, Ort (Postfachadresse) (Quelle: O)                                                    |
| 111  | Ermittlung über Postleitzahl, Ort (Postfachadresse), aber vorherige Prüfung über Straße<br>ging schief (Quelle: O) |
| 200  | Datenbankfehler (keine Datenbank gefunden)                                                                         |
| 201  | Datenbankfehler (Fehler bei der Abfrage)                                                                           |

#### <span id="page-146-0"></span>Tabelle 10.3: Werte des Feldes stab.Status

## **AGS.STATUS**

Das Feld ags.Status gibt einen Status zur AGS Ermittlung.

| Wert | Bedeutung                                                                      |
|------|--------------------------------------------------------------------------------|
| 0    | AGS über Ortsname gefunden und Postleitzahl stimmt überein                     |
| 1    | AGS nur über Postleitzahl gefunden                                             |
| 10   | AGS über Ort gefunden, aber keine Ziffern der Postleitzahl stimmten überein    |
| 11   | AGS über Ort gefunden, aber nur eine Ziffer der Postleitzahl stimmten überein  |
| 12   | AGS über Ort gefunden, aber nur zwei Ziffern der Postleitzahl stimmten überein |
| 13   | AGS über Ort gefunden, aber nur drei Ziffern der Postleitzahl stimmten überein |
| 14   | AGS über Ort gefunden, aber nur vier Ziffern der Postleitzahl stimmten überein |
| 15   | AGS über Ort gefunden, aber nur vier Ziffern der Postleitzahl stimmten überein |
| 20   | AGS nicht gefunden                                                             |
| 30   | AGS nicht gefunden                                                             |
| 40   | Postleitzahl nicht eindeutig                                                   |
| 41   | AGS für Postleitzahl nicht gefunden                                            |
| 200  | Datenbankfehler (keine Datenbank gefunden)                                     |
| 201  | Datenbankfehler (Fehler bei der Abfrage)                                       |
|      |                                                                                |

#### <span id="page-147-0"></span>Tabelle 10.4: Werte des Feldes ags.Status
