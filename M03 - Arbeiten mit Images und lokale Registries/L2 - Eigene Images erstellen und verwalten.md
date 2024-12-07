
# Eigene Images erstellen und verwalten

## Aufbau und Verwaltung eigener Images (Dockerfile, Best Practices)

Ein Docker-Image ist eine schreibgeschützte Vorlage, die alle benötigten Dateien, Abhängigkeiten und Anweisungen enthält, um eine Anwendung in einem Container auszuführen.

### Erstellung eines Docker-Images mit einem Dockerfile

Das Dockerfile ist eine Textdatei, die die Befehle enthält, um ein Docker-Image zu erstellen.

#### Beispiel-Dockerfile:
```dockerfile
# Ausgangspunkt: Basis-Image
FROM python:3.9-slim

# Setzen des Arbeitsverzeichnisses
WORKDIR /app

# Kopieren von Anforderungen und Installation
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Kopieren der Anwendungsdateien
COPY . .

# Definieren des Standard-Befehls
CMD ["python", "app.py"]
```

### Erstellen eines Images mit `docker build`
Der `docker build`-Befehl wird verwendet, um ein Docker-Image basierend auf einem Dockerfile zu erstellen.
```bash
docker build -t mein-image:1.0 .
```

### Best Practices für Dockerfiles
1. **Minimalistisches Basis-Image verwenden**:
   Verwenden Sie kleinere Basis-Images (z. B. `alpine`), um die Größe des endgültigen Images zu reduzieren.
   ```dockerfile
   FROM alpine:latest
   ```

2. **Mehrstufiges Bauen**:
   Vermeiden Sie unnötige Dateien im endgültigen Image, indem Sie mehrstufige Builds verwenden.
   ```dockerfile
   FROM golang:1.17 AS builder
   WORKDIR /app
   COPY . .
   RUN go build -o main .

   FROM alpine:latest
   COPY --from=builder /app/main /main
   CMD ["/main"]
   ```

3. **Caching effektiv nutzen**:
   Platzieren Sie häufig unveränderte Anweisungen wie das Installieren von Abhängigkeiten am Anfang des Dockerfiles.

4. **Sicherheitsmaßnahmen**:
   - Entfernen Sie sensible Daten und temporäre Dateien.
   - Aktualisieren Sie das Basis-Image regelmäßig.

5. **Verwendung von `.dockerignore`**:
   Vermeiden Sie das Hinzufügen unnötiger Dateien ins Image.
   ```plaintext
   node_modules
   .git
   *.log
   ```

## Images versionieren, sichern und verwalten

Die Verwaltung von Docker-Images ist ein zentraler Bestandteil des Workflows, insbesondere wenn mehrere Versionen einer Anwendung bereitgestellt werden sollen.

### Versionierung von Images
Die Versionierung erfolgt durch Tags. Tags helfen dabei, verschiedene Versionen eines Images zu unterscheiden.
```bash
docker build -t mein-image:1.0 .
docker build -t mein-image:latest .
```

- Verwenden Sie semantische Versionierung (`1.0`, `1.1`, `latest`).
- Der `latest`-Tag sollte die stabilste Version widerspiegeln.

### Sichern von Images
Docker-Images können in Repositories gespeichert und gesichert werden, beispielsweise in:
- **Docker Hub** (öffentlich und privat):
  ```bash
  docker tag mein-image:1.0 mein-dockerhub/mein-image:1.0
  docker push mein-dockerhub/mein-image:1.0
  ```
- **Private Registries**:
  Eigene Registries wie Harbor oder AWS ECR können genutzt werden.

### Verwaltung von Images
#### Auflisten lokaler Images:
```bash
docker images
```

#### Entfernen nicht benötigter Images:
```bash
docker rmi image_id
```

#### Aufräumen von nicht verwendeten Ressourcen:
```bash
docker system prune
```

### Automatisierung der Verwaltung
Mit CI/CD-Tools wie Jenkins oder GitLab CI können Sie den Build-, Test- und Push-Prozess für Images automatisieren.

Beispiel für GitLab CI/CD:
```yaml
stages:
  - build
  - push

build-job:
  stage: build
  script:
    - docker build -t mein-image:${CI_COMMIT_SHA} .
    - docker tag mein-image:${CI_COMMIT_SHA} mein-image:latest

push-job:
  stage: push
  script:
    - docker push mein-image:${CI_COMMIT_SHA}
    - docker push mein-image:latest
```