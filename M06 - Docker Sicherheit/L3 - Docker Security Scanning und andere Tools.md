
# Docker Security Scanning und andere Tools
## Docker Security Scanning

Docker Security Scanning ist ein Tool, das Docker-Images auf bekannte Schwachstellen überprüft. Es analysiert die im Image enthaltenen Pakete und vergleicht diese mit einer Datenbank bekannter Sicherheitslücken (CVE).

### Vorteile:
- **Sicherheitsbewertung**: Identifiziert Schwachstellen in Abhängigkeiten und Basis-Images.
- **Automatisierte Überprüfung**: Integriert sich nahtlos in CI/CD-Pipelines.
- **Compliance**: Unterstützt Unternehmen bei der Einhaltung von Sicherheitsstandards.

### Verwendung:
Docker Security Scanning ist in Docker Hub und Docker Desktop integriert und kann auch mit Drittanbieter-Tools wie `Trivy` oder `Anchore` genutzt werden.

#### Beispiel mit Docker CLI:
1. Aktivieren des Docker Content Trust (DCT):
   ```bash
   export DOCKER_CONTENT_TRUST=1
   ```
2. Scannen eines Docker-Images:
   ```bash
   docker scan my-image:latest
   ```

#### Beispiel mit Trivy:
Trivy ist ein leichtgewichtiges Open-Source-Tool zur Sicherheitsanalyse von Docker-Images.
1. Installation:
   ```bash
   brew install aquasecurity/trivy/trivy
   ```
2. Scannen eines Docker-Images:
   ```bash
   trivy image my-image:latest
   ```

### Wichtige Sicherheitsaspekte:
- Scannen Sie Images regelmäßig.
- Verwenden Sie schlanke Basis-Images (z. B. `alpine`), um die Angriffsfläche zu reduzieren.
- Halten Sie Images aktuell.

## Andere Tools zur Sicherheitsüberprüfung

Neben Docker Security Scanning gibt es eine Vielzahl anderer Tools, die zur Verbesserung der Sicherheit in Docker-Umgebungen beitragen können.

### 1. **Clair**
Clair ist ein Open-Source-Tool, das Docker-Images auf Schwachstellen überprüft und eine REST-API für die Integration in andere Systeme bereitstellt.
- **Installation**: Clair erfordert ein eigenes Backend (z. B. PostgreSQL).
- **Verwendung**:
   1. Starten Sie den Clair-Server.
   2. Übertragen Sie das Docker-Image mit einem Tool wie `docker2aci` in das Clair-Format.
   3. Analysieren Sie das Image mit Clair.

### 2. **Anchore**
Anchore ist ein weiteres Sicherheitsanalyse-Tool, das detaillierte Berichte zu Schwachstellen und Compliance für Docker-Images erstellt.
- **Vorteile**: Integration in CI/CD-Pipelines, benutzerdefinierte Richtlinien.
- **Verwendung**:
   1. Installieren Sie Anchore CLI:
      ```bash
      pip install anchorecli
      ```
   2. Analysieren Sie ein Docker-Image:
      ```bash
      anchore-cli image add my-image:latest
      anchore-cli image vuln my-image:latest all
      ```

### 3. **Docker Bench for Security**
Docker Bench ist ein Open-Source-Skript, das die Sicherheit von Docker-Hosts und Containern überprüft und Sicherheitsrichtlinien (CIS Benchmarks) anwendet.
- **Verwendung**:
   ```bash
   docker run --rm -it --net host --pid host    --cap-add audit_control    -v /var/run/docker.sock:/var/run/docker.sock    -v /etc:/etc:ro -v /usr/bin/docker:/usr/bin/docker:ro    docker/docker-bench-security
   ```

### 4. **Sysdig Secure**
Sysdig bietet Sicherheitsüberwachung und -analyse für Container und Kubernetes-Umgebungen.
- Echtzeit-Überwachung von Container-Aktivitäten.
- Erkennung von Anomalien und Sicherheitsverletzungen.

## Best Practices für Docker-Sicherheit

1. **Vertrauenswürdige Images verwenden**:
   - Verwenden Sie ausschließlich Images aus vertrauenswürdigen Quellen.
   - Validieren Sie die Signaturen der Images.

2. **Minimale Berechtigungen**:
   - Führen Sie Container mit den minimal notwendigen Berechtigungen aus.
   - Verwenden Sie den `--read-only`-Modus, um Schreiboperationen im Container zu verhindern.

3. **Regelmäßiges Scannen**:
   - Integrieren Sie Sicherheits-Scanning in Ihre CI/CD-Pipelines.

4. **Netzwerksicherheit**:
   - Begrenzen Sie den Zugriff auf Container-Netzwerke.
   - Verwenden Sie Firewalls und Netzwerksegmentierung.

5. **Aktuelle Software**:
   - Halten Sie sowohl Docker selbst als auch Ihre Images auf dem neuesten Stand.
