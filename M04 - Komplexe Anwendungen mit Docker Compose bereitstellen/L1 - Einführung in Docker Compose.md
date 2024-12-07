
# Einführung in Docker Compose
## Grundlagen von Docker Compose und YAML-Konfigurationen

### Was ist Docker Compose?
Docker Compose ist ein Werkzeug, mit dem Sie Multi-Container-Docker-Anwendungen definieren und verwalten können. Statt mehrere Container manuell zu starten und zu verbinden, können Sie eine Konfigurationsdatei (`docker-compose.yml`) verwenden, um alle Aspekte Ihrer Anwendung zu definieren.

### Vorteile von Docker Compose
- **Einfachheit**: Starten, stoppen und verwalten Sie Multi-Container-Anwendungen mit einem einzigen Befehl.
- **Wiederholbarkeit**: Definieren Sie die gesamte Anwendung in einer zentralen Datei.
- **Flexibilität**: Unterstützt die Definition von Netzwerken, Volumes und spezifischen Umgebungen für Container.

### YAML-Konfigurationen
Die Konfigurationsdatei für Docker Compose verwendet YAML (YAML Ain't Markup Language). YAML ist ein menschenlesbares Datenserialisierungsformat, das sich durch seine einfache Struktur auszeichnet.

Grundlegende Syntaxregeln:
- Einrückungen: Verwenden Sie zwei Leerzeichen (keine Tabs).
- Schlüssel-Wert-Paare: Definieren Sie Konfigurationsoptionen als Schlüssel-Wert-Paare.
- Listen: Verwenden Sie Bindestriche (`-`), um Listen zu erstellen.

Beispiel für YAML:
```yaml
version: '3.8'
services:
  app:
    image: python:3.9
    volumes:
      - ./app:/usr/src/app
    ports:
      - "8000:8000"
```

## Aufbau einer Docker-Compose-Datei

Eine Docker-Compose-Datei definiert Services, Netzwerke und Volumes für eine Anwendung. Die Struktur ist modular und einfach zu erweitern.

### Bestandteile einer Docker-Compose-Datei

#### 1. `version`
Gibt die Version des Docker-Compose-Dateiformats an. Neuere Versionen (z. B. `3.8`) bieten zusätzliche Funktionen und bessere Kompatibilität.

```yaml
version: '3.8'
```

#### 2. `services`
Hier definieren Sie die Container, die Ihre Anwendung benötigt. Jeder Service repräsentiert einen Container.

Beispiel:
```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

#### 3. `volumes`
Volumes dienen zur persistenten Speicherung von Daten, die über Neustarts hinaus erhalten bleiben sollen.

Beispiel:
```yaml
volumes:
  data-volume:
    driver: local
```

#### 4. `networks`
Netzwerke definieren, wie Container miteinander kommunizieren.

Beispiel:
```yaml
networks:
  frontend:
  backend:
```

### Beispiel einer vollständigen `docker-compose.yml`

Erstellen Sie eine Multi-Container-Anwendung mit einem Webserver und einer Datenbank:
```yaml
version: '3.8'

services:
  web:
    image: nginx
    ports:
      - "8080:80"
    networks:
      - frontend

  db:
    image: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: exampledb
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend

volumes:
  db-data:

networks:
  frontend:
  backend:
```

### Wichtige Docker Compose-Befehle

1. **Starten von Containern**:
   ```bash
   docker-compose up
   ```
   Verwenden Sie den `-d`-Parameter, um die Container im Hintergrund auszuführen:
   ```bash
   docker-compose up -d
   ```

2. **Stoppen von Containern**:
   ```bash
   docker-compose down
   ```

3. **Logs anzeigen**:
   ```bash
   docker-compose logs
   ```

4. **Neuaufbau von Containern**:
   ```bash
   docker-compose up --build
   ```

5. **Bestimmten Service starten**:
   ```bash
   docker-compose up web
   ```