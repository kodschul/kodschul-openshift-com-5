
# Grundlagen der Docker-Sicherheit
## Warum ist Docker-Sicherheit wichtig?

Container bieten eine isolierte Umgebung für Anwendungen, aber sie sind nicht immun gegen Sicherheitsrisiken. Sicherheitslücken können dazu führen, dass:
- Unautorisierter Zugriff auf Container oder Host-Systeme erfolgt.
- Sensible Daten kompromittiert werden.
- Container als Angriffsvektoren genutzt werden.

Docker-Sicherheit zielt darauf ab, solche Risiken zu minimieren und robuste Containerumgebungen zu gewährleisten.

## Grundlagen der Docker-Sicherheit

### 1. Minimale Basis-Images verwenden
- **Warum?**: Basis-Images sind die Grundlage jedes Containers. Je kleiner das Basis-Image, desto geringer ist die Angriffsfläche.
- **Empfehlung**: Verwenden Sie offiziell unterstützte und minimalistische Images wie `alpine` oder `debian-slim`.

Beispiel:
```dockerfile
FROM alpine:latest
```

### 2. Least Privilege-Prinzip anwenden
- Führen Sie Container mit minimalen Rechten aus. Standardmäßig laufen Docker-Container als `root`, was ein Sicherheitsrisiko darstellt.
- **Lösung**: Definieren Sie einen nicht-privilegierten Benutzer im Dockerfile.

Beispiel:
```dockerfile
RUN addgroup --system appgroup && adduser --system appuser --ingroup appgroup
USER appuser
```

### 3. Secrets sicher verwalten
- Verwenden Sie keine Passwörter, Tokens oder andere sensible Daten direkt im Dockerfile.
- Nutzen Sie Docker Secrets oder Umgebungsvariablen, um vertrauliche Informationen zu verwalten.

Beispiel für Docker Secrets:
1. Secret erstellen:
   ```bash
   echo "mein-geheimes-token" | docker secret create mein_token -
   ```
2. Secret in einem Service verwenden:
   ```bash
   docker service create --name meine_app --secret mein_token mein_image
   ```

### 4. Ressourcenlimits setzen
- Setzen Sie Limits für CPU- und Speicherressourcen, um die Auswirkungen potenzieller Angriffe zu begrenzen.

Beispiel:
```bash
docker run --memory=512m --cpus=1 my_image
```

### 5. Sicherheitsrichtlinien für Images durchsetzen
- **Scanning von Images**: Nutzen Sie Tools wie `Trivy` oder `Docker Scout`, um Images auf bekannte Sicherheitslücken zu überprüfen.
- **Signieren von Images**: Verwenden Sie Docker Content Trust (DCT), um die Authentizität und Integrität von Images zu gewährleisten.

Beispiel für das Aktivieren von DCT:
```bash
export DOCKER_CONTENT_TRUST=1
```

### 6. Netzwerkisolierung sicherstellen
- Erstellen Sie benutzerdefinierte Docker-Netzwerke, um den Zugriff zwischen Containern zu kontrollieren.
- Nutzen Sie Firewalls oder Sicherheitsgruppen, um den Zugriff auf Container von außen zu beschränken.

Beispiel:
```bash
docker network create --driver bridge mein_sicheres_netzwerk
docker run --network mein_sicheres_netzwerk my_image
```

### 7. Container-Laufzeit überwachen
- Überwachen Sie Container während der Laufzeit mit Tools wie `Falco` oder `sysdig`, um verdächtige Aktivitäten zu erkennen.

### 8. Aktualisierung und Patch-Management
- Halten Sie Docker, die Basis-Images und die Container-Images regelmäßig auf dem neuesten Stand.

Beispiel:
```bash
docker pull my_image:latest
docker stop mein_container
docker rm mein_container
docker run my_image:latest
```