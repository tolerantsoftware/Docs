  

## **15 Sicherheitshinweise**

<span id="page-196-1"></span><span id="page-196-0"></span>Im Folgenden wird auf mögliche Sicherheitsproblematiken hingewiesen.

## **15.1 PORTS UND FIREWALLS**

POST Batch ist im Normalfall so konfiguriert, dass ein Batch-Prozess über einen speziellen Port abgebrochen werden kann. Der Port ist standardmäßig auf 8979 eingestellt. Soll dieser Mechanismus nicht genutzt werden, so kann dies durch Setzen des Attributs abortPort in der postRuntime auf den Wert "OFF" unterbunden werden. Beachten Sie, dass auch die Benutzeroberfläch[e¹](#page-196-2) diesen Mechanismus nutzt, um einen Batchlauf auf Wunsch vorzeitig abzubrechen.

<span id="page-196-2"></span><sup>1</sup> Nicht in allen Produkten verfügbar

#### BENUTZEROBERFLÄCHE

Die Benutzeroberfläche benutzt je nach Art der Installation entweder den gleichen Port wie der Service, oder einen eigenen, lokalen Port. Der lokale Port muss nicht nach außen freigegeben werden.

## <span id="page-197-0"></span>**15.2 PASSWÖRTER**

In der Konfiguration von POST Batch müssen eventuell Passwörter für die benutzten Datenbanken hinterlegt werden (s. Abschnitt *['Datenbanken'](#page-44-0)* (Seite [37\)](#page-44-0)).

Wird eine externe PostEngine benutzt, so sind Passwörter für den Engine-Account und eventuell für einen Internet-Proxy zu konfigurieren.

#### VERSCHLÜSSELUNG DER PASSWÖRTER

Passwörter, die in der Konfiguration eines Services oder Batches oder in der auth-users.json vorkommen, können unverschlüsselt angegeben werden. Dann ist vor das Passwort ein '#' (Hash-Zeichen) anzugeben. Daraus folgt, dass Passwörter selbst nicht mit einem Hashzeichen anfangen dürfen. Verschlüsselte Passwörter beginnen nie mit einem Hashzeichen.

Konfiguriert man einen Service oder einen Batchlauf direkt über das GUI, so wird dort die Verschlüsselung automatisch vorgenommen.

Erstellt man eine Konfigurationsdatei von Hand, so kann mit Hilfe eines Skripts ein Passwort verschlüsselt werden.

Dazu übergeben Sie das unverschlüsselte Passwort (ob mit oder ohne Hashzeichen ist egal) an das Skript und übertragen anschließend das ausgegebene verschlüsselte Passwort in die Konfiguration.

#### AUFRUF UNTER WINDOWS

Öffnen Sie einen Windows Kommandoprompt (cmd) und tippen Sie folgende Kommandos:

```
cd <Installationsverzeichnis >
bin\security.bat --password <unverschlüsseltesPasswort >
```

#### AUFRUF UNTER LINUX

Öffnen Sie ein Terminalfenster und tippen Sie folgende Kommandos:

```
cd <Installationsverzeichnis >
bin/security.sh --password <unverschlüsseltesPasswort >
```

## <span id="page-198-0"></span>**15.3 KUNDENBEZOGENE DATEN**

POST Batch speichert während des Betriebs keinerlei Daten in Datenbanken, allerdings könnten kundenspezifische Daten aus den Protokolldateien des Services ermittelt werden. Über Einstellungen in der log4J2.yml Konfigurationsdatei lassen sich solche Daten in den Logdateien anonymisieren oder die Protokolldateien abschalten. Details zu den Einstellungen finden Sie im Abschnitt *'***??***'* (Seite **??**). Achten Sie bitte auch darauf, dass diese Dateien periodisch gelöscht werden und die Zugriffsrechte korrekt gesetzt sind.
