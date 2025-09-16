**9**

## <span id="page-106-0"></span>**Adressfelder und Rückgabewerte**

## <span id="page-106-1"></span>**9.1 ADRESSFELDER**

Alle möglichen internen Felder der Adressvalidierung sind in Tabelle [9.1:](#page-106-2) *['Liste der Adressfelder \(Logisch](#page-106-2) [gruppiert\)'](#page-106-2)* festgehalten und können in der Konfiguration verwendet werden. Manche Felder können nur als Eingabe und manche nur als Ausgabewerte benutzt werden. Stellen die Felder Statuswerte dar, so sind die möglichen Wertebereiche in den folgenden Abschnitten erklärt. Manche Felder sind nur für bestimmte Modi sinnvoll.

Nicht alle Felder sind für alle Länder gesetzt. Es ist nicht zwingend notwendig, alle Felder direkt zu besetzen. Werden Straße und Hausnummer in post.Street gesetzt, so werden die Daten bei der Adressüberprüfung getrennt in post.Street und post.Number. Die Adressüberprüfung gelingt besser, wenn eines der Länderfelder (post.Country, post.CountryEN, ...) gesetzt ist.

<span id="page-106-2"></span>Um manche Felder verwenden zu können, benötigen Sie zusätzliche Referenzdaten. In der folgenden Tabelle werden diese Felder in der Spalte 'Daten' gekennzeichnet, dabei bedeutet 'S', dass die Standardreferenzdaten (BATCH\_INTERACTIVE oder FAST\_COMPLETION) benötigt werden.

| Feldname                                                                   | <b>Richtung</b><br>E=Eingang,<br>A=Ausgang | <b>Daten</b><br>s. Text | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------------------------------|--------------------------------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.RecipientLine1<br>                                                    | E / A                                      | S                       | Empfängeradresszeile                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| post.RecipientLine4                                                        |                                            |                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| post.DeliveryAddressLine1<br><br>post.DeliveryAddressLine4                 | E / A                                      | S                       | Adresszeilen für gemischte Eingabe<br>mit Einzelfeldern. Werden die Felder<br>post.DeliveryAddressLineN als Eingabe<br>verwendet, so muss post.Country ebenfalls<br>gesetzt sein. post.DeliveryAddressLineN<br>Felder können in Verbindung mit anderen<br>Feldern wie post.Street, etc. genutzt wer-<br>den. Weiterhin können über die Attribute<br>inputFormat* und outputFormat* im Profil<br>Veränderungen vorgenommen werden. Ach-<br>tung: Funktioniert nicht in Verbindung mit<br>FastCompletion-Modus. |
| post.CountrySpecificLocalityLine1<br><br>post.CountrySpecificLocalityLine4 | E/A                                        | S                       | Adresszeile für länderspezifische Angaben (V5<br>ONLY, für V6 bitte das kombinierte Feld<br>post.PostalLocalityLine nutzen).                                                                                                                                                                                                                                                                                                                                                                                  |
| post.PostalLocalityLine                                                    | -/ A                                       | S                       | Enthält Informationen wie Ort, Postleitzahl, Pro-<br>vinz und Länderangaben (V6_ONLY).                                                                                                                                                                                                                                                                                                                                                                                                                        |
| post.AddressLine1<br><br>post.AddressLine8                                 | E / A                                      | S                       | Eine Zeile einer Adresse im Format für posta-<br>lische Adressierung. Der jeweilige Wert hängt<br>von der Adressart und dem Land ab. Werden die<br>Felder post.AddressLine <i>N</i> als Eingabe verwen-<br>det, so muss post.Country ebenfalls gesetzt sein<br>post.AddressLineN Felder können nicht in Ver-<br>bindung mit anderen Feldern wie post.Street<br>etc. genutzt werden. Weiterhin können über die<br>Attribute inputFormat* und outputFormat* im<br>Profil Veränderungen vorgenommen werden.      |
| post.AddressComplete                                                       | E / A                                      | S                       | Eine komplette Adresse in einem Feld (nur in Zu-<br>samenhang mit Mode "FASTCOMPLETION". Zur<br>Aktivierung mit anderen Verarbeitungsmodus<br>kontaktieren Sie bitte unseren Support unter<br>Kapitel 'Support' (Seite 3)).                                                                                                                                                                                                                                                                                   |

Tabelle 9.1: Liste der Adressfelder (Logisch gruppiert)

#### **TOLERANT** Software KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE

| Feldname                    | Richtung | Daten | Bedeutung                                                                                                         |
|-----------------------------|----------|-------|-------------------------------------------------------------------------------------------------------------------|
| post.Key                    | E / A    | -     | Ein Schlüssel der zu überprüfenden Daten. Die<br>ser Schlüssel wird einfach durchgereicht.                        |
| post.Country                | E / A    | S     | Ländername (Sprachabhängige Bezeichnung)                                                                          |
| post.CountryEN              | E / A    | S     | Ländername (englische Bezeichnung, V5_ONLY)                                                                       |
| post.CountryISO2            | E / A    | S     | Ländercode ISO3166 alpha-2 (V5_ONLY)                                                                              |
| post.CountryISO3            | E / A    | S     | Ländercode ISO3166 alpha-3 (V5_ONLY)                                                                              |
| post.CountryISONumber       | –/ A     | S     | Ländercode als ISO3166-Nummer (bspw. 276 für<br>Deutschland, V5_ONLY)                                             |
| post.CountryAbbrev          | E / A    | S     | Abgekürzter Ländername (V5_ONLY), für V6<br>durch<br>abgelöst<br>post.CountryCode                                 |
| post.Territory              | E / A    | S     | Territory eines Landes (V5_ONLY)                                                                                  |
| post.Locality               | E / A    | S     | Stadt                                                                                                             |
| post.City                   | E / A    | S     | Identisch zu<br>post.Locality                                                                                     |
| post.CityComplete           | –/ A     | S     | Vollständiger Name der Stadt (V5_ONLY, in V6<br>bereits in<br>und<br>ent<br>post.Locality<br>post.City<br>halten) |
| post.SubCity                | E / A    | S     | Ortsteil                                                                                                          |
| post.CityDistrict           | E / A    | S     | Zusätzliche Informationen zu einer Unterebe<br>ne des Ortsteils (sofern von den Referenzdaten<br>unterstützt)     |
| post.CityAbbrev             | –/ A     | S     | Abgekürzter Ortsname                                                                                              |
| post.SortingCode            | –/ A     | S     | Zusatzcode Ortsname                                                                                               |
| post.PostalCode             | E / A    | S     | Postleitzahl (formatiert)                                                                                         |
| post.PostalCodeUnformatted  | E / A    | S     | Postleitzahl (unformatiert)                                                                                       |
| post.PostalCodeBase         | –/ A     | S     | Postleitzahl (Basis Informationen)                                                                                |
| post.PostalCodeAddOn        | –/ A     | S     | Postleitzahl (zusätzliche Informationen)                                                                          |
| post.PostalCode2            | E / A    | S     | Postleitzahl2 (formatiert)                                                                                        |
| post.PostalCode2Unformatted | E / A    | S     | Postleitzahl2 (unformatiert)                                                                                      |
| post.PostalCode2Base        | –/ A     | S     | Postleitzahl2 (Basis Informationen)                                                                               |
| post.PostalCode2AddOn       | –/ A     | S     | Postleitzahl2 (zusätzliche Informationen)                                                                         |
| post.Province1              | E / A    | S     | Provinz / Bundesland                                                                                              |
| post.Province1Ext           | –/ A     | S     | Provinz / Bundesland (zusätzliche Informatio<br>nen)                                                              |
| post.Province1Abbrev        | –/ A     | S     | Provinz / Bundesland (Abkürzung)                                                                                  |
| post.Province2              | E / A    | S     | Teilprovinz                                                                                                       |
| post.Province2Ext           | –/ A     | S     | Teilprovinz (zusätzliche Informationen)                                                                           |
| post.post.Province2Abbrev   | –/ A     | S     | Teilprovinz (Abkürzung)                                                                                           |
| post.Province3              | –/ A     | S     | Traditionelle Bezirkinformation                                                                                   |
| post.Province3Ext           | –/ A     | S     | Traditionelle Bezirkinformation (zusätzliche In<br>formationen)                                                   |

#### KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE **TOLERANT** Software

| Feldname                | Richtung | Daten | Bedeutung                                                                                                                                                                                                                                                                       |
|-------------------------|----------|-------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.Province3Abbrev    | –/ A     | S     | Traditionelle Bezirkinformation (Abkürzung)                                                                                                                                                                                                                                     |
| post.Street             | E / A    | S     | Komplette Straßenbezeichnung                                                                                                                                                                                                                                                    |
| post.StreetAdd          | –/ A     | S     | Straße (zusätzliche Informationen)                                                                                                                                                                                                                                              |
| post.StreetPreDesc      | –/ A     | S     | Straße (Beschreibung vorne)                                                                                                                                                                                                                                                     |
| post.StreetPostDesc     | –/ A     | S     | Straße (Beschreibung hinten)                                                                                                                                                                                                                                                    |
| post.StreetPreDir       | –/ A     | S     | Straße (Richtungsangabe vorne)                                                                                                                                                                                                                                                  |
| post.StreetPostDir      | –/ A     | S     | Straße (Richtungsangabe hinten)                                                                                                                                                                                                                                                 |
| post.StreetAbbrev       | –/ A     | S     | Straßenbezeichnung (Abkürzung)                                                                                                                                                                                                                                                  |
| post.StreetName         | –/ A     | S     | Straßenname                                                                                                                                                                                                                                                                     |
| post.SubStreet          | E / A    | S     | Komplette Straßenbezeichnung (2. Ebene)                                                                                                                                                                                                                                         |
| post.SubStreetAdd       | –/ A     | S     | Straße (zusätzliche Informationen) (2. Ebene)                                                                                                                                                                                                                                   |
| post.SubStreetPreDesc   | –/ A     | S     | Straße (Beschreibung vorne) (2. Ebene)                                                                                                                                                                                                                                          |
| post.SubStreetPostDesc  | –/ A     | S     | Straße (Beschreibung hinten) (2. Ebene)                                                                                                                                                                                                                                         |
| post.SubStreetPreDir    | –/ A     | S     | Straße (Richtungsangabe vorne) (2. Ebene)                                                                                                                                                                                                                                       |
| post.SubStreetPostDir   | –/ A     | S     | Straße (Richtungsangabe hinten) (2. Ebene)                                                                                                                                                                                                                                      |
| post.SubStreetName      | –/ A     | S     | Straßenname (2. Ebene)                                                                                                                                                                                                                                                          |
| post.Number             | E / A    | S     | Hausnummer                                                                                                                                                                                                                                                                      |
| post.NumberNr           | –/ A     | S     | Hausnummer (nur Zahlen)                                                                                                                                                                                                                                                         |
| post.NumberDescriptor¹  | –/ A     | S     | Hausnummer (Beschreibung) (nur für einige<br>Länder verfügbar)                                                                                                                                                                                                                  |
| post.NumberPreDesc      | –/ A     | S     | Hausnummer (Beschreibung vorne, V6_ONLY)<br>(z.B. "Nummner" im Fall von "Nummer 22")                                                                                                                                                                                            |
| post.NumberPostDesc     | –/ A     | S     | Hausnummer (Beschreibung hinten, V6_ONLY)<br>(nur für einige Länder verfügbar)                                                                                                                                                                                                  |
| post.Number2            | E / A    | S     | Hausnummer (2. Ebene)                                                                                                                                                                                                                                                           |
| post.Number2Nr          | –/ A     | S     | Hausnummer (nur Zahlen) (2. Ebene)                                                                                                                                                                                                                                              |
| post.Number2Descriptor¹ | –/ A     | S     | Hausnummer (Beschreibung) (2. Ebene) (nur<br>für einige Länder verfügbar)                                                                                                                                                                                                       |
| post.Number2PreDesc     | –/ A     | S     | Hausnummer (Beschreibung vorne, V6_ONLY)<br>(2. Ebene) (z.B. "Nummner" im Fall von "Num<br>mer 22")                                                                                                                                                                             |
| post.Number2PostDesc    | –/ A     | S     | Hausnummer (Beschreibung hinten, V6_ONLY)<br>(2. Ebene) (nur für einige Länder verfügbar)                                                                                                                                                                                       |
| post.NumberFrom         | –/ A     | S     | Hausnummer (untere Grenze). Dieser Wert<br>ist<br>nur<br>gesetzt,<br>wenn<br>die<br>Hausnummer<br>nicht<br>eindeutig<br>bestimmt<br>werden<br>konnte.<br>Das Feld<br>ist dann leer, wenn<br>post.Number<br>und<br>un<br>post.NumberFrom<br>post.NumberTo<br>terschiedlich sind. |

| Feldname                    | Richtung | Daten | Bedeutung                                                                                                                                                                                                                                                                                  |
|-----------------------------|----------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.NumberTo               | –/ A     | S     | Hausnummer<br>(obere<br>Grenze).<br>Dieser<br>Wert<br>ist<br>nur<br>gesetzt,<br>wenn<br>die<br>Hausnummer<br>nicht<br>eindeutig<br>bestimmt<br>werden<br>konnte.<br>Das Feld<br>ist dann leer, wenn<br>post.Number<br>und<br>un<br>post.NumberFrom<br>post.NumberTo<br>terschiedlich sind. |
| post.NumberExt              | –/ A     | S     | Hausnummernzusatz, nur gesetzt, wenn<br>spli<br>gesetzt und<br>tHouseNoExtensions<br>storeHou<br>auf "SEPARATE_FIELD"<br>gesetzt<br>seNoExtension<br>ist, die Adresse in Deutschland ist und ein Haus<br>nummernzusatz vorliegt.                                                           |
| post.Building               | E / A    | S     | Gebäude                                                                                                                                                                                                                                                                                    |
| post.BuildingName           | –/ A     | S     | Gebäudename                                                                                                                                                                                                                                                                                |
| post.BuildingNr             | –/ A     | S     | Gebäudenummer                                                                                                                                                                                                                                                                              |
| post.BuildingDescriptor¹    | –/ A     | S     | Gebäudebeschreibung (nur für einige Länder<br>verfügbar)                                                                                                                                                                                                                                   |
| post.BuildingPreDesc        | –/ A     | S     | Gebäudebeschreibung (vorne, V6_ONLY) (nur<br>für einige Länder verfügbar)                                                                                                                                                                                                                  |
| post.BuildingPostDesc       | –/ A     | S     | Gebäudebeschreibung (hinten, V6_ONLY) (nur<br>für einige Länder verfügbar)                                                                                                                                                                                                                 |
| post.Building2              | E / A    | S     | Gebäude (2. Ebene)                                                                                                                                                                                                                                                                         |
| post.Building2Name          | –/ A     | S     | Gebäudename (2. Ebene)                                                                                                                                                                                                                                                                     |
| post.Building2Nr            | –/ A     | S     | Gebäudenummer (2. Ebene)                                                                                                                                                                                                                                                                   |
| post.Building2Descriptor¹   | –/ A     | S     | Gebäudebeschreibung (2. Ebene) (nur für einige<br>Länder verfügbar)                                                                                                                                                                                                                        |
| post.Building2PreDesc       | –/ A     | S     | Gebäudebeschreibung (vorne, V6_ONLY) (2. Ebe<br>ne)<br>(nur<br>für<br>einige<br>Länder<br>verfügbar,<br>z.B.<br>"APARTMENT" für "APARTMENT 4B")                                                                                                                                            |
| post.Building2PostDesc      | –/ A     | S     | Gebäudebeschreibung (hinten, V6_ONLY) (2.<br>Ebene) (nur für einige Länder verfügbar)                                                                                                                                                                                                      |
| post.SubBuilding            | E / A    | S     | Untergebäude                                                                                                                                                                                                                                                                               |
| post.SubBuildingName        | –/ A     | S     | Untergebäudename                                                                                                                                                                                                                                                                           |
| post.SubBuildingNr          | –/ A     | S     | Untergebäudenummer                                                                                                                                                                                                                                                                         |
| post.SubBuildingDescriptor¹ | –/ A     | S     | Untergebäudebeschreibung (nur für einige Län<br>der verfügbar)                                                                                                                                                                                                                             |
| post.SubBuildingPreDesc     | –/ A     | S     | Untergebäudebeschreibung (vorne, V6_ONLY)<br>(nur für einige Länder verfügbar)                                                                                                                                                                                                             |
| post.SubBuildingPostDesc    | –/ A     | S     | Untergebäudebeschreibung (hinten, V6_ONLY)<br>(nur für einige Länder verfügbar)                                                                                                                                                                                                            |
| post.SubBuilding2           | E / A    | S     | Untergebäude (2. Ebene)                                                                                                                                                                                                                                                                    |
| post.SubBuilding2Name       | –/ A     | S     | Untergebäudename (2. Ebene)                                                                                                                                                                                                                                                                |

| Feldname                         | Richtung | Daten | Bedeutung                                                                                                         |
|----------------------------------|----------|-------|-------------------------------------------------------------------------------------------------------------------|
| post.SubBuilding2Nr              | –/ A     | S     | Untergebäudenummer (2. Ebene)                                                                                     |
| post.SubBuilding2Descriptor¹     | –/ A     | S     | Untergebäudebeschreibung (2. Ebene) (nur für<br>einige Länder verfügbar)                                          |
| post.SubBuilding2PreDesc         | –/ A     | S     | Untergebäudebeschreibung (vorne, V6_ONLY)<br>(2. Ebene) (nur für einige Länder verfügbar)                         |
| post.SubBuilding2PostDesc        | –/ A     | S     | Untergebäudebeschreibung (hinten, V6_ONLY)<br>(2. Ebene) (nur für einige Länder verfügbar)                        |
| post.SubBuilding3                | E / A    | S     | Untergebäude (3. Ebene)                                                                                           |
| post.SubBuilding3Name            | –/ A     | S     | Untergebäudename (3. Ebene)                                                                                       |
| post.SubBuilding3Nr              | –/ A     | S     | Untergebäudenummer (3. Ebene)                                                                                     |
| post.SubBuilding3Descriptor¹     | –/ A     | S     | Untergebäudebeschreibung (3. Ebene) (nur für<br>einige Länder verfügbar)                                          |
| post.SubBuilding3PreDesc         | –/ A     | S     | Untergebäudebeschreibung (vorne, V6_ONLY)<br>(3. Ebene) (nur für einige Länder verfügbar)                         |
| post.SubBuilding3PostDesc        | –/ A     | S     | Untergebäudebeschreibung (hinten, V6_ONLY)<br>(3. Ebene) (nur für einige Länder verfügbar)                        |
| post.Entrance                    | E / A    | S     | identisch zu post.SubBuilding                                                                                     |
| post.Floor                       | E / A    | S     | identisch zu post.SubBuilding2                                                                                    |
| post.DeliveryService             | E / A    | S     | Zusätzliche Lieferinformationen, bspw. Postfach                                                                   |
| post.DeliveryServiceNr           | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(nur Nummer)                                                   |
| post.DeliveryServiceAdd          | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(zusätzliche Informationen)                                    |
| post.DeliveryServiceDescriptor¹  | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(Beschreibung) (nur für einige Länder verfüg<br>bar)           |
| post.DeliveryServicePreDesc      | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(Beschreibung vorne) (nur für einige Länder ver<br>fügbar)     |
| post.DeliveryServicePostDesc     | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(Beschreibung hinten) (nur für einige Länder<br>verfügbar)     |
| post.DeliveryService2            | E / A    | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(2. Ebene)                                                     |
| post.DeliveryService2Nr          | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(nur Nummer) (2. Ebene)                                        |
| post.DeliveryService2Add         | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(zusätzliche Informationen) (2. Ebene)                         |
| post.DeliveryService2Descriptor¹ | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(Beschreibung) (2. Ebene) (nur für einige Länder<br>verfügbar) |

| Feldname                      | Richtung | Daten | Bedeutung                                                                                                                                                                                                                                   |
|-------------------------------|----------|-------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.DeliveryService2PreDesc  | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(Beschreibung vorne) (2. Ebene) (nur für einige<br>Länder verfügbar)                                                                                                                     |
| post.DeliveryService2PostDesc | –/ A     | S     | Zusätzliche Lieferinformationen, bspw. Postfach<br>(Beschreibung hinten) (2. Ebene) (nur für einige<br>Länder verfügbar)                                                                                                                    |
| post.DeliveryServiceFrom      | –/ A     | S     | Untere Grenze<br>der Postfachnummer,<br>wenn<br>das Postfach nicht eindeutig bestimmt wer<br>den konnte. Das Feld post.DeliveryService ist<br>dann leer, wenn post.DeliveryServiceFrom und<br>post.DeliveryServiceTo verschieden sind.      |
| post.DeliveryServiceTo        | –/ A     | S     | Obere<br>Grenze<br>der<br>Postfachnummer,<br>wenn<br>das Postfach nicht eindeutig bestimmt wer<br>den konnte. Das Feld post.DeliveryService ist<br>dann leer, wenn post.DeliveryServiceFrom und<br>post.DeliveryServiceTo verschieden sind. |
| post.Organization             | E / A    | S     | Organisation                                                                                                                                                                                                                                |
| post.OrganizationName         | –/ A     | S     | Organisation (nur Name)                                                                                                                                                                                                                     |
| post.OrganizationDepartment   | –/ A     | S     | Organisation (nur Abteilung)                                                                                                                                                                                                                |
| post.OrganizationDescriptor¹  | –/ A     | S     | Organisation (Beschreibung) (nur für einige Län<br>der verfügbar)                                                                                                                                                                           |
| post.OrganizationPreDesc      | –/ A     | S     | Organisation (Beschreibung vorne) (nur für ei<br>nige Länder verfügbar)                                                                                                                                                                     |
| post.OrganizationPreDesc      | –/ A     | S     | Organisation (Beschreibung hinten) (nur für ei<br>nige Länder verfügbar)                                                                                                                                                                    |
| post.Organization2            | E / A    | S     | Organisation (2. Ebene)                                                                                                                                                                                                                     |
| post.OrganizationPreDesc      | –/ A     | S     | Organisation (Beschreibung vorne) (2. Ebene)<br>(nur für einige Länder verfügbar)                                                                                                                                                           |
| post.OrganizationPreDesc      | –/ A     | S     | Organisation (Beschreibung hinten) (2. Ebene)<br>(nur für einige Länder verfügbar)                                                                                                                                                          |
| post.Organization2Name        | –/ A     | S     | Organisation (nur Name) (2. Ebene)                                                                                                                                                                                                          |
| post.Organization2Department  | –/ A     | S     | Organisation (nur Abteilung) (2. Ebene)                                                                                                                                                                                                     |
| post.Organization2Descriptor¹ | –/ A     | S     | Organisation (Beschreibung) (2. Ebene) (nur für<br>einige Länder verfügbar)                                                                                                                                                                 |
| post.Contact                  | E / A    | S     | Zusätzliche Kontaktinformation (Name, Vorna<br>me)                                                                                                                                                                                          |
| post.ContactFirstName         | –/ A     | S     | Zusätzliche Kontaktinformation (Vorname)                                                                                                                                                                                                    |
| post.ContactMiddleName        | –/ A     | S     | Zusätzliche Kontaktinformation (mittler Name)                                                                                                                                                                                               |
| post.ContactLastName          | –/ A     | S     | Zusätzliche Kontaktinformation (Nachmane)                                                                                                                                                                                                   |
| post.ContactName              | –/ A     | S     | Zusätzliche Kontaktinformation (kompletter Na<br>me)                                                                                                                                                                                        |

#### KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE **TOLERANT** Software

| Feldname                 | Richtung | Daten | Bedeutung                                                                                                                                              |
|--------------------------|----------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.ContactTitle        | –/ A     | S     | Zusätzliche Kontaktinformation (Titel)                                                                                                                 |
| post.ContactFunction     | –/ A     | S     | Zusätzliche Kontaktinformation (Funktion)                                                                                                              |
| post.ContactSalutation   | –/ A     | S     | Zusätzliche Kontaktinformation (Anrede)                                                                                                                |
| post.ContactGender       | –/ A     | S     | Zusätzliche Kontaktinformation (Geschlecht)                                                                                                            |
| post.Contact2            | E / A    | S     | Zusätzliche Kontaktinformation (Name, Vorna<br>me) (2. Ebene)                                                                                          |
| post.Contact2FirstName   | –/ A     | S     | Zusätzliche Kontaktinformation (Vorname) (2.<br>Ebene)                                                                                                 |
| post.Contact2MiddleName  | –/ A     | S     | Zusätzliche Kontaktinformation (mittler Name)<br>(2. Ebene)                                                                                            |
| post.Contact2LastName    | –/ A     | S     | Zusätzliche Kontaktinformation (Nachmane) (2.<br>Ebene)                                                                                                |
| post.Contact2Name        | –/ A     | S     | Zusätzliche Kontaktinformation (kompletter Na<br>me) (2. Ebene)                                                                                        |
| post.Contact2Title       | –/ A     | S     | Zusätzliche Kontaktinformation (Titel) (2. Ebene)                                                                                                      |
| post.Contact2Function    | –/ A     | S     | Zusätzliche Kontaktinformation (Funktion) (2.<br>Ebene)                                                                                                |
| post.Contact2Salutation  | –/ A     | S     | Zusätzliche<br>Kontaktinformation<br>(Anrede)<br>(2.<br>Ebene)                                                                                         |
| post.Contact2Gender      | –/ A     | S     | Zusätzliche Kontaktinformation (Geschlecht) (2.<br>Ebene)                                                                                              |
| post.Residue             | –/ A     | S     | Adresselemente, die nicht zugeordnet werden<br>konnten                                                                                                 |
| post.Residue2            | –/ A     | S     | Adresselemente, die nicht zugeordnet werden<br>konnten (2. Teil)                                                                                       |
| post.Residue3            | –/ A     | S     | Adresselemente, die nicht zugeordnet werden<br>konnten (3. Teil)                                                                                       |
| post.GeoCodeLatitude     | –/ A     | GC    | Geographische Breite                                                                                                                                   |
| post.GeoCodeLongitude    | –/ A     | GC    | Geographische Länge                                                                                                                                    |
| post.GeoCodeComplete     | –/ A     | GC    | Geokoordinaten, komplett                                                                                                                               |
| post.GeoCodeUnit         | –/ A     | GC    | Die Genauigkeit/Einheit der Koordinaten/des<br>Koordinatensystems (V5_ONLY)                                                                            |
| post.GeoCodingStatus     | –/ A     | GC    | Dieser Status bewertet die Genauigkeit der er<br>mittelten Geokoordinaten. Eine genaue Auflis<br>tung der Werte finden Sie im folgenden Ab<br>schnitt. |
| post.GeoCodingStatusDesc | –/ A     | GC    | Beschreibung des Statuswerts, der die Genauig<br>keit der ermittelten Geokoordinaten bewertet.                                                         |
| post.ResultPercentage    | –/ A     | S     | Qualitätswert, der sich aus Eingabedaten und<br>Rückgabedaten ermittelt.                                                                               |

| Feldname                                     | Richtung     | Daten  | Bedeutung                                                                                                                                                                                                                               |
|----------------------------------------------|--------------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.ShortReport                             | –/ A         | S      | Der ShortReport stellt eine Kurzform des Pro<br>zessStatus dar und wird in einer leichter zu ver<br>arbeitenden Form dargestellt. Der ShortReport<br>ist ein achtstelliger String.                                                      |
| post.ProcessStatus<br>post.ProcessStatusDesc | –/ A<br>–/ A | S<br>S | Der Prozesstatus beschreibt die Qualität des<br>Rückgabeergebnisses.<br>Erklärtext des Processstatus.                                                                                                                                   |
|                                              |              |        |                                                                                                                                                                                                                                         |
| post.ReturnCode                              | –/ A         | S      | Der ReturnCode beschreibt in einer Kurzform<br>die Qualität des Ergebnisses.                                                                                                                                                            |
| post.ReturnCodeDesc                          | –/ A         | S      | Erklärtext des Returncodes.                                                                                                                                                                                                             |
| post.ElementInputStatus                      | –/ A         | S      | Status der Eingabe. Hier erhalten Sie Informatio<br>nen darüber, welches Eingabeelement bei der<br>Eingabe korrekt war und welches korrigiert wer<br>den musste. Eine genaue Auflistung der Werte<br>finden Sie im folgenden Abschnitt. |
| post.ElementResultStatus                     | –/ A         | S      | Status der Rückgabe. Hier erhalten Sie Informa<br>tionen darüber, welches Rückgabeelement kor<br>rigiert werden musste. Eine genaue Auflistung<br>der Werte finden Sie im folgenden Abschnitt.                                          |
| post.ElementRelevance                        | –/ A         | S      | Liefert Informationen über die Relevanz einer<br>Information für die Adresse. Eine genaue Auf<br>listung finden Sie im folgenden Abschnitt.                                                                                             |
| post.ExtElementStatus                        | –/ A         | S      | Liefert zusätzliche Informationen zur Rückgabe.<br>Eine genaue Auflistung finden Sie im folgenden<br>Abschnitt.                                                                                                                         |
| post.MailabilityScore                        | –/ A         | S      | Bewertet die Zustellbarkeit von Post für die er<br>mittelte Adresse (V5_ONLY, wird ersetzt durch<br>post.ResultQuality<br>in V6)                                                                                                        |
| post.MailabilityScoreDesc                    | –/ A         | S      | Erklärtext<br>für<br>das<br>post.MailabilityScore<br>Feld<br>(V5_ONLY,<br>wird<br>ersetzt<br>durch<br>in V6)<br>post.ResultQualityDesc                                                                                                  |
| post.ResultQuality                           | –/ A         | S      | Bewertet die Zustellbarkeit von Post für die er<br>mittelte Adresse (V6_ONLY)                                                                                                                                                           |
| post.ResultQualityDesc                       | –/ A         | S      | Erklärtext für das<br>post.ResultQuality-Feld<br>(V6_ONLY)                                                                                                                                                                              |
| post.AddressType                             | –/ A         | S      | Der Typ der validierten/korrigierten Adresse. Ei<br>ne genaue Auflistung der Werte finden Sie im<br>folgenden Abschnitt.                                                                                                                |

| Feldname                   | Richtung     | Daten    | Bedeutung                                                                                                                                                                                                                                                  |
|----------------------------|--------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.AddressTypeDesc       | –/ A         | S        | Erklärtext für das post.AddressType-Feld.                                                                                                                                                                                                                  |
| post.AddressResolutionCode | –/ A         | S        | Beschreibt, warum die Adresse nicht verifiziert<br>werden konnte. Relevant für Adressen, die mit<br>= Ix validiert sind. Eine<br>post.ProcessStatus<br>genaue Auflistung der Werte finden Sie im fol<br>genden Abschnitt (V5_ONLY, abgekündigt für<br>V6). |
| post.MatchingScope         | –/ A         | S        | Gibt den MatchingScope aus, mit dem die Adres<br>se gefunden wurde. Dies ist hilfreich, wenn als<br>AUTO eingestellt wurde.<br>matchingScope                                                                                                               |
| post.PostalCodeExt         | –/ A         | E1       | Zusatz zur Postleitzahl (zurzeit nur für die<br>Schweiz).                                                                                                                                                                                                  |
| post.DEULocalityID         | –/ A         | E1       | Eindeutiger interner Schlüssel für Städte in<br>Deutschland                                                                                                                                                                                                |
| post.DEUStreetID           | –/ A         | E1       | Eindeutiger interner Schlüssel für Straßen in<br>Deutschland                                                                                                                                                                                               |
| post.StreetCode            | –/ A         | E1       | Der Straßencodeteil (die Stellen 6, 7 und 8) des<br>Frachtleitcodes (nur für Deutschland).                                                                                                                                                                 |
| post.DPS                   | –/ A         | E1       | Zusatz zur Postleitzahl (nur für Großbritannien).                                                                                                                                                                                                          |
| post.UDPRN<br>post.UPRN    | –/ A<br>–/ A | E1<br>E1 | Eindeutige 8-stellige Referenznummer, die für<br>jeden Lieferpunkt definiert ist (nur für Großbri<br>tannien).<br>Eindeutige Referenznummer, die für ein Grund                                                                                             |
| post.AddressKey            | –/ A         | E1       | stück definiert ist (nur für Großbritannien). Die<br>se Nummer kann bis 12 Ziffern speichern.<br>Adressschlüssel (nur für Großbritannien).                                                                                                                 |
| post.PAC                   | –/ A         | E1       | Zusatz zur Postleitzahl (nur für Österreich).                                                                                                                                                                                                              |
| post.PACID                 | –/ A         | E1       | Eindeutige Referenznummer einer Identadresse<br>(nur für Österreich).                                                                                                                                                                                      |
| post.GminaCode             | –/ A         | E1       | Gmina-Code (nur für Polen).                                                                                                                                                                                                                                |
| post.LocalityTerytID       | –/ A         | E1       | Kennzeichnung zu den Ortsdaten in Polen.                                                                                                                                                                                                                   |
| post.StreetTerytID         | –/ A         | E1       | Kennzeichnung zu den Straßendaten in Polen.                                                                                                                                                                                                                |
| post.INSEE                 | –/ A         | E1       | Der offizielle geographische Code (nur für Frank<br>reich).                                                                                                                                                                                                |
| post.INSEE9                | –/ A         | E1       | Der offizielle geographische Code (nur für Frank<br>reich).                                                                                                                                                                                                |
| post.PAK                   | –/ A         | E1       | Zusatz zur Postleitzahl (nur für Serbien).                                                                                                                                                                                                                 |

| Feldname                   | Richtung | Daten | Bedeutung                                                                                                                                                         |
|----------------------------|----------|-------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.ChoumeiAzaCode        | –/ A     | E1    | Der Choumei-Code (nur für Japan).                                                                                                                                 |
| post.NewChoumeiAzaCode     | –/ A     | E1    | Der neue Choumei-Code (nur für Japan).                                                                                                                            |
| post.CurrentChoumeiAzaCode | –/ A     | E1    | Der aktuelle Choumei-Code (nur für Japan).                                                                                                                        |
| post.GaikuCode             | –/ A     | E1    | Der Gaiku-Code (nur für Japan).                                                                                                                                   |
| post.IBGE                  | –/ A     | E1    | Der offizielle geographische Code (nur für Bra<br>silien).                                                                                                        |
| post.NAD                   | –/ A     | E1    | Kennzeichnung zu den Straßendaten in Südafri<br>ka.                                                                                                               |
| post.CountyFIPSCode        | –/ A     | E1    | 3-stellige Kennzeichnung eines Bezirkes in den<br>USA.                                                                                                            |
| post.StateFIPSCode         | –/ A     | E1    | 2-stellige Kennzeichnung eines Staates in den<br>USA.                                                                                                             |
| post.PlaceFIPSCode         | –/ A     | E1    | 5-stellige Kennzeichnung einer Stadt in den<br>USA.                                                                                                               |
| post.MSA                   | –/ A     | E1    | Der 4-stellige MSA-Code (nur für die USA).                                                                                                                        |
| post.CBSA                  | –/ A     | E1    | Der 5-stellige CBSA-Code (nur für die USA).                                                                                                                       |
| post.FinanceNumber         | –/ A     | E1    | Die 6-stellige Finanznummer (nur für die USA).                                                                                                                    |
| post.RecordType            | –/ A     | E1    | Der Typ des Lieferpunktes (nur für die USA).                                                                                                                      |
| post.CMSA                  | –/ A     | E1    | 4-stellige Kennzeichnung (nur für die USA).                                                                                                                       |
| post.CensusTractNO         | –/ A     | E1    | 6-stellige Kennzeichnung eines Blockes in den<br>USA.                                                                                                             |
| post.CensusBlockNO         | –/ A     | E1    | 4-stellige Kennzeichnung eines Gebietes in den<br>USA.                                                                                                            |
| post.CensusBlockGroup      | –/ A     | E1    | Bezeichnet eine Gruppe von Census-Blöcken,<br>die die gleiche erste Ziffer teilen (nur für die<br>USA).                                                           |
| post.PMSA                  | –/ A     | E1    | 4-stellige Kennzeichnung (nur für die USA).                                                                                                                       |
| post.MCD                   | –/ A     | E1    | Bezeichnet eine kleine Bürgerteilung (nur für<br>die USA).                                                                                                        |
| post.TimeZoneCode          | –/ A     | E1    | Zeitzone (nur für die USA).                                                                                                                                       |
| post.TimeZoneName          | –/ A     | E1    | Name der Zeitzone in den USA.                                                                                                                                     |
| post.NIS                   | –/ A     | E1    | 9-stelliger Addressschlüssel. Die führenden 5<br>Ziffern stehen für den NIS code, während die<br>letzten 4 für die Nachbarschaft-ID (nur für Bel<br>gien) gelten. |
| post.FIAS                  | –/ A     | E1    | Kennzeichnung zu den Adressen in Russland.                                                                                                                        |
| post.AMASErrorCode         | –/ A     | Cx    | Interner Fehlercode (nur für Australien mit<br>Modus).<br>CERTIFIED                                                                                               |

| Feldname                         | Richtung | Daten | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|----------------------------------|----------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.AMASRecordType              | –/ A     | Cx    | Der Typ des Rekords (nur für Australien mit<br>CER<br>Modus).<br>TIFIED                                                                                                                                                                                                                                                                                                                                                                            |
| post.AMASDeliveryPointId         | –/ A     | Cx    | Kennzeichnung der Zustellungsadresse (nur für<br>Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                            |
| post.AMASLotNumber               | –/ A     | Cx    | Die Lot Nummer (nur für Australien mit<br>CER<br>Modus).<br>TIFIED                                                                                                                                                                                                                                                                                                                                                                                 |
| post.AMASPostalDeliveryNBR       | –/ A     | Cx    | Referenznummer der postalischen Auslieferung<br>(nur für Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                    |
| post.AMASPostalDeliveryNBRPrefix | –/ A     | Cx    | Das Prefix des postalischen Auslieferungscodes<br>(nur für Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                  |
| post.AMASPostalDeliveryNBRSuffix | –/ A     | Cx    | Das Suffix des postalischen Auslieferungscodes<br>(nur für Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                  |
| post.AMASHouseNumberFL           | –/ A     | Cx    | Hausnummer (1. Ebene) (nur für Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                                              |
| post.AMASHouseNumberFLSuffix     | –/ A     | Cx    | Das Suffix der Hausnummer (1. Ebene) (nur für<br>Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                            |
| post.AMASHouseNumberSL           | –/ A     | Cx    | Hausnummer (2. Ebene) (nur für Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                                              |
| post.AMASHouseNumberSLSuffix     | –/ A     | Cx    | Das Suffix der Hausnummer (2. Ebene) (nur für<br>Australien mit<br>Modus).<br>CERTIFIED                                                                                                                                                                                                                                                                                                                                                            |
| post.AMASStatusDesc              | –/ A     | Cx    | Beschreibung der ermittelten zertifizierten Da<br>ten.                                                                                                                                                                                                                                                                                                                                                                                             |
| post.SNACategory                 | –/ A     | Cx    | Der Validierungsstatus der geprüften Adresse<br>(nur für Frankreich mit<br>Modus). Das<br>CERTIFIED<br>Ausgabefeld kann folgende Werte haben: ORI<br>(Adresse is gültig und die Eingabe und Ausgabe<br>stimmen überein), RES (Adresse ist gültig, aber<br>die Ausgabe wurde angepasst oder korrigiert),<br>AVE (Adresse ist falsch und wurde abgelehnt.<br>Die Adresse muss per Hand korrigiert werden)<br>und NOK (Die Adresse ist nicht gültig). |
| post.KORAddressID                | E / A    | E1    | Kennzeichnung zu den Adressen in Südkorea.                                                                                                                                                                                                                                                                                                                                                                                                         |
| post.INEProvinceCode             | –/ A     | E1    | Kennzeichnung der Provinzen in Spanien .                                                                                                                                                                                                                                                                                                                                                                                                           |
| post.INEMunicipalityCode         | –/ A     | E1    | Kennzeichnung zu den Gemeinden in Spanien.                                                                                                                                                                                                                                                                                                                                                                                                         |
| post.INEStreetCode               | –/ A     | E1    | Kennzeichnung zu den Straßen in Spanien.                                                                                                                                                                                                                                                                                                                                                                                                           |
| post.ISTATCode                   | –/ A     | E1    | Geographische und demographische Zusatzin<br>formationen einer validen Adresse in Italien.                                                                                                                                                                                                                                                                                                                                                         |
| post.Sequence                    | –/ A     | S     | Eine laufende Nummer, die die Sortierreihen<br>folge des Ergebnisses entspricht.                                                                                                                                                                                                                                                                                                                                                                   |

| Feldname                    | Richtung | Daten | Bedeutung                                                                                                          |
|-----------------------------|----------|-------|--------------------------------------------------------------------------------------------------------------------|
| post.ResultCount            | –/ A     | S     | Die gesamte Anzahl der zurückgelieferten Er<br>gebnisse einer Abfrage.                                             |
| post.TotalCount             | –/ A     | S     | Die gesamte Anzahl der gefundenen Ergebnisse<br>einer Abfrage.                                                     |
| post.RUIANDeliveryID        | –/ A     | E1    | Eindeutige Referenznummer einer Adresse auf<br>Hausnummernebene (nur für die Tschechische<br>Republik).            |
| post.RUIANBuildingID        | –/ A     | E1    | Eindeutige Referenznummer einer Adresse auf<br>Gebäudeebene (nur für die Tschechische Repu<br>blik).               |
| post.RUIANEntranceID        | –/ A     | E1    | Eindeutige Referenznummer einer Adresse auf<br>Untergebäudeebene (nur für die Tschechische<br>Republik).           |
| post.Engine                 | –/ A     | S     | Name der Varlidierungsengine.                                                                                      |
| post.EngineErrorCode        | –/ A     | S     | Der bei der Adressvalidierung generierte Feh<br>lercode.                                                           |
| post.CAMEOStatus            | –/ A     | CA    | Bewertung der ermittelten CAMEO Daten.                                                                             |
| post.CAMEOCategory          | –/ A     | CA    | Bewertung<br>des<br>Wohlstands<br>der<br>validierten<br>Adresse.                                                   |
| post.CAMEOGroup             | –/ A     | CA    | Bewertung der Nachbarschaft der validierten<br>Adresse.                                                            |
| post.CAMEOInternational     | –/ A     | CA    | Bewertung auf internationaler Ebene des Wohl<br>stands der validierten Adresse.                                    |
| post.CAMEOMVID              | –/ A     | CA    | CAMEO-Schlüssel.                                                                                                   |
| post.CAMEOCategoryDesc      | –/ A     | CA    | Beschreibung des Wohlstands der validierten<br>Adresse.                                                            |
| post.CAMEOGroupDesc         | –/ A     | CA    | Beschreibung der Nachbarschaft der validierten<br>Adresse.                                                         |
| post.CAMEOInternationalDesc | –/ A     | CA    | Beschreibung auf internationaler Ebene des<br>Wohlstands der validierten Adresse.                                  |
| post.CAMEOStatusDesc        | –/ A     | CA    | Beschreibung der ermittelten CAMEO Daten.                                                                          |
| post.GNAFId                 | –/ A     | E1    | Ein 14-stelliger Code, der eine Adresse in dem<br>GNA-File identifiziert. Nur für Australien.                      |
| post.SA1Main                | –/ A     | E1    | Ein 11-stelliger Code, der den statistischen Le<br>vel 1 Bereich der Adresse identifiziert. Nur für<br>Australien. |
| post.SA1Digit               | –/ A     | E1    | Ein 7-stelliger Code, der den statistischen Le<br>vel 1 Bereich der Adresse identifiziert. Nur für<br>Australien.  |

| Feldname                     | Richtung | Daten | Bedeutung                                                                                                                                       |
|------------------------------|----------|-------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| post.SA2Main                 | –/ A     | E1    | Ein 9-stelliger Code, der den statistischen Le<br>vel 2 Bereich der Adresse identifiziert. Nur für<br>Australien.                               |
| post.SA2Digits               | –/ A     | E1    | Ein 5-stelliger Code, der den statistischen Le<br>vel 2 Bereich der Adresse identifiziert. Nur für<br>Australien.                               |
| post.SA2Name                 | –/ A     | E1    | Der Name eines Level 2 statistischen Bereiches.<br>Nur für Australien.                                                                          |
| post.SA3Code                 | –/ A     | E1    | Ein 5-stelliger Code, der den statistischen Le<br>vel 3 Bereich der Adresse identifiziert. Nur für<br>Australien.                               |
| post.SA3Name                 | –/ A     | E1    | Der Name eines Level 3 statistischen Bereiches.<br>Nur für Australien.                                                                          |
| post.SA4Code                 | –/ A     | E1    | Ein 3-stelliger Code, der den statistischen Le<br>vel 4 Bereich der Adresse identifiziert. Nur für<br>Australien.                               |
| post.SA4Name                 | –/ A     | E1    | Der Name eines Level 4 statistischen Bereiches.<br>Nur für Australien.                                                                          |
| post.GCCCode                 | –/ A     | E1    | Ein 5-stelliger alphanumerischer Code, der den<br>statistischen Bereich einer großen Hauptstadt<br>kennzeichnet. Nur für Australien.            |
| post.GCCName                 | –/ A     | E1    | Der Name eines statistischen Bereiches einer<br>großen Haupstadt. Nur für Australien.                                                           |
| post.STECode                 | –/ A     | E1    | Ein 1-stelliger Code, der einen Staat oder ein<br>Gebiet kennzeichnet. Nur für Australien.                                                      |
| post.STEName                 | –/ A     | E1    | Der Name des Staates bzw. des Gebietes. Nur<br>für Australien mit E1 Daten.                                                                     |
| post.CCD06                   | –/ A     | E1    | Ein 7-stelliger Code, der die Volkszählungsdaten<br>vom Jahr 2006 identifiziert. Nur für Australien.                                            |
| post.MESHBlock11             | –/ A     | E1    | Ein 11-stelliger Code, der vom Amt für Statistik<br>für die Volkszählungsdaten vom Jahr 2011 der<br>Adresse zugewiesen ist. Nur für Australien. |
| post.MESHBlock16             | –/ A     | E1    | Ein 11-stelliger Code, der vom Amt für Statistik<br>für die Volkszählungsdaten vom Jahr 2016 der<br>Adresse zugewiesen ist. Nur für Australien. |
| post.SupplementaryStatus     | –/ A     | E1    | Bewertung der ermittelten ergänzenden Daten.                                                                                                    |
| post.SupplementaryStatusDesc | –/ A     | E1    | Beschreibung der ermittelten ergänzenden Da<br>ten.                                                                                             |
| post.CASSErrorCode           | –/ A     | Cx    | Interner Fehlercode. Nur für USA mit<br>CERTI<br>Modus.<br>FIED                                                                                 |

| Feldname                         | Richtung | Daten | Bedeutung                                                                                                                                                   |
|----------------------------------|----------|-------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.CASSBarcode                 | –/ A     | Cx    | Ein 11-stelliger Barcode, der den Lieferpunkt<br>kennzeichnet. Nur für USA mit<br>Mo<br>CERTIFIED<br>dus.                                                   |
| post.CASSDeliveryPoint           | –/ A     | Cx    | Ein 2-stelliger Code, der den Lieferpunkt identi<br>fiziert. Nur für USA mit<br>Modus.<br>CERTIFIED                                                         |
| post.CASSPoBoxOnly               | –/ A     | Cx    | Gibt an, ob die PLZ nur für Postfächer relevant<br>ist. Nur für USA mit<br>Modus.<br>CERTIFIED                                                              |
| post.CASSRecordType              | –/ A     | Cx    | Ein Großbuchstabe, der den Typ des Records<br>angibt. Nur für USA mit<br>Modus.<br>CERTIFIED                                                                |
| post.CASSCarrierRoute            | –/ A     | Cx    | Der 4-stellige Code des Bezirks. Nur für USA mit<br>Modus.<br>CERTIFIED                                                                                     |
| post.CASSCogressionalDistrict    | –/ A     | Cx    | Die Nummer des Kongressbezirks. Nur für USA<br>mit<br>Modus.<br>CERTIFIED                                                                                   |
| post.CASSDeliveryPointCheckDigit | –/ A     | Cx    | Prüfziffer des CASS Barcodes. Nur für USA mit<br>Modus.<br>CERTIFIED                                                                                        |
| post.CASSHighRiseDefault         | –/ A     | Cx    | Ein Merker (Y N), der angibt, ob die Eingabe<br>einer PLZ+4 Standardhochhausadresse zugeord<br>net werden kann. Nur für USA mit<br>CERTIFIED<br>Modus.      |
| post.CASSHighRiseExact           | –/ A     | Cx    | Ein Merker (Y N), der angibt, ob die Eingabe<br>einer PLZ+4 Exakthochhausadresse zugeordnet<br>werden kann. Nur für USA mit<br>Mo<br>CERTIFIED<br>dus.      |
| post.CASSRuralRouteDefault       | –/ A     | Cx    | Ein Merker (Y N), der angibt, ob die Eingabe ei<br>ner PLZ+4 ländlichen Standerdadresse zugeord<br>net werden kann. Nur für USA mit<br>CERTIFIED<br>Modus.  |
| post.CASSRuralRouteExact         | –/ A     | Cx    | Ein Merker (Y N), der angibt, ob die Eingabe<br>einer PLZ+4 ländlichen Exaktadresse zugeord<br>net werden kann. Nur für USA mit<br>CERTIFIED<br>Modus.      |
| post.CASSLACS                    | –/ A     | Cx    | 'L', wenn die Adresse von LACS konvertiert wur<br>de. Nur für USA mit<br>Modus.<br>CERTIFIED                                                                |
| post.CASSDPVConfirmation         | –/ A     | Cx    | Gibt an, inwieweit die Adresse von DPV bestätigt<br>werden kann (Y, D, S, N oder leer). Nur für USA<br>mit<br>Modus.<br>CERTIFIED                           |
| post.CASSDPVConfirmation3553     | –/ A     | Cx    | Gibt an, ob der Datensatz in die ZIP+4/DPV<br>bestätigte Zählung des 3553-Formulars aufge<br>nommen werden soll. Nur für USA mit<br>CERTI<br>Modus.<br>FIED |

#### KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE **TOLERANT** Software

| Feldname                      | Richtung | Daten | Bedeutung                                                                          |
|-------------------------------|----------|-------|------------------------------------------------------------------------------------|
| post.CASSDPV_CMRA             | –/ A     | Cx    | Gibt an, ob die Adresse als CMRA erkannt wurde.                                    |
|                               |          |       | Nur für USA mit<br>Modus.<br>CERTIFIED                                             |
| post.CASSDPV_PBSA             | –/ A     | Cx    | Gibt an, ob die Adresse als PBSA erkannt wurde.                                    |
|                               |          |       | Nur für USA mit<br>Modus.<br>CERTIFIED                                             |
| post.CASSDPV_DNA              | –/ A     | Cx    | Gibt an, ob die Adresse als DNA erkannt wurde.                                     |
|                               |          |       | Nur für USA mit<br>Modus.<br>CERTIFIED                                             |
| post.CASSDPV_NSL              | –/ A     | Cx    | Gibt an, ob die Adresse als NSL erkannt wurde.                                     |
|                               |          |       | Nur für USA mit<br>Modus.<br>CERTIFIED                                             |
| post.CASSDPVFalsePositive     | –/ A     | Cx    | Gibt an, ob die Adresse als DPV "false positive"<br>erkannt wurde. Nur für USA mit |
|                               |          |       | CERTIFIED                                                                          |
|                               |          |       | Modus.                                                                             |
| post.CASSFootnote1            | –/ A     | Cx    | Die erste Anmerkung der Lieferpunktsvalidie<br>rung (DPV) . Nur für USA mit<br>Mo  |
|                               |          |       | CERTIFIED                                                                          |
| post.CASSFootnote2            | –/ A     | Cx    | dus.<br>Die zweite Anmerkung der Lieferpunktsvalidie                               |
|                               |          |       | rung (DPV). Nur für USA mit<br>Modus.                                              |
| post.CASSFootnote3            | –/ A     | Cx    | CERTIFIED<br>Die dritte Anmerkung der Lieferpunktsvalidie                          |
|                               |          |       |                                                                                    |
|                               |          |       | rung (DPV). Nur für USA mit<br>Modus.<br>CERTIFIED                                 |
| post.CASSFootnoteComplete     | –/ A     | Cx    | Die komplette Anmerkung der Lieferpunktsva<br>lidierung (DPV). Nur für USA mit     |
|                               |          |       | CERTIFIED<br>Modus.                                                                |
|                               | –/ A     | Cx    | Der LACSLINK Rückgabewert. Nur für USA mit                                         |
| post.CASSLACSLinkRC           |          |       | Modus.                                                                             |
| post.CASSSUITELinkRC          | –/ A     | Cx    | CERTIFIED<br>Der SUITELINK Rückgabewert. Nur für USA mit                           |
|                               |          |       | Modus.                                                                             |
|                               |          |       | CERTIFIED                                                                          |
| post.CASSEWSRC                | –/ A     | Cx    | Gibt an, ob die Adresse in der EWS Data gefun                                      |
|                               |          |       | den werden kann. Nur für USA mit<br>CERTIFIED                                      |
|                               |          |       | Modus.                                                                             |
| post.CASSZipMoveRC            | –/ A     | Cx    | Der ZIPMOVE Rückgabewert. Nur für USA mit                                          |
|                               |          |       | Modus.<br>CERTIFIED                                                                |
| post.CASSDSF2NoStatsIndicator | –/ A     | Cx    | Gibt<br>das<br>Ergebnis<br>der<br>Abfrage<br>in<br>der<br>DPV                      |
|                               |          |       | NOSTATS-Tabelle an. Nur für USA mit<br>CERTI                                       |
|                               |          |       | Modus.<br>FIED                                                                     |
| post.CASSDSF2VacantIndicator  | –/ A     | Cx    | Gibt<br>das<br>Ergebnis<br>der<br>Abfrage<br>in<br>der<br>DPV                      |
|                               |          |       | VACANT-Tabelle an. Nur für USA mit<br>CERTI                                        |
|                               |          |       | Modus.<br>FIED                                                                     |
| post.CASSDSF2NoStatsReason    | –/ A     | Cx    | Ein Code zwischen 01-05, der das Ergebnis des                                      |
|                               |          |       | "post.CASSDSF2NoStatsIndicator" erklärt. Nur                                       |
|                               |          |       | für USA mit<br>Modus.<br>CERTIFIED                                                 |
| post.CASSDefaultFlag          | –/ A     | Cx    | Gibt an, ob die Adresse eine Defaultadresse ent                                    |
|                               |          |       | spricht. Nur für USA mit<br>Modus.<br>CERTIFIED                                    |

| Feldname                   | Richtung | Daten | Bedeutung                                                                                                                                                  |
|----------------------------|----------|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| post.CASSLACSLinkIndicator | –/ A     | Cx    | Ein Merker des LACSLINK. Nur für USA mit<br>CER<br>Modus.<br>TIFIED                                                                                        |
| post.CASSRDI               | –/ A     | Cx    | Gibt an, ob der Lieferpunkt eine Privatadresse<br>ist. Nur für USA mit<br>Modus.<br>CERTIFIED                                                              |
| post.CASSELOTFlag          | –/ A     | Cx    | Gibt an, ob die Reihenfolge im ELOFT-Feld ab<br>steigend oder aufsteigend ist. Nur für USA mit<br>Modus.<br>CERTIFIED                                      |
| post.CASSELOTSequence      | –/ A     | Cx    | Eine Zahl, die die Reihenfolge der Erweiterungs<br>codes einer Adresse angibt . Nur für USA mit<br>Modus.<br>CERTIFIED                                     |
| post.CASSNDD               | –/ A     | Cx    | Ein '8-bit' Code, der jeden Tag der Woche ent<br>weder als Lieferungs- oder Nichtlieferungstag<br>kennzeichnet. Nur für USA mit<br>Mo<br>CERTIFIED<br>dus. |
| post.CASSStatus            | –/ A     | Cx    | Der Status der CASS Abfrage. Nur für USA mit<br>Modus.<br>CERTIFIED                                                                                        |
| post.CASSStatusDesc        | –/ A     | Cx    | Beschreibung zu dem "post.CASSStatus" Wert.<br>Nur für USA mit<br>Modus.<br>CERTIFIED                                                                      |
| post.Language              | E / –    | –     | Bestimmt die Sprache der Ausgabe. Nur mit<br>Engine.<br>HERE                                                                                               |
| post.InputGeocodeLatitude  | E / –    | GTA   | Geographische Breite in WGS84 Format, z.B.<br>"48.7779691". (V6_ONLY)                                                                                      |
| post.InputGeocodeLongitude | E / –    | GTA   | Geographische Länge in WGS84 Format, z.B.<br>"9.1705242". (V6_ONLY)                                                                                        |
| post.InputGeocodeComplete  | E / –    | GTA   | Geokoordinaten, komplett, kommasepariert in<br>WGS84 Format, z.B. "48.7779691,9.1705242". (V6<br>ONLY)                                                     |
| post.GeocodeDistance       | –/ A     | GTA   | Distanz zum Geocode in Metern. (V6_ONLY)                                                                                                                   |
| post.GeocodeDirection      | –/ A     | GTA   | Richtung zum Geocode. (V6_ONLY)                                                                                                                            |

1 V5\_ONLY: Die internen Felder des Formats \*Descriptor wurden für die Version 6 der Addressvalidierungsengine jeweils durch die 2 Felder \*PreDesc und \*PostDesc abgelöst.

Beispiel: post.StreetDescriptor wurde ersetzt durch post.StreetPreDesc und post.StreetPostDesc

## <span id="page-123-0"></span>**9.2 SONDERFÄLLE**

Alle Post Felder werden so, wie sie befüllt werden, an den Adressvalidierungsmechanismus übergeben, mit einer Ausnahme:

Steht in dem Eingangsfeld, dass post.Street zugewiesen wird, ein Wert, der mit dem String *POSTBOX* (in Großschreibung) beginnt, und ist das Feld post.DeliveryService unbenutzt, so wird der Wert von post.Street nach post.DeliveryService kopiert. Das Straßenfeld wird dann geleert. Nach einer Adressüberprüfung wird dann ebenso ein eventuell gefülltes post.DeliveryService wieder auf post.Street zurückkopiert.

Der Grund für dieses Verhalten ist, dass oft Adressen überprüft werden, in denen im Straßenfeld das Postfach eingetragen ist und kein spezielles Postfachfeld vorhanden ist. Soll dieses Verhalten genutzt werden, so ist für das Eingangsfeld Straße eine Synonymliste anzugeben, die die üblichen Schreibweisen von Postfach, etc. in *POSTBOX* umwandelt. Da die Adressvalidierung die korrekte Bezeichnung für Postfach zurückgibt, ist für das Ausgangsfeld keine Synonymliste erforderlich.

Weitere Besonderheiten für einige Länder sind im *[Anhang: Länder](#page-200-0)* (Seite [193\)](#page-200-0) zu finden.

### <span id="page-124-0"></span>**9.3 STATUSINFORMATIONEN UND RÜCKGABEWERTE**

Sie können bei jeder Anfrage spezifische Statusinformationen erhalten, die sie für die Weiterverarbeitung der Ergebnisse verwenden können. Folgende Felder enthalten Statusinformationen:

- [post.ResultPercentage](#page-124-0)
- [post.ShortReport](#page-124-0)
- [post.ProcessStatus](#page-126-0)
- [post.ReturnCode](#page-131-0)
- [post.ElementInputStatus](#page-132-0)
- [post.ElementResultStatus](#page-134-0)
- [post.ElementRelevance](#page-136-0)
- [post.ExtElementStatus](#page-136-0)
- [post.MailabilityScore](#page-137-0) (V5\_ONLY)
- [post.ResultQuality](#page-138-0) (V6\_ONLY)
- [post.GeoCodingStatus](#page-138-1)
- [post.AddressType](#page-140-0)
- [post.AddressResolutionCode](#page-140-1) (V5\_ONLY)
- [post.SupplementaryStatus](#page-141-0) (V5\_ONLY)
- [post.AMASStatus](#page-141-1) (V5\_ONLY)
- [post.CASSStatus](#page-142-0) (V5\_ONLY)
- [post.CAMEOStatus](#page-142-1) (V5\_ONLY)

#### KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE **TOLERANT** Software

#### POST.RESULTPERCENTAGE

Hier erhalten Sie einen Wert zwischen 1 (Ergebnis mit schlechter Qualität) und 100 (Ergebnis mit bester Qualität) oder 0 (nicht verarbeitet). Ergebnisse unter 60 sollten Sie besser nicht maschinell weiterverarbeiten, da Eingabe und Ausgabe zu stark voneinander abweichen. Der Wert sagt **nichts** über die Qualität des Resultats bzgl. der Zustellbarkeit aus, sondern lediglich wie ähnlich das Ergebnis zu der Eingabe ist. Für die Zustellbarkeit ist post.MailabilityScore relevant.

#### POST.SHORTREPORT

Der post.ShortReport ist eine Kurzform des Verarbeitungsprotokolls post.ElementInputStatus und post.ElementResultStatus und wird in einer anderen Struktur dargestellt. Wenn keine detailreiche Ausgabe des Protokolls gewünscht wird, ist der Short Report eine Möglichkeit, die Verarbeitungsergebnisse in einer kurzen, übersichtlichen Form darzustellen. Der Short Report wird als 8-stelliger String zurück gegeben und ist wie folgt zu lesen:

| Position | Feld                          |  |  |
|----------|-------------------------------|--|--|
| 1        | post.Country, post.CountryEn, |  |  |
| 2        | post.PostalCode               |  |  |
| 3        | post.city                     |  |  |
| 4        | post.SubCity                  |  |  |
| 5        | post.Street                   |  |  |
| 6        | post.Number                   |  |  |
| 7        | post.DeliveryService          |  |  |
| 8        | post.Organization             |  |  |
|          |                               |  |  |

#### <span id="page-125-0"></span>Tabelle 9.2: Struktur des Feldes post.ShortReport

Die an den jeweiligen Stellen enthaltenen Werte haben folgende Bedeutung:

| Wert | Bedeutung                                                            |  |
|------|----------------------------------------------------------------------|--|
| –    | Nicht berücksichtigt/analysiert.                                     |  |
| *    | Der Status für das Feld konnte nicht ermittelt werden.               |  |
| 0    | Wert war ok und wurde unverändert übernommen.                        |  |
| 1    | Schreibweise wurde normiert.                                         |  |
| 2    | Wert korrigiert.                                                     |  |
| 3    | Veraltete Schreibweise gefunden und durch aktuelle Version ersetzt.  |  |
| 4    | Eingegebener Wert war mehrdeutig und konnte nicht korrigiert werden. |  |
| 5    | Eingabe falsch/Nicht ermittelbar.                                    |  |
| 6    | Angaben fehlen aber Wert konnte anderweitig ermittelt werden.        |  |
| 7    | Angaben fehlen und Wert konnte nicht anderweitig ermittelt werden.   |  |
| 8    | Postfach abgetrennt. (zurzeit nicht benutzt)                         |  |
| 9    | Postfachnummer war falsch. (zurzeit nicht benutzt)                   |  |
| A    | Feld war zu kurz. (zurzeit nicht benutzt)                            |  |
| B    | Hausnummer nicht umsetzbar. (zurzeit nicht benutzt)                  |  |
| C    | Nur normiert. (zurzeit nicht benutzt)                                |  |
| D    | Über StaB gefunden.                                                  |  |
| E    | Über StaB nicht gefunden.                                            |  |
| F    | Nicht ermittelt wegen fehlender StaB Referenzdaten.                  |  |
| G    | Wurde gestrichen. (zurzeit nicht benutzt)                            |  |
|      |                                                                      |  |

#### <span id="page-126-0"></span>Tabelle 9.3: Bedeutung der Werte im post.ShortReport

#### POST.PROCESSSTATUS

Der post.ProcessStatus gibt an, inwieweit die Eingangsadresse bearbeitet werden konnte. Der Status ist bei einer Abfrage für alle Resultate gleich. Folgende Werte können dabei angenommen werden.

#### POST.RETURNCODE

<span id="page-127-0"></span>Der post.ReturnCode fasst den post.ProcessStatus und den post.ShortReport zusammen.

| Wert | Bedeutung                                                                                                                       |
|------|---------------------------------------------------------------------------------------------------------------------------------|
| 0    | Ok                                                                                                                              |
| 1    | Ok, aber Adressen angepasst (nur bei FastCompletion- und FastCompletionExt-Modi)                                                |
| 2    | Adressen gefunden, aber die Adressen sind unvollständig (nur FastCompletion- und<br>FastCompletionExt-Modi)                     |
| 3    | Eingabe war für die Suche nach möglichen Adressen zu ungenau (nur FastCompletion<br>und FastCompletionExt-Modi)                 |
| 4    | Das Feld<br>hat einen fehlenden Wert oder ist nicht zugeordnet<br>post.AddressComplete<br>(Pflichtfeld im FastCompletion-Modus) |
| 10   | Fehler im Ort                                                                                                                   |
| 11   | Ort und Postleitzahl sind mehrdeutig                                                                                            |
| 13   | Fehler in der Postleitzahl                                                                                                      |
| 14   | Ortsteil falsch                                                                                                                 |
| 20   | Straßennamen ist falsch oder fehlt                                                                                              |
| 27   | Mehrdeutige Orts- / Straßenkombination                                                                                          |
| 30   | Postfach ist falsch oder fehlt                                                                                                  |
| 40   | Ländername ist falsch oder fehlt                                                                                                |
| 41   | Länderdaten sind nicht installiert                                                                                              |
| 42   | Unbekannter Ländercode (V5_Only)                                                                                                |
| 43   | Ländercode mehrdeutig (V5_Only)                                                                                                 |
| 44   | Kein Unlockcode für das angegebenen Land                                                                                        |
| 45   | Referenzdaten für Land zu alt oder defekt                                                                                       |

Tabelle 9.7: Bedeutung der Werte im post.ReturnCode

| Wert | Bedeutung                                                                                                                                                  |
|------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 46   | Lizenz nicht vorhanden oder nicht mehr gültig                                                                                                              |
| 50   | Firmenname ist falsch oder fehlt                                                                                                                           |
| 60   | Hausnummer falsch oder fehlt                                                                                                                               |
| 75   | Die Hausnummer war falsch oder fehlt (nur bei specialReturnCodes=Y)                                                                                        |
| 76   | Es gibt keine Referenzdaten für Hausnummern in dieser Straße (nur bei specialReturn<br>Codes=Y)                                                            |
| 77   | Es gibt keine Referenzdaten für Straßen in diesem Ort (nur bei specialReturnCodes=Y)                                                                       |
| 78   | Es gibt keine Referenzdaten für Postfächer (nur bei specialReturnCodes=Y)                                                                                  |
| 80   | Schlechte Qualität der Eingangsdaten                                                                                                                       |
| 99   | interner Fehler                                                                                                                                            |
| 100  | Gebäude falsch oder fehlt                                                                                                                                  |
| 110  | Wohnungsnummer falsch oder fehlt                                                                                                                           |
| 120  | Datenbankfehler im List-Modus                                                                                                                              |
| 121  | Distanz bei Distanzsuche zu groß                                                                                                                           |
| 122  | Distanz bei Distanzsuche negativ oder keine Zahl                                                                                                           |
| 124  | StaB- oder AGS-Eingangswert zu kurz                                                                                                                        |
| 125  | StaB- oder AGS-Eingangswert zu lang                                                                                                                        |
| 126  | Datenbankfehler beim Zugriff auf AGS/-StaB-datenbanken                                                                                                     |
| 130  | Fehler in der Cloud-Engine                                                                                                                                 |
| 131  | Lizenzen der Cloud-Engine sind entweder falsch oder abgelaufen                                                                                             |
| 140  | Keine Validierungsengine für das angegebenen Land                                                                                                          |
| 151  | Parsen lieferte mehrdeutiges Ergebnis                                                                                                                      |
| 152  | Reihenfolge der Eingabewerte wurde korrigiert                                                                                                              |
| 153  | Eingabe konnte nicht sinnvoll zerlegt werden                                                                                                               |
| 160  | Keine Adresse innerhalb des Suchradius gefunden oder Geocode liegt nicht im eingege<br>benen Land                                                          |
| 161  | Falsches Eingabeformat für Geocode (keine WGS 84 konforme Eingabe)                                                                                         |
| 162  | Fehlende Eingabe                                                                                                                                           |
| 163  | Mehrere<br>Eingabeoptionen<br>ausgewählt<br>(z.B.<br>bei<br>zeitgleicher<br>Befüllung<br>von<br>post.InputGeocodeLatitudeund<br>post.InputGeocodeComplete) |

| Wert | Bedeutung       |  |
|------|-----------------|--|
| 999  | interner Fehler |  |

| Wert | Bedeutung                                                                                                                                                 |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| V4   | Geprüft, alle Elemente in Ordnung                                                                                                                         |
| V3   | Geprüft, die meisten Elemente sind korrekt, manche Elemente sind eventuell in einer<br>veralteten Schreibweise oder konnten nicht richtig normiert werden |
| V2   | Geprüft, manche Elemente konnten aufgrund fehlender Referenzdateninformationen<br>nicht normiert werden                                                   |
| V1   | Geprüft, manche Elemente konnten nicht korrigiert werden                                                                                                  |
| C4   | Korrekturen, alle Elemente sind ok                                                                                                                        |
| C3   | Korrekturen, manche Elemente konnten nicht geprüft werden                                                                                                 |
| C2   | Korrekturen, Zustellstatus unklar                                                                                                                         |
| C1   | Korrekturen, Zustellstatus unklar, da manche Elemente nicht korrigiert werden konnten                                                                     |
| I4   | Daten sind fehlerhaft, Zustellung möglicherweise trotzdem möglich                                                                                         |
| I3   | Daten sind fehlerhaft. Es gibt mehrere Auswahlmöglichkeiten                                                                                               |
| I2   | Daten sind fehlerhaft und können nicht korrigiert werden                                                                                                  |
| I1   | Daten sind komplett fehlerhaft und können nicht korrigiert werden                                                                                         |
| Q3   | (nur im Modus FASTCOMPLETION/FASTCOMPLETION_EXT) Vorschläge sind verfügbar.<br>Adresse vollständig                                                        |
| Q2   | (nur im Modus FASTCOMPLETION/FASTCOMPLETION_EXT) Vorschlag ist vollständig                                                                                |
| Q1   | (nur im Modus FASTCOMPLETION/FASTCOMPLETION_EXT) Adresse ist noch nicht voll<br>ständig. Mehr Eingaben nötig                                              |
| Q0   | (nur im Modus FASTCOMPLETION/FASTCOMPLETION_EXT) Noch nicht genug Eingaben,<br>um Vorschläge zu liefern                                                   |
| S4   | (nur im Modus PARSE) Komplett zerlegt                                                                                                                     |
| S3   | (nur im Modus PARSE) Zerlegt mit mehreren Resultaten                                                                                                      |
| S2   | (nur im Modus PARSE) Zerlegt mit Fehlern (Reihenfolge geändert)                                                                                           |
| S1   | (nur im Modus PARSE) Eingabeformat nicht erkannt                                                                                                          |
| N1   | Validierungsfehler: Land nicht erkannt                                                                                                                    |
| N2   | Validierungsfehler: Keine Referenzdaten gefunden                                                                                                          |
| N3   | Validierungsfehler: Keine Lizenz für das Land                                                                                                             |
| N4   | Validierungsfehler: Referenzdatenbank defekt                                                                                                              |
| N5   | Validierungsfehler: Referenzdatenbank zu alt                                                                                                              |
| N6   | Validierungsfehler: Keine Adresszeilenvalidierung für das Land                                                                                            |
| N7   | Validierungsfehler: Adresszeilenvalidierung ist nicht freigeschaltet (Unlock-Code fehlt)                                                                  |
| L0   | (nur im Modus LIST): alles ok                                                                                                                             |
| L1   | (nur im Modus LIST): Fehler                                                                                                                               |
| A1   | Adresse mit Rückwärtssuche angereichert                                                                                                                   |

<span id="page-130-0"></span>Tabelle 9.4: Bedeutung der Werte im post.ProcessStatus (Version 5 der Adressvalidierungs-Engine)

<span id="page-131-1"></span>Tabelle 9.5: Bedeutung der Werte im post.ProcessStatus (Version 6 der Adressvalidierungs-Engine)

| Wert | Bedeutung                                                                                                                               |
|------|-----------------------------------------------------------------------------------------------------------------------------------------|
| V    | Alle Adresselemente wurden geprüft und stimmen mit den Referenzdaten überein                                                            |
| A    | Eines oder mehrere Adresselemente der Adresse wurden ergänzt                                                                            |
| C    | Eines oder mehrere Adresselemente wurden korrigiert oder normalisiert                                                                   |
| I    | Adresse konnte nicht korrigiert werden und ist nicht valide                                                                             |
| N    | Adresse wurde nicht in den Referenzdaten gefunden oder das angegebene Land wird<br>nicht unterstützt                                    |
| F    | Vollständige Adressen wurden in der Vorschlagsliste zurück geliefert (Nur im Modus<br>Addressvervollständigung / Fast Completion Mode)  |
| P    | Angereicherte Adressen wurden in der Vorschlagsliste zurück geliefert (Nur im Modus<br>Addressvervollständigung / Fast Completion Mode) |

<span id="page-131-0"></span>

| Tabelle 9.6:<br>Bedeutung der Werte im<br>post.ProcessStatus<br>(nur bei Modus GEOCODE_TO_ADDRESS |
|---------------------------------------------------------------------------------------------------|
| und Version 6 der Adressvalidierungs-Engine)                                                      |

| Wert | Bedeutung                                                                                                         |
|------|-------------------------------------------------------------------------------------------------------------------|
| I    | Geocode nicht im referenzierten Land oder keine Adresse innerhalb des angegebenen<br>Suchradius gefunden          |
|      |                                                                                                                   |
| N    | Adresse wurde nicht in den Referenzdaten gefunden oder das angegebene Land wird<br>nicht unterstützt              |
| F    | Eine oder mehrere Adressen konnten innerhalb des Suchradius und wurden in der<br>Vorschlagsliste zurück geliefert |

POST.ELEMENTINPUTSTATUS, POST.ELEMENTRESULTSTATUS, POST.ELEMENTRELEVANCE, POST.EXTELEMENTSTATUS

Die Werte post.ElementInputStatus, post.ElementResultStatus, post.ElementRelevance und post.ExtElementStatus benutzen die gleiche Feldstruktur, die in Tabelle [9.8:](#page-132-0) *['Struktur der](#page-132-0) Felder* [post.Element\\*](#page-132-0)*'* beschrieben ist.

| Position | Feld                             |
|----------|----------------------------------|
| 1        | post.PostalCode                  |
| 2        | post.PostalCode2                 |
| 3        | post.Locality (City)             |
| 4        | post.SubCity                     |
| 5        | post.Province1                   |
| 6        | post.Province2                   |
| 7        | post.Street                      |
| 8        | post.SubStreet                   |
| 9        | post.Number                      |
| 10       | post.Number2                     |
| 11       | post.DeliveryService             |
| 12       | post.DeliveryService2            |
| 13       | post.Building                    |
| 14       | post.Building2                   |
| 15       | post.Entrance (post.SubBuilding) |
| 16       | post.Floor (post.SubBuilding2)   |
| 17       | post.Organization                |
| 18       | post.Organization2               |
| 19       | post.Country, post.CountryEn,    |
| 20       | post.Territory                   |

<span id="page-132-0"></span>Tabelle 9.8: Struktur der Felder post.Element\*

#### POST.ELEMENTINPUTSTATUS

Der post.ElementInputStatus ist eine ausführliche Bewertung der Eingabewerte. Jedes Zeichen in der 20-stelligen Zeichenkette hat eine entsprechende Bedeutung für die geprüften Felder. Die an den jeweiligen Stellen enthaltenen Werte haben folgende Bedeutung:

<span id="page-133-0"></span>Tabelle 9.9: Werte des Feldes post.ElementInputStatus (Version 5 der Adressvalidierungs-Engine)

| Wert | Bedeutung                                                |
|------|----------------------------------------------------------|
| 0    | Keine Angabe gefunden                                    |
| 1    | Nicht gefunden                                           |
| 2    | Nicht geprüft (Keine Referenzdaten vorhanden)            |
| 3    | Fehler / falsch                                          |
| 4    | Angaben in Referenzen gefunden (mit Fehlern)             |
| 5    | Angaben in Referenzen gefunden (mit kleinen Korrekturen) |
| 6    | Angaben in Referenzen gefunden (ohne Fehler/Korrekturen) |

#### <span id="page-134-0"></span>Tabelle 9.10: Werte des Feldes post.ElementInputStatus (**Version 6** der Adressvalidierungs-Engine)

| Wert | Bedeutung                                                                                                                                                                                                                                  |
|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0    | Keine Angabe gefunden                                                                                                                                                                                                                      |
| 1    | Die Referenzdaten enthalten nicht das Eingabeadressenelement für das Land als Ganzes<br>oder für eine bestimmte Adresse, einen bestimmten Ort und eine bestimmte Straße                                                                    |
| 2    | Die Referenzdaten enthalten keine Übereinstimmung mit dem Wert des eingegebenen<br>Elements und die Eingabeadresse enthält nicht genügend überprüfbare Elemente, um<br>eine Schätzung eines korrekten Werts für das Element zu ermöglichen |
| 3    | Die Referenzdaten enthalten keine Übereinstimmung mit dem Wert des Eingabeele<br>ments und die Adressprüfung kann den Wert nicht korrigieren. Die Adressprüfung lehnt<br>die Adresse ab                                                    |
| 4    | Der Wert des Eingabeelements stimmt teilweise mit den Referenzdaten überein                                                                                                                                                                |
| 5    | Der Wert des Eingabeelements stimmt mit den Referenzdaten überein, aber die Adress<br>überprüfung hat die Daten korrigiert                                                                                                                 |
| 6    | Der Wert des Eingangselements stimmt mit den Referenzdaten innerhalb des verfügba<br>ren Bereichs überein                                                                                                                                  |
| 7    | Der Wert des Eingabeelements stimmt mit den Referenzdaten überein, aber die Daten<br>wurden normalisiert                                                                                                                                   |
| 8    | Der Wert des eingegebenen Elements stimmt perfekt mit den Referenzdaten überein<br>(ohne Fehler/Korrekturen)                                                                                                                               |

#### POST.ELEMENTRESULTSTATUS

Der post.ElementResultStatus ist eine ausführliche Bewertung der Ausgabewerte. Jedes Zeichen in der 20-stelligen Zeichenkette hat eine entsprechende Bedeutung für die geprüften Felder.

<span id="page-135-0"></span>Tabelle 9.11: Werte des Feldes post.ElementResultStatus (Version 5 der Adressvalidierungs-Engine)

| Wert | Bedeutung                                                                                                                                   |
|------|---------------------------------------------------------------------------------------------------------------------------------------------|
| 0    | Keine Angabe gefunden und zurückgeliefert                                                                                                   |
| 1    | Ungeprüft, Originalbeitrag wurde übernommen                                                                                                 |
| 2    | Nicht geprüft. Standardisierung wurde vorgenommen                                                                                           |
| 3    | Ungenügende/fehlerhafte Eingabe                                                                                                             |
| 4    | fehlende Referenzdaten                                                                                                                      |
| 5    | Mehrfachauswahl                                                                                                                             |
| 6    | Eingabewerte wurden gelöscht                                                                                                                |
| 7    | Korrigiert anhand der Referenzdaten                                                                                                         |
| 8    | Korrekturen und Ergänzungen anhand von Referenzdaten                                                                                        |
| 9    | Keine Änderungen, Angaben vermutlich fehlerhaft, da sie außerhalb des geforderten<br>Wertebereiches liegen (z.B. bei zu großen Hausnummern) |
| C    | Angaben waren veraltet und wurden korrigiert. (Archiveinträge)                                                                              |
| D    | Angaben wurden anhand der offiziellen Schreibweise geändert                                                                                 |
| E    | Angaben wurden standardisiert                                                                                                               |
| F    | Perfekte Angaben, keine Änderungen notwendig                                                                                                |
|      |                                                                                                                                             |

<span id="page-136-0"></span>Tabelle 9.12: Werte des Feldes post.ElementResultStatus (**Version 6** der Adressvalidierungs-Engine)

| Wert | Bedeutung                                                                                                                                                                                                   |
|------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Adresselement wurde nicht verändert                                                                                                                                                                         |
| 2    | Adresselement wurde normalisiert                                                                                                                                                                            |
| 3    | Adresselement wurde ersetzt, weil die Eingabe veraltete Synonyme, archivierte Namen<br>oder eine Kombination von Sprachen enthält                                                                           |
| 4    | Bei der Adressüberprüfung wurde der sekundäre Teil der Elemente geändert. Die Adress<br>überprüfung korrigierte zum Beispiel Straßenbezeichnungen, Richtungsangaben oder<br>die +4-Daten in Postleitzahlen. |
| 5    | Bei der Adressüberprüfung wurde der primäre Teil der Elemente geändert. Die Adress<br>überprüfung korrigierte beispielsweise Straßennamen oder fünfstellige Postleitzahlen.                                 |
| 6    | Die Adressüberprüfung hat das Element zur Ergebnisausgabe hinzugefügt.                                                                                                                                      |

#### KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE **TOLERANT** Software

#### POST.ELEMENTRELEVANCE

Das Feld post.ElementRelevance ist eine ausführliche Bewertung der Relevanz der Ausgabewerte. Jedes Zeichen in der 20-stelligen Zeichenkette hat eine entsprechende Bedeutung für die geprüften Felder. Ist das Zeichen an einer Position eine *0*, so ist das Feld für eine Anschrift nicht notwendig. Ist das Zeichen eine *1*, so ist das Feld notwendig.

#### POST.EXTELEMENTSTATUS

Der post.ExtElementStatus beschreibt das Ergebnis und gibt an, ob mehr Informationen vorhanden sind. Jedes Zeichen in der 20-stelligen Zeichenkette hat eine entsprechende Bedeutung für die geprüften Felder.

| Wert | Bedeutung                                                               |
|------|-------------------------------------------------------------------------|
| 0    | Keine Informationen vorhanden                                           |
| 1    | Informationen vorhanden, aber nicht zurückgeliefert                     |
| 2    | Nicht geprüft, Standardisierung wurde vorgenommen                       |
| 3    | Korrekte numerische Anhabe, Standardisierung wurde vorgenommen          |
| 4    | Korrekt/Nicht geprüft, Angabe falsch formatiert                         |
| 5    | Alternativen vorhanden                                                  |
| 6    | Teile der Eingabe nicht geprüft                                         |
| 7    | Level-Änderung wurde vorgenommen                                        |
| 8    | Typänderung wurde vorgenommen                                           |
| 9    | Verletzung einer generellen postalischen Autoritätsregel                |
| A    | Eingabe beinhaltet zweideutigen Elemente                                |
| B    | Relevanz wurde anhand einer landesweiten Regel bestimmt (unzuverlässig) |
| C    | Zu viele Ergebnisse (FastCompletion/FastCompletionExt)                  |
| D    | Numerische Ausgabe interpoliert                                         |
| E    | Ausgabesprache nicht vorhanden                                          |
| F    | Ausgaben sind veraltet                                                  |
|      |                                                                         |

#### <span id="page-137-0"></span>Tabelle 9.13: Werte des Feldes post.ExtElementStatus

#### POST.MAILABILITYSCORE

Das Feld post.MailabilityScore beschreibt die Güte der Zustellbarkeit in Version 5 der Adressvalidierungs-Engine.

| Bedeutung                                                  |  |  |
|------------------------------------------------------------|--|--|
| Alle Elemente konnten validiert werden, sichere Zustellung |  |  |
| Recht sichere Zustellung                                   |  |  |
| Unsicherheiten in der Adresse. Zustellung aber möglich     |  |  |
| Größere Unsicherheiten in der Adresse                      |  |  |
| Adresse fehlerhaft                                         |  |  |
| Zustellung unmöglich                                       |  |  |
|                                                            |  |  |

#### <span id="page-138-0"></span>Tabelle 9.14: Werte des Feldes post.MailabilityScore

#### POST.RESULTQUALITY

Das Feld post.ResultQuality beschreibt die Güte der Zustellbarkeit in **Version 6** der Adressvalidierungs-Engine.

| Wert | Bedeutung                                                  |
|------|------------------------------------------------------------|
| 6    | Alle Elemente konnten validiert werden, sichere Zustellung |
| 5    | Recht sichere Zustellung                                   |
| 4    | Sekundärer Adressteil nicht verifiziert                    |
| 3    | Primärer Adressteil nicht verifiziert                      |
| 2    | Zustellinformationen konnten nicht verifiziert werden      |
| 1    | Unvollständige oder widersprüchliche Adresse               |
| 0    | Keine Adresse erkannt                                      |

#### <span id="page-138-1"></span>Tabelle 9.15: Werte des Feldes post.ResultQuality

#### KAPITEL 9. ADRESSFELDER UND RÜCKGABEWERTE **TOLERANT** Software

#### POST.GEOCODINGSTATUS

Das Feld post.GeoCodingStatus beschreibt das Ergebnis der Geokoordinatenermittlung.

| Wert | Bedeutung                                                                       |  |
|------|---------------------------------------------------------------------------------|--|
| EGCN | Keine Referenzdaten für Geokoordinaten gefunden                                 |  |
| EGCU | Referenzdaten für Geokoordinaten nicht freigegeben                              |  |
| EGCC | Referenzdaten für Geokoordinaten fehlerhaft                                     |  |
| EGC0 | Keine Geokoordinaten vorhanden                                                  |  |
| EGC4 | Geocode vorhanden – Genauigkeit auf Ebene eines Teiles der Postleitzahl         |  |
| EGC5 | Geocode vorhanden – Genauigkeit auf Postleitzahlenebene                         |  |
| EGC6 | Geocode vorhanden – Genauigkeit auf Ortsebene                                   |  |
| EGC7 | Geocode vorhanden – Genauigkeit auf Straßenebene                                |  |
| EGC8 | Geocode vorhanden – Genauigkeit auf Hausnummernebene                            |  |
| EGC9 | Geocode vorhanden – Genauigkeit punktuell auf den Anfang einer Parzelle         |  |
| EGCA | Geocode vorhanden – Genauigkeit punktuell auf das Zentrum einer Parzelle        |  |
| EGCB | Geocode vorhanden – Genauigkeit punktuell auf den höchsten Punkt einer Parzelle |  |
| EGCF | Mehrere Geokoordinaten gefunden – der erste Treffer wird zurückgeliefert        |  |
|      |                                                                                 |  |

<span id="page-139-0"></span>Tabelle 9.16: Werte des Feldes post.GeoCodingStatus (Version 5 der Adressvalidierungs-Engine)

#### POST.ADDRESSTYPE

Das Feld post.AddressType beschreibt den Typ der ermittelten Adresse.

| Wert               | Bedeutung                                                                                                                |
|--------------------|--------------------------------------------------------------------------------------------------------------------------|
| ErDataNotAvailable | Keine Referenzdaten für Geokoordinaten gefunden                                                                          |
| ErNotUnlocked      | Referenzdaten für Geokoordinaten nicht freigegeben                                                                       |
| ErDataCorrupt      | Referenzdaten für Geokoordinaten fehlerhaft                                                                              |
| NothingFound       | Keine Geokoordinaten vorhanden                                                                                           |
| PocoBaseCenter     | Geocode vorhanden – Genauigkeit auf Ebene eines Teiles der Postleitzahl, z.B.<br>701xx                                   |
| PocoCenter         | Geocode vorhanden – Genauigkeit auf Postleitzahlenebene                                                                  |
| LocalityCenter     | Geocode vorhanden – Genauigkeit auf Ortsebene                                                                            |
| StreetCenter       | Geocode vorhanden – Genauigkeit auf Straßenebene                                                                         |
| Interpolated       | Die Geokoordinaten sind bis auf Hausnummernebene genau. (geschätzte Lage<br>des Grundstücks mit straßenseitigem Versatz) |
| PointArrivalPoint  | Hochpräzise Ankunftspunkt-Geokoordinaten (gemessener Eingang des Flur<br>stücks)                                         |
| PointRooftop       | Hochpräzise Ankunftspunkt-Geokoordinaten auf Basis der Lage des Hausdachs                                                |

<span id="page-140-0"></span>Tabelle 9.17: Werte des Feldes post.GeoCodingStatus (**Version 6** der Adressvalidierungs-Engine)

#### <span id="page-140-1"></span>Tabelle 9.18: Werte des Feldes post.AddressType

| Wert                      | Bedeutung                                                      |  |
|---------------------------|----------------------------------------------------------------|--|
| B(=building name)         | Adresse wurde validiert/korrigiert zu einem Gebäudenamen       |  |
| F(=firm)                  | Adresse wurde validiert/korrigiert zu einem Firmennamen        |  |
| G(=general delivery)      | Normale Postanschrift                                          |  |
| H(=high-rise Building)    | Adresse wurde validiert/korrigiert zu einem Hochhaus (Default) |  |
| L(=large volume receiver) | Adresse ist ein Großkundenempfänger                            |  |
| M(=military)              | Militärische Adresse                                           |  |
| P(=postal)                | Adresse wurde validiert/korrigiert zu Postfachadresse          |  |
| R(=rural)                 | Adresse wurde validiert/korrigiert zu einer Landstraße         |  |
| S(=street)                | Adresse wurde validiert/korrigiert zu einer Straßenadresse     |  |
| U(=unknown)               | Adresse wurde nicht validiert/korrigiert                       |  |

#### POST.ADDRESSRESOLUTIONCODE

Der post.AddressResolutionCode beschreibt, warum die Adresse nicht validiert werden konnte. Jedes Zeichen in der 20-stelligen Zeichenkette hat eine entsprechende Bedeutung für die geprüften Felder.

| Wert | Bedeutung                                                                                    |  |
|------|----------------------------------------------------------------------------------------------|--|
| 0    | Keine Informationen vorhanden                                                                |  |
| 2    | Keine Angabe des Adresselementes gefunden                                                    |  |
| 3    | Falsche numerische Angabe, z.B. wenn die Hausnummer nicht zu einem validen Bereich<br>gehört |  |
| 4    | Mehrfachangabe des Elementes gefunden                                                        |  |
| 5    | Eingabe ist mehrdeutig / Mehrfach gefunden                                                   |  |
| 6    | Das Element hat Konflikte mit anderen Elemente der Eingabe                                   |  |
| 7    | Zu viele Korrekturen sind notwendig                                                          |  |
| 8    | Verletzung einer generellen postalischen Autoritätsregel                                     |  |

#### <span id="page-141-0"></span>Tabelle 9.19: Werte des Feldes post.AddressResolutionCode

#### POST.SUPPLEMENTARYSTATUS

Der post.SupplementaryStatus beschreibt das Ergebnis der Ermittlung von ergänzenden Daten.

#### <span id="page-141-1"></span>Tabelle 9.20: Werte des Feldes post.SupplementaryStatus

| Wert | Bedeutung                                                                                                                        |  |
|------|----------------------------------------------------------------------------------------------------------------------------------|--|
| N    | Keine Referenzdaten für ergänzende Daten gefunden                                                                                |  |
| U    | Referenzdaten für ergänzende Daten nicht freigegeben                                                                             |  |
| C    | Referenzdaten für ergänzende Daten fehlerhaft                                                                                    |  |
| 0    | Keine ergänzende Daten vorhanden                                                                                                 |  |
| 1    | Ergänzende Daten vorhanden – Dieser Status bedeutet nicht, dass alle verfügbaren<br>Anreicherungen in der Ausgabe enthalten sind |  |

#### POST.AMASSTATUS

<span id="page-142-0"></span>Der post.AMASStatus beschreibt das Ergebnis der Ermittlung von den zertifizierten AMAS-Daten.

| Tabelle 9.21:<br>Werte des Feldes<br>post.AMASStatus |                                                                                     |  |
|------------------------------------------------------|-------------------------------------------------------------------------------------|--|
| Wert                                                 | Bedeutung                                                                           |  |
| EAM0                                                 | AMAS-Ausgabe ist nicht verfügbar                                                    |  |
| EAM1                                                 | AMAS-Ausgabe ist vorhanden                                                          |  |
| EAM2                                                 | AMAS-Ausgabe ist nicht vorhanden – Adresse konnte nicht validiert/korrigiert werden |  |

#### POST.CASSSTATUS

Der post.CASSStatus beschreibt das Ergebnis der Ermittlung von den zertifizierten CASS-Daten.

<span id="page-142-1"></span>

| Tabelle 9.22: | Werte des Feldes | post.CASSStatus |
|---------------|------------------|-----------------|
|               |                  |                 |

| Wert | Bedeutung                                    |
|------|----------------------------------------------|
| ECA0 | CASS-Ausgabe ist nicht verfügbar             |
| ECA1 | CASS-Ausgabe ist nicht vollständig verfügbar |
| ECA5 | CASS-Ausgabe verfügbar                       |

#### POST.CAMEOSTATUS

Der post.CAMEOStatus beschreibt das Ergebnis der Ermittlung von CAMEO-Daten.

| Wert | Bedeutung                                                      |
|------|----------------------------------------------------------------|
| ECON | Keine Referenzdaten für CAMEO gefunden                         |
| ECO0 | CAMEO-Ausgabe nicht verfügbar                                  |
| ECO1 | CAMEO-Ausgabe ist vorhanden                                    |
| ECOI | CAMEO-Ausgabe ist nicht vorhanden – Die Eingabe ist mehrdeutig |

#### <span id="page-142-2"></span>Tabelle 9.23: Werte des Feldes post.CAMEOStatus
