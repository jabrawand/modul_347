# Docker Projekt - Microservices für KMU

Dieses Repository enthält ein Docker-Projekt zur Bereitstellung von Microservices mittels docker-compose.

## Dokumentation
Die Projektdokumentation befindet sich in [documentation.md](./documentation.md)

---

## Installationsanleitung
Diese Anleitung beschreibt die Installation der containerisierten Microservice-Infrastruktur mittels Docker und docker-compose.

---

### Voraussetzungen
Folgende Komponenten müssen auf dem Zielsystem installiert sein:

- Docker Engine 
- Docker Compose Plugin
- Git
- Internetzugang zum Herunterladen der Docker Images

---

### Projekt herunterladen
Das Projekt wird über ein Git-Repository bereitgestellt. Um den Docker zu starten bitte folgende Befehle ausführen:

```bash
git clone https://github.com/jabrawand/modul_347.git
cd modul_347
```
---

### Konfiguration der Login-Variablen
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

GOGS_DB_NAME=gogs
GOGS_DB_USER=gogs
GOGS_DB_PASSWORD= gogspass
GOGS_DB_ROOT_PASSWORD=rootpass
```
