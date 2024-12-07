
# Multi-Container-Anwendungen erstellen
## Container-basierte Anwendungen mit mehreren Services erstellen

Docker ermöglicht es, Anwendungen aus mehreren Containern zu erstellen, die verschiedene Services bereitstellen. Zum Beispiel kann eine typische Webanwendung folgende Services enthalten:
- **Webserver** (z. B. Nginx)
- **Anwendungsserver** (z. B. Python, Node.js)
- **Datenbank** (z. B. MySQL, PostgreSQL)

### Beispiel: Eine einfache Anwendung mit mehreren Containern
1. **Erstellen Sie die benötigten Dateien**:
   - **Dockerfile** für den Anwendungsserver:
     ```dockerfile
     FROM python:3.9
     WORKDIR /app
     COPY requirements.txt .
     RUN pip install -r requirements.txt
     COPY . .
     CMD ["python", "app.py"]
     ```

   - **requirements.txt**:
     ```
     flask
     redis
     ```

   - **app.py**:
     ```python
     from flask import Flask
     import redis

     app = Flask(__name__)
     cache = redis.StrictRedis(host='redis', port=6379)

     @app.route('/')
     def hello():
         count = cache.incr('hits')
         return f'Hello, Docker! I have been seen {count} times.'

     if __name__ == '__main__':
         app.run(host='0.0.0.0')
     ```

2. **Erstellen und starten Sie Container mit Docker Compose**:
   - **docker-compose.yml**:
     ```yaml
     version: '3.8'
     services:
       web:
         build: .
         ports:
           - "5000:5000"
         depends_on:
           - redis
       redis:
         image: "redis:alpine"
     ```

3. **Docker Compose-Befehle verwenden**:
   - Starten Sie die Anwendung:
     ```bash
     docker-compose up
     ```
   - Stoppen Sie die Anwendung:
     ```bash
     docker-compose down
     ```

Die Anwendung besteht aus zwei Services: einem Webserver und einem Redis-Server. Sie können über `http://localhost:5000` darauf zugreifen.

## Microservices-Architektur mit Docker Compose verwalten

Docker Compose ist ein leistungsstarkes Werkzeug zur Orchestrierung von Multi-Container-Anwendungen. Es ermöglicht die Verwaltung von Microservices und deren Konfiguration in einer zentralen Datei.

### Vorteile von Docker Compose
- **Einfache Konfiguration**: Alle Services werden in einer einzigen YAML-Datei beschrieben.
- **Flexibilität**: Unterstützung für mehrere Umgebungen (Entwicklung, Produktion).
- **Effizienz**: Automatisches Netzwerk-Setup und Abhängigkeitshandling zwischen Containern.

### Beispiel für eine Microservices-Anwendung
Nehmen wir eine Anwendung mit folgenden Services:
1. **Frontend**: React (läuft auf Node.js).
2. **Backend**: Flask.
3. **Datenbank**: PostgreSQL.

**docker-compose.yml**:
```yaml
version: '3.8'
services:
  frontend:
    image: node:14
    working_dir: /app
    volumes:
      - ./frontend:/app
    ports:
      - "3000:3000"
    command: ["npm", "start"]

  backend:
    build:
      context: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

### Best Practices für Docker Compose in einer Microservices-Architektur
1. **Umgebungsvariablen verwenden**:
   - Definieren Sie sensible Daten wie Passwörter in `.env`-Dateien.
   ```env
   POSTGRES_USER=user
   POSTGRES_PASSWORD=password
   POSTGRES_DB=mydatabase
   ```
   - Verweisen Sie in `docker-compose.yml` darauf:
     ```yaml
     environment:
       POSTGRES_USER: ${POSTGRES_USER}
       POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
     ```

2. **Separate Netzwerke für Sicherheit**:
   - Erstellen Sie dedizierte Netzwerke für verschiedene Services.
     ```yaml
     networks:
       frontend_net:
       backend_net:
     ```

3. **Healthchecks einrichten**:
   - Fügen Sie Healthchecks hinzu, um sicherzustellen, dass Services ordnungsgemäß laufen.
     ```yaml
     healthcheck:
       test: ["CMD", "curl", "-f", "http://localhost:5000"]
       interval: 30s
       retries: 3
     ```
