
**Modul**: 347
**Projekt:** Microservice-Infrastruktur für ein Informatik-KMU
**Autor:** Jasmin Brawand
**Klasse:** UIFZ-2425-005
**Datum:** 28.01.2026

---
## Inhaltsverzeichnis

1. Einleitung
2. Informieren
3. Planen
4. Entscheiden
5. Realisieren
6. Testkonzept und Testprotokoll
7. Sicherheitskonzept
8. Auswertung / Reflexion
---
# 1. Einleitung

Im Rahmen dieses Projektes wurde für ein Informatik-KMU eine containerisierte Serverlösung mit mehreren Diensten realisiert. Ziel war es, mithilfe von Docker und docker-compose einen Microservice-basierten Applikationsstack aufzubauen, welcher firmeninterne Services wie Filesharing, Versionsverwaltung und ein Wiki bereitstellt.

---

# 2. Informieren

## 2.1 Ausgangslage

Das KMU benötigt mehrere interne Dienste, welche zentral, zuverlässig und wartbar betrieben werden sollen. Um eine flexible und skalierbare Lösung zu ermöglichen, sollen diese Dienste containerisiert umgesetzt werden.

---
## 2.2 Analyse der Anforderungen
Folgende Anforderungen wurden vom Auftraggeber definiert:

- Bereitstellung eines internen MediaWiki
- Einsatz von Nextcloud für Filesharing und Kollaboration
- Interne Versionsverwaltung mittels GitLab oder Gogs
- Persistente Speicherung aller Daten
- Überwachung der Container mittels Portainer
- Versionierung aller Konfigurationsdateien und der Dokumentation in Git

**Tabelle 1.1 - Funktionale Anforderungen in Tabellarischer Form**

| Dienst     | Beschreibung                | Port |
| ---------- | --------------------------- | ---- |
| MediaWiki  | Internes Firmen-Wiki        | 8085 |
| Nextcloud  | Filesharing & Kollaboration | 8081 |
| Git-Server | GitLab                      |      |
| Portainer  | Monitoring                  | 9000 |

---
## 2.3 Randbedingungen und Einschränkungen

- Betrieb auf einer virtuellen Maschine
- Begrenzte Ressourcen (RAM & Speicherplatz)
- Einsatz von Docker und docker-compose ist zwingend
- Alle Dienste müssen nach einem Neustart weiterhin funktionsfähig sein
- Die Lösung muss verständlich dokumentiert sein

---
## 2.4 Technische Vorabklärungen
Mögliche Technologien:

- Docker und docker-compose (gemäss Anforderungen vorgegeben)
- MariaDB, MySQL oder PostgreSQL als Datenbank für MediaWiki und Nextcloud
- Docker Volumes oder Bind Mounts
- Portainer zur grafischen Verwaltung und Überwachung der Container (gemäss Anforderungen vorgegeben)

---
## 2.5 Zielsetzung des Projekts
Das Ziel ist der Aufbau einer funktionsfähigen, dokumentierten und getesteten Docker-Infrastruktur, welche alle Anforderungen erfüllt und einen realitätsnahen Betrieb simuliert.

---
# 3. Planen 

## 3.1 Vorgehensmodell
Die Umsetzung erfolgt nach dem IPERKA-Modell:
- Informieren
- Planen
- Entscheiden
- Realisieren
- Kontrollieren
- Auswerten

---
## 3.2Arbeitsplanung 
Das Projekt wurde als Einzelarbeit durchgeführt.

**Informieren**
- Anforderungen und Randbedingungen analysieren
- Informationen über Images auf Docker Hub holen
- 

**Tabelle 3.1 – Arbeitsplanung nach IPERKA-Phasen**

| Phase         | Aufgabe                                       |
| ------------- | --------------------------------------------- |
| Informieren   | Anforderungen und Randbedingungen analysieren |
| Planen        | Erstellung Zeitplan und Ressourcenplanung     |
| Entscheiden   | Technologien und Architektur auswählen        |
| Realisieren   | Aufbau Docker-Container und Konfiguration     |
| Testen        | Testfälle erstellen und durchführen           |
| Kontrollieren | Ergebnisse prüfen, Abweichungen analysieren   |
| Auswerten     | Dokumentation abschliessen, Reflektion        |


---

## 3.3 Testplanung
Für jeden Hauptdienst wurden Funktionstests definiert (siehe Kapitel 6)

---
# 4. Entscheiden

## 4.1 Auswahl Datenbank

**Tabelle 4.1 – Vergleich MairaDB & MySQL**


| Kriterium               | MariaDB                      | MySQL                 | PostgreSQL                   |
| ----------------------- | ---------------------------- | --------------------- | ---------------------------- |
| Unterstützung MediaWiki | Offiziell unterstützt        | Offiziell unterstützt | Unterstützt                  |
| Unterstützung Nextcloud | Offiziell unterstützt        | Offiziell unterstützt | Offiziell unterstützt        |
| Kompatibilität          | Sehr hoch (MySQL-kompatibel) | Hoch                  | Gut, aber abweichende Syntax |
| Ressourcenbedarf        | Niedrig bis mittel           | Mittel                | Mittel bis höher             |
| Docker-Unterstützung    | Sehr gut                     | Sehr gut              | Sehr gut                     |
| Konfigurationsaufwand   | Gering                       | Gering                | Etwas höher                  |
| Praxistauglichkeit      | Sehr hoch                    | Hoch                  | Hoch                         |

**Entscheidung**: MariaDB
**Begrünung**: Open Source, gute Performance, hohe Kompatibilität zu Nextcloud und MediaWiki.

---
## 4.2 Auswahl Git-Server

**Tabelle 4.2– Vergleich Gitea & Gogs**

**Entscheidung**: Gogs
**Begrünung**: Ressourcenarm und für KMU ausreichend.

---

# 5. Realisieren
## 5.1 Docker-Infrastruktur
Die Infrastruktur wurde mit docker-compose realisiert. Jeder Dienst läuft in einem eigenen Container und ist über ein gemeinsames Netzwerk verbunden.

---

## 5.2 Persistenz
Alle Dienste verwenden Docker Volumes zur persistenten Datenspeicherung.

---

## 5.3 Ressourcenbeschränkungen

- Datenbank-Container: max. 700 MB RAM
- Web-Container: max 300 MB RAM
- Portainer: max. 150 MB RAM

---

## 5.4 Sicherheit

- Passwörter sind im `.env`-file abgelegt
- Reduzierte Container-Rechte
- Trennung der Dienste

---

## 5.5 Probleme & Lösungen
Bei Nextcloud traten Probleme mit Schreibrechten und Security-Optionen auf. Diese wurden durch temporäre Anpassungen während der Initialisierung behoben.

N: nicht erreichbar über 8081


---

# 6. Testen
Die Tests wurden in einer kontrollierten Umgebung durchgeführt, welche dem geplanten Zielsystem möglichst nahekommt. Ziel war es, die Funktionalität, Stabilität und Persistenz der Docker-basierten Dienste zu überprüfen.

## 6.1 Testumgebung
- Virtuelle Maschine
- Docker Engine
- lokaler Browserzugriff

---
## 6.2 Testkonzept
Das Testkonzept definiert welche Komponenten getestet werden und welche Kriterien für einen erfolgreichen Test erfüllt sein müssen. 

**Testzeil**: 
- Sicherstellung der Erreichbarkeit aller Dienste
- Überprüfung der Grundfunktionen (Login, Nutzung, Datenspeicherung)
- Kontrolle der Datenpersistenz nach Neustart

---
## 6.3 Testfälle

**Tabelle 6.1– Testfälle Portainer**

| Testfall | Beschreibung                | Erwartetes Ergebnis                               |
| -------- | --------------------------- | ------------------------------------------------- |
| T1       | Aufrufen der Weboberfläche  | Protainer ist über Port 9000 erreichbar           |
| T2       | Login mit Admin-User        | erfolgreiche Anmeldung                            |
| T3       | Containerübersicht anzeigen | Alle laufenden Container werden korrekt angezeigt |
| T4       | Neustart                    | Container kann erfolgreich neugestartet werden    |

**Tabelle 6.2– Testfälle Mediawiki**

| Testffall | Beschreibung              | Erwartetes Ergebnis                    |
| --------- | ------------------------- | -------------------------------------- |
| T1        | Aufruf MediaWiki          | MediaWiki ist auf Port 8085 erreichbar |
| T2        | Neue Wiki-Seite erstellen | Seite wird gespeichert und angezeigt   |
| T3        | Neustart Container        | Inhalte bleiben erhalten               |

**Tabelle 6.2– Testfälle Nextcloud**

| Testffall | Beschreibung       | Erwartetes Ergebnis                    |
| --------- | ------------------ | -------------------------------------- |
| T1        | Aufruf Nextcloud   | Nextcloud ist auf Port 8081 erreichbar |
| T2        | Anmelden           | Erfolgreiche Anmeldung                 |
| T3        | Datei hochladen    | Datei kann hochgeladen werden          |
| T4        | Neustart Container | Inhalte bleiben erhalten               |

**Tabelle 6.2– Testfälle Gogs**

| Testffall | Beschreibung         | Erwartetes Ergebnis                  |
| --------- | -------------------- | ------------------------------------ |
| T1        | Aufruf Gogs          | Gogs ist auf Port 3000 erreichbar    |
| T2        | Anmelden             | Erfolgreiche Anmeldung               |
| T3        | Repository erstellen | Repository wird erfolgreich angelegt |
| T4        | Neustart             | Inhalte bleiben erhalten             |

---
## 6.4 Testprotokoll

---
# 7. Sicherheitskonzept

## 7.1 Ziele

- Vertraulichkeit
- Integrität
- Verfügbarkeit

---
## 7.2 Container-spezifische Risiken

- Container Breakout
- Unsichere Secrets
- Fehlende Ressourcenlimits

---
## 7.3 Massnahmen

- Kein Root-Zugriff
- `.env` im `.gitignore`
- Ressourcenkontrollen
- Minimale Capabilities

---
# 8. Auswertung / Reflexion

Durch dieses Projekt konnte ich praktische Erfahrung im Aufbau einer Microservice-Infrastruktur mit Docker sammeln. Besonders lehrreich waren die Fehlersuche bei Nextcloud, der Umgang mit Persistenz und das Thema Sicherheit in containerisierten Umgebungen.

Ich habe gelernt, dass Security-Massnahmen sorgfältig geplant werden müssen und dass eine saubere Dokumentation essenziell für Wartung und Nachvollziehbarkeit ist.