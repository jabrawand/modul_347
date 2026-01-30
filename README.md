# Docker Projekt - Microservices für KMU

Dieses Repository enthält ein Docker-Projekt zur Bereitstellung von Microservices mittels docker-compose.

## Dokumentation
Die Projektdokumentation befindet sich in [documentation.md](./documentation.md)

---

# Installationsanleitung
Diese Anleitung beschreibt die Installation der containerisierten Microservice-Infrastruktur mittels Docker und docker-compose.

---

## Voraussetzungen
Folgende Komponenten müssen auf dem Zielsystem installiert sein:

- Docker Engine 
- Docker Compose Plugin
- Git
- Internetzugang zum Herunterladen der Docker Images
- User ohne Root rechte benutzen

---

## Projekt herunterladen
Das Projekt wird über ein Git-Repository bereitgestellt. Um den Docker zu starten bitte folgende Befehle ausführen:

```bash
git clone https://github.com/jabrawand/modul_347.git
cd modul_347
```
---

## Konfiguration der Login-Variablen
Vor dem Start müssen die benötigten Login-Variablen definiert werden.
Dazu in der CLI im Projektverzeichnis folgenden Befehl ausführen:

```bash
nano .env
```
und folgendes hineinkopieren:

```bash
MEDIAWIKI_DB_NAME=mediawiki
MEDIAWIKI_DB_USER=mediawiki
MEDIAWIKI_DB_PASSWORD=mediawikipass
MEDIAWIKI_DB_ROOT_PASSWORD=rootpass

NEXTCLOUD_DB_NAME=nextcloud
NEXTCLOUD_DB_USER=nextcloud
NEXTCLOUD_DB_PASSWORD=nextcloudpass
NEXTCLOUD_DB_ROOT_PASSWORD=rootpass

GITEA_DB_NAME=gitea
GITEA_DB_USER=gitea
GITEA_DB_PASSWORD=giteapass
```

> [!NOTE]
> Die `.env`-Datei ist aus Sicherheitsgründen im `.gitignore` eingetragen und wird nicht versioniert.
>

> [!IMPORTANT]
> Die in dieser Anleitung verwendeten Login- und Datenbankvariablen dienen ausschliesslich als Beispiel und müssen vor der produktiven Nutzung angepasst werden. Es wird empfohlen, starke und individuelle Passwörter zu verwenden.
>

---
## Container starten
Nach der Konfiguration können alle Dienste mit folgendem Befehl gestartet werden:

```bash
docker compose up -d
```

Docker lädt beim ersten Start automatisch alle benötigten Images herunter und initialisiert die Container.

---
## Status prüfen
Zum prüfen, ob alle Container laufen bitte folgenden Befehl ausführen:

```bash
docker ps
```
Alle Services sollten den Status **Up** haben.

---
## Zugriff auf die Dienste
Nach erfolgreichem Start sind die Dienste über folgende Ports erreichbar:

| Dienst | URL |
| ------ | --- |
| Portainer | http://localhost:9000 |
| Nextcloud | http://localhost:8081 |
| MediaWiki | http://localhost:8085 |
| Gitea | http://localhost:3000 |

Beim ersten Aufruf von Nextcloud und MediaWiki und Gitea erfolgt die webbasierte Ersteinrichtung.

---
## Portainer 
1. Webbrowser öffnen und URL eingeben ( *http://localhost:9000*)
2. Admin-Benutzer erstellen
   1. Benutzername: admin
   2. starkes Passwort eingeben
3. 'Create user'
4. Portainer ist einsatzbereit und zeigt alle laufenden Container an.

---
## Nextcloud 
1. Webbrowser öffnen und URL eingeben (*http://localhost:8081*)
2. Login 
   1. Benutzername: (gemäss `.env`)
   2. Passwort: (gemäss `.env`)
3. 'Logging in' Button wählen
4. Nextcloud ist eingerichtet und betriebsbereit

---
## Gitea
1. Webbrowser öffnen und URL eingeben (*http://localhost:3000*)
2. Folgende Datenbankeinstellungen überprüfen:
   1. Datenbanktyp: **PostgreSQL**
   2. Host: `gitea-db:5432`
   3. Benutzername: (gemäss `.env`)
   4. Passwort: (gemäss `.env`)
   5. Datenbankname: `gitea`
3. Allgemeine Einstellungen prüfen:
   1. Repository-Verzeichnis: `/data/git/repositories`
   2. Git-LSF-Wurzelpfad: `/data/git/lsf`
   3. Ausführen als: `git`
   4. Server-Domain: `localhost`
   5. SSH-Server-Port: `22`
   6. HTTP-Listen-Port: `3000`
   7. Gitea-Basis-URL: `http://localhost:3000/`
   8. Logdateipfad: `/data/gitea/log`
4. Installation abschiessen
5. Einloggen
   1. Benutzername: (gemäss `.env`)
   2. E-Mail: `test@beispiel.com`
   3. Passwort: (gemäss `.env`)
6. Gogs ist nun einsatzbereit

## MediaWiki
1. Webbrowser öffnen und URL eingeben (*http://localhost:8085*)
2. Auf „Set up the wiki“ klicken
3. Sprache auswählen
4. Als Datenbanktyp **MySQL / MariaDB** wählen
5. Datenbankverbindung konfigurieren:
   1. Datenbank-Host: `mediawiki-db`
   2. Datenbankname: `mediawiki`
   3. Benutzername: (gemäss `.env`)
   4. Passwort: (gemäss `.env`)
6. Wiki-Name und Administrator-Konto festlegen
7. Installation abschliessen

> [!IMPORTANT]
> Nach dem Abschluss wird von MediaWiki ein `LocalSettings.php` erzeugt, welches zwingend auf der selben Ebene wie das `docker-compose.yml` abgelegt werden muss.
Im docker-compose muss die auskommentierte Zeile 28 aktiviert werden. 
>

Der `mediawiki`-Container muss nun mit folgendem Befehl neu gestartet werden:

```bash
docker compose restart mediawiki
```
Danach erneut Webbrowser öffnen und URL eingeben (*http://localhost:8085*)

---
Alle Konfigurationsdaten und Inhalte werden in Docker Volumes gespeichert. Die Ersteinrichtung muss nur einmal durchgeführt werden und bleibt auch nach einem Container Neustart erhalten.

---
## Abschluss
Nach der Ersteinrichtung stehen folgende Dienste zur Verfügung:
- Portainer zur Container-Verwaltung
- MediaWiki als internes Wiki
- Nextcloud für Filesharing und Kollaboration
- Gogs als interner Git-Server