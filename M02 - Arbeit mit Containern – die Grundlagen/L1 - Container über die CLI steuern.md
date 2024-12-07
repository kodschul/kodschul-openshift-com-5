
# Container über die CLI steuern
## Einführung ins Command Line Interface (CLI) für Docker

Die Docker-CLI ist ein leistungsstarkes Werkzeug, mit dem Sie Docker-Container und andere Docker-Ressourcen direkt von der Kommandozeile aus verwalten können.

### Vorteile der CLI-Nutzung:
- **Flexibilität**: Direkte Steuerung aller Docker-Operationen.
- **Automatisierung**: Einfaches Skripting und Integration in CI/CD-Pipelines.
- **Effizienz**: Schnellere Verwaltung im Vergleich zu GUI-basierten Lösungen.

### Grundlegender Aufbau von Docker-Befehlen
Docker-Befehle folgen der Struktur:
```bash
docker <befehl> [optionen] [argumente]
```

Beispiele:
- `docker ps`: Zeigt laufende Container an.
- `docker run`: Startet einen neuen Container.
- `docker stop`: Stoppt einen laufenden Container.

## Wichtige Docker-Befehle zum Starten, Stoppen und Löschen von Containern

### 1. Container starten
Der Befehl `docker run` wird verwendet, um einen neuen Container zu starten.
#### Syntax:
```bash
docker run [optionen] <image>
```
#### Beispiel:
```bash
docker run -d --name mein_container nginx
```
- **`-d`**: Startet den Container im Hintergrund (detached mode).
- **`--name`**: Weist dem Container einen benutzerdefinierten Namen zu.
- **`nginx`**: Das Docker-Image, das für den Container verwendet wird.

### 2. Container anzeigen
Verwenden Sie den Befehl `docker ps`, um eine Liste der laufenden Container anzuzeigen.
#### Beispiel:
```bash
docker ps
```
- Zeigt Container-ID, Namen, Status und mehr.

Um alle Container (auch gestoppte) anzuzeigen:
```bash
docker ps -a
```

### 3. Container stoppen
Mit dem Befehl `docker stop` können Sie einen laufenden Container anhalten.
#### Syntax:
```bash
docker stop <container_id|container_name>
```
#### Beispiel:
```bash
docker stop mein_container
```

### 4. Container löschen
Gestoppte Container können mit `docker rm` entfernt werden.
#### Syntax:
```bash
docker rm <container_id|container_name>
```
#### Beispiel:
```bash
docker rm mein_container
```

Um mehrere Container auf einmal zu löschen:
```bash
docker rm $(docker ps -a -q)
```
- **`$(docker ps -a -q)`**: Gibt die IDs aller Container zurück.

### 5. Weitere nützliche Befehle
- **Logs anzeigen**:
  ```bash
  docker logs <container_id|container_name>
  ```
  Beispiel:
  ```bash
  docker logs mein_container
  ```

- **Container starten und stoppen**:
  - Starten eines gestoppten Containers:
    ```bash
    docker start mein_container
    ```
  - Neustarten eines Containers:
    ```bash
    docker restart mein_container
    ```

- **Container-Details anzeigen**:
  ```bash
  docker inspect <container_id|container_name>
  ```
  Beispiel:
  ```bash
  docker inspect mein_container
  ```

## Praktische Übung

### Aufgabe: Einen Nginx-Webserver mit Docker-CLI starten
1. Starten Sie einen neuen Container mit dem Namen `nginx_webserver`:
   ```bash
   docker run -d --name nginx_webserver -p 8080:80 nginx
   ```
   - Der Container wird im Hintergrund ausgeführt und der Webserver ist auf Port 8080 erreichbar.

2. Überprüfen Sie, ob der Container läuft:
   ```bash
   docker ps
   ```

3. Zeigen Sie die Logs des Containers an:
   ```bash
   docker logs nginx_webserver
   ```

4. Stoppen Sie den Container:
   ```bash
   docker stop nginx_webserver
   ```

5. Löschen Sie den Container:
   ```bash
   docker rm nginx_webserver
   ```