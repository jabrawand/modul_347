# Docker Projekt - Microservices für KMU

**Modul**: 347 - Dienst mit Container anwenden
**Projekt:** Docker Microservices mit docker-compose  
**Autor:** Jasmin Brawand 
**Klasse:** UIFZ-2425-005 
**Datum:** [Abgabedatum] TODO 

---

## Inhaltsverzeichnis
1. [Einleitung]
2. [Informieren]
3. [Planen]
4. [Entscheiden]
5. [Realisieren]
6. [Testen]
7. [Kontrollieren]
8. [Auswerten]
9. [Anhang]

---

## Einleitung
In dieser Dokumentation wird die Umsetzung eines Docker-Projekts für ein Informatik-KMU beschrieben. Ziel des Projekts ist es, eine funktionierende Container-Infrastruktur bereitzustellen, die verschiedene firmenintere Dienste integriert, stabil läuft und nachvollziehbar dokumentiert ist.
Die Dokumentation orientiert sich am IPERKA-Modell und zeigt die einzelnen Projektschritte von der Analyse über die Planung und Umsetzung bis hin zur Kontrolle und Auswertung. Dabei liegt der Fokus sowohl auf der technischen Umsetzung sowie dem Testen und der Dokumentation der Ergebnisse.

---

## 1. Informieren

### 1.1 Ausgangslage
Im Rahmen dieses Projekts soll für ein Informatik-KMU eine containerisierte Serverlösung mit mehreren Diensten umgesetzt werden. Ziel ist es, mit Docker und docker-compose einen Applikationsstack aufzubauen, der verschiedene firmeninterne Services bereitstellt. Die Lösung soll übersichtlich dokumentiert, getestet und versioniert werden.

Der Fokus liegt auf dem Einsatz von Microservices sowie auf der persistenten Speicherung der Daten, um einen realitätsnahen Betrieb sicherzustellen.

---

### 1.2 Analyse der Anforderungen
Der Auftraggeber hat die folgenden Anforderungen definiert:
- Bereitstellung eines Media-Wiki für firmeninterne Zwecke
- Einsatz der Open-Source Plattform Nextcloud für Filesharing und Kollaboration
- Verwaltung des Quellcodes über eine eigene GitLab oder Gogs Instanz
- Persistente Speicherung aller Daten und eingesetzten Dienste
- Überwachung der Verwaltung mittels Portainer
- Versionierung der notwendigen Dateien sowie der Systemdokumentation in einem privaten GitHub-Repository

Zusätzlich wurde darauf hingewiesen, dass GitLab hohe Hardwareanforderungen hat, während Gogs eine ressourcenschonende Alternative darstellt.

**Tabelle 1.1 - Funktionale Anforderungen in Tabellarischer Form**
| Dienst    | Beschreibung                | Port |
| --------- | --------------------------- | ---- |
| MediaWiki | Internes Firmen-Wiki        | 8085 |
| Nextcloud | Filesharing & Kollaboration | 8080 |
| Git-Server | Gitlab oder Gogs           |      |
| Portainer | Monitoring                  | 9000 |

---

### 1.3 Randbedingungen und Einschränkungen
Für die Umsetzung des Projekts gelten folgende Randbedingungen:
- Die Lösung wird auf einer virtuellen Maschine betrieben
- Begrenzte Ressourcen (RAM und Speicherplatz)
- Einsatz von Docker und docker-compose ist zwingend
- Alle Dienste müssen nach einem Neustart weiterhin funktionsfähig sein
- Die Lösung muss verständlich dokumentiert sein
  
  ---

### 1.4 Technische Vorabklärungen
Für die Umsetzung kommen folgende Technologien grundsätzlich in Frage:
- Docker und docker-compose zur Containerisierung und Orchestrierung der einzelnen Dienste (gemäss Anforderungen vorgegeben)
- MariaDB, MySQL oder PostgreSQL als Datenbank für MediaWiki und Nextcloud
- Docker Volumes oder Bind Mounts zu persisitenten Datenspeicherung
- Portainer zu grafischen Verwaltung und Überwachung der Container (gemäss Anforderungen vorgegeben)

Die genaue Auswahl und er konkrete Systemaufbau werden in der Phase *Entscheidung* festgelegt.

---

### 1.5 Zielsetzung des Projekts
Ziel dieses Projekts ist es, eine funktionsfähige und dokumentierte Docker-Umgebung bereitzustellen, welche die Anforderungen des Auftraggebers erfüllt. Dabei sollen insbesondere folgende Ziele erreicht werden:
- Aufbau eines Microsoft-Stacks mit docker-compose
- Einsatz eine Testkonzepts und Durchführung der Tests
- Strukturierte Dokumentation nach dem IPERKA-Modell

---

## 2. Planen
In diesem Kapitel wird die Planung des Projekts beschrieben. Dazu gehören der Zeitplan, die benötigten Ressourcen sowie die Planung von Risiken und Sicherheitsaspekten.

### 2.1 Arbeitsplanung und Zeitplan
**Tablelle 2.1 - Arbeitsplanung nach IPERKA-Phasen**

| Phase | Aufgabe | geschätzte Zeit |
| ----- | ------- | --------------- |
| Informieren | Anforderungen und Randbedingungen analysieren | 1 h |
| Planen |
| Entscheiden |
| Realisieren | 
| Testen |
| Kontrollieren | 
| Auswerten | Dokumentation abschliessen, Reflektion | 2 h |

Da ich dieses Projekt als Einzelarbeit mache fällt die Arbeitsaufteilung weg.

---

### 2.2 Ressourcenplanung
Für die Umsetzung werden folgende Ressourcen benötigt:
- **Hardware**: Virtuelle Maschine mit mindestens 4 GB RAM und 6 GB freiem Speicherplatz (für GitLab evt. höher)
-  **Software**: Docker, docker-compose, Portainer, Nextcloud, MediaWiki, GitLab oder Gogs
- **Daten**: Persistente Volumes für alle Container zur Speicherung von Nutzerdaten und Konfigurationen
- **Sonstiges**: GitHub-Repository für Versionierung der Dateien und Dokumentation

**Tabelle 2.2 – Geplante Ressourcen je Dienst**

| Dienst     | RAM     | Speicherplatz | Bemerkung                  |
| ---------- | ------- | ------------- | -------------------------- |
| MediaWiki  | 300 MB  | 1 GB          | Standard Container         |
| Nextcloud  | 300 MB  | 2 GB          | inkl. Datenpersistenz      |
| Git-Server | 700 MB  | 3 GB          | GitLab benötigt mehr RAM   |
| Portainer  | 100 MB  | 200 MB        | Monitoring                 |

---

### 2.3 Risiken und Einschränkungen
Mögliche Risiken wurden im Vorfeld berücksichtigt:

- **Ressourcenengpässe:** Zu wenig RAM oder Speicher auf der VM kann zu Abstürzen führen  
- **Fehlerhafte Container-Konfiguration:** Dienste starten nicht oder Daten gehen verloren  
- **Sicherheitsrisiken:** Offene Ports, Standardpasswörter oder Root-Rechte in Containern  
- **Abhängigkeiten der Dienste:** Einige Dienste benötigen andere Dienste (z.B. MediaWiki / Nextcloud auf Datenbank angewiesen)

**Tabelle 2.3 – Risiken und Massnahmen**

| Risiko                         | Mögliche Auswirkung                  | Geplante Massnahme                               |
| ------------------------------- | ----------------------------------- | ------------------------------------------------ |
| Ressourcenengpass               | Dienste stürzen ab                   | RAM-Limits setzen, Ressourcen beobachten        |
| Container starten nicht         | Dienste nicht erreichbar             | Testen vor Produktion, Logs prüfen              |
| Sicherheitslücken               | Datenverlust, Angriffe               | Passwörter auslagern, Ports beschränken         |
| Abhängigkeiten zwischen Diensten | Dienste starten nicht in der richtigen Reihenfolge | `depends_on` in docker-compose nutzen |

---

### 2.4 Zusammenfassung Planung

Die Planungsphase legt den Grundstein für eine strukturierte Umsetzung.  
Arbeitsabläufe, Ressourcen und Risiken sind dokumentiert, um einen reibungslosen Aufbau der Docker-Infrastruktur sicherzustellen.

---

## 3. Entscheiden

In dieser Phase werden auf Basis der Analyse und Planung die konkreten Technologieentscheidungen für die Umsetzung des Projekts getroffen. Die Auswahl erfolgt unter Berücksichtigung der Anforderungen, der vorhanden Ressourcen sowie der Praxistauglichkeit der eingesetzten Lösungen.

### 3.1 Auswahl der Container-Technologien

Für die Containerisierung und Orchestrierung der Dienste wird **Docker in Kombination mit docker-compose** eingesetzt. Diese Technologien ermöglichen eine übersichtliche Definition der einzelnen Microservices, deren Abhängigkeiten sowie eine einfache Inbetriebnahme und Wartung der gesamten Infrastruktur.

Diese Entscheidung basiert auf den vorgegebenen Randbedingungen sowie auf der guten Dokumentation und weiten Verbreitung der Technologien.

---

### 3.2 Auswahl der Datenbank

Für den Betrieb von MediaWiki und Nextcloud wird eine relationale Datenbank benötigt. Im Vorfeld wurden die gängigen Open-Source-Datenbanksysteme MariaDB, MySQL und PostgreSQL hinsichtlich ihrer Eignung für die beiden Anwendungen verglichen.

**Tabelle 3.1 – Vergleich der Datenbanksysteme im Hinblick auf MediaWiki und Nextcloud**

| Kriterium              | MariaDB                              | MySQL                                | PostgreSQL                           |
| ---------------------- | ------------------------------------ | ------------------------------------ | ------------------------------------ |
| Unterstützung MediaWiki | Offiziell unterstützt                | Offiziell unterstützt                | Unterstützt                          |
| Unterstützung Nextcloud | Offiziell unterstützt                | Offiziell unterstützt                | Offiziell unterstützt                |
| Kompatibilität         | Sehr hoch (MySQL-kompatibel)          | Hoch                                 | Gut, aber abweichende Syntax         |
| Ressourcenbedarf       | Niedrig bis mittel                   | Mittel                               | Mittel bis höher                     |
| Docker-Unterstützung   | Sehr gut                              | Sehr gut                              | Sehr gut                             |
| Konfigurationsaufwand  | Gering                                | Gering                               | Etwas höher                          |
| Praxistauglichkeit     | Sehr hoch                             | Hoch                                 | Hoch                                 |

Aufgrund der hohen Kompatibilität mit MediaWiki und Nextcloud, des geringen Ressourcenbedarfs sowie der einfachen Integration in Docker-Umgebungen wurde **MariaDB** als Datenbanksystem für dieses Projekt gewählt.

---

### 3.3 Auswahl des Git-Servers

Für die interne Quellcodeverwaltung standen **GitLab** und **Gogs** zur Auswahl.

Aufgrund der begrenzten Ressourcen der virtuellen Maschine wurde **Gogs** gewählt. Gogs bietet:
- eine einfache Weboberfläche
- Multiuser-Unterstützung
- geringe Hardwareanforderungen
- schnelle Start- und Stoppzeiten

Damit erfüllt Gogs alle Anforderungen des Auftraggebers, ohne unnötige Systemressourcen zu beanspruchen.

---

### 3.4 Persistenzkonzept

Um sicherzustellen, dass Daten auch nach einem Neustart der Container erhalten bleiben, wird für alle zustandsbehafteten Dienste eine persistente Datenspeicherung eingesetzt.

Hierfür werden **Docker Volumes** verwendet, da diese:
- einfach zu verwalten sind
- unabhängig vom Container-Lifecycle funktionieren
- eine saubere Trennung zwischen Anwendung und Daten ermöglichen

Alle Datenbanken sowie applikationsspezifischen Datenverzeichnisse werden über Volumes eingebunden.

---

### 3.5 Systemübersicht

Die Infrastruktur besteht aus mehreren Microservices, die über docker-compose miteinander verbunden sind. Die einzelnen Webdienste greifen auf gemeinsame Datenbank-Container zu und werden über definierte Ports nach aussen verfügbar gemacht. Die Verwaltung und Überwachung der Container erfolgt über Portainer.

**Abbildung 3.1 – Systemübersicht der Docker-Infrastruktur**

TODO: Systemübersicht einfügen

---

### 3.6 Zusammenfassung der Entscheidungen

Die getroffenen Entscheidungen ermöglichen eine ressourcenschonende, wartbare und übersichtliche Docker-Infrastruktur. Durch den Einsatz von etablierten Open-Source-Technologien wird eine stabile Basis geschaffen, welche die Anforderungen des Auftraggebers erfüllt und einen realistischen Betrieb ermöglicht.

---

## 4. Realisieren
In dieser Phase wurde die geplante Docker-Infrastruktur umgesetzt. Ziel war es, die definierten Dienste als Microservices bereitzustellen, persistent zu speichern, sicher zu konfigurieren und deren Funktionalität zu überprüfen. Die Umsetzung erfolgte iterativ, wobei Konfigurationsanpassungen anhand von Tests und Fehlermeldungen vorgenommen wurden.

---

### 3.1 Aufbauf der Docker-Infrastruktur
Die Infrastruktur wurde mit Docker und docker-compose realisiert. Jeder Dienst läuft in einem eigenen Container und ist über ein gemeisames Docker-Netzwerk miteinander verbunden.  

---

## 5. Testen

MediaWiki läuft
Gogs läuft
Portainer läuft

---

## 6. Kontrollieren

### 6.7 Umgang mit Zugangsdaten

Um zu verhindern, dass Passwörter im Klartext im Repository abgelegt werden, werden sensible Informationen in einer separaten `.env`-Datei gespeichert. Diese Datei wird über `.gitignore` explizit vom Versionskontrollsystem ausgeschlossen.

Die Verwendung von `.env`-Dateien stellt für dieses Projekt eine einfache, nachvollziehbare und praxistaugliche Lösung dar. Docker Secrets wurden bewusst nicht eingesetzt, da kein Docker-Swarm verwendet wird und der Fokus auf einer verständlichen Umsetzung liegt.


---

## 7. Auswerten

Nextcloud benötigt beim ERSTEN Start mehr Rechte als im laufenden Betrieb.

---

## Anhang