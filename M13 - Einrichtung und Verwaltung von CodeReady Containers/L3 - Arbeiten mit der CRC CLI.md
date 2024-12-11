
#  Arbeiten mit der CRC CLI
## Was ist die CRC CLI?

Die CRC CLI (CodeReady Containers Command Line Interface) ist ein Werkzeug, das es Entwicklern ermöglicht, eine lokale OpenShift-Umgebung zu starten, zu verwalten und zu überwachen. Es wird oft für Entwicklungs- und Testzwecke verwendet.

### Vorteile der CRC CLI:
- Lokale OpenShift-Umgebung für einfache Tests.
- Schnelles Setup ohne große Infrastruktur.
- Zugriff auf alle OpenShift-Funktionen in einer kompakten Umgebung.

---

## Grundlegende Befehle: Start, Stop, Status, Logs

Die grundlegenden Befehle der CRC CLI ermöglichen es Ihnen, Ihre lokale OpenShift-Umgebung zu starten, zu stoppen und den aktuellen Status zu überprüfen.

### 1. Starten der CRC-Umgebung
```bash
crc start
```
- Startet die lokale OpenShift-Umgebung.
- Während des Starts werden die notwendigen Ressourcen bereitgestellt.

#### Beispielausgabe:
```
INFO Checking if podman remote executable is cached
INFO Checking if admin-helper executable is cached
INFO Starting CodeReady Containers
INFO OpenShift is running
```

### 2. Stoppen der CRC-Umgebung
```bash
crc stop
```
- Stoppt die aktuell laufende OpenShift-Instanz.
- Dies spart Systemressourcen, wenn die Umgebung nicht benötigt wird.

### 3. Überprüfen des Status
```bash
crc status
```
- Zeigt den aktuellen Status der CRC-Umgebung an.
- Wichtige Informationen:
  - Laufzeitstatus (`Running`, `Stopped`).
  - IP-Adresse der OpenShift-Instanz.
  - Cluster-Details.

#### Beispielausgabe:
```
CRC VM:          Running
OpenShift:       Running (v4.7.0)
Disk Usage:      15.23GB
Cache Usage:     4.12GB
```

### 4. Abrufen von Logs
```bash
crc logs
```
- Zeigt die Logs der CRC-Instanz an.
- Nützlich für das Debugging von Problemen während des Starts oder Betriebs.

#### Beispiel:
```bash
crc logs | grep "ERROR"
```
- Filtern Sie nach Fehlern in den Logs.

---

## Troubleshooting und Monitoring von CRC

Fehler und Probleme können bei der Arbeit mit CRC auftreten. Die folgenden Schritte helfen bei der Fehlerbehebung und Überwachung.

### 1. Fehlerbehebung bei Startproblemen
- **Fehler: "Insufficient resources"**:
  - Stellen Sie sicher, dass Ihr System die Mindestanforderungen erfüllt:
    - 4 vCPUs.
    - 9 GB RAM.
    - 35 GB Speicherplatz.
  - Erhöhen Sie die Ressourcenzuweisung in der CRC-Konfiguration:
    ```bash
    crc config set cpus 6
    crc config set memory 12288
    ```

- **Fehler: "Cluster not reachable"**:
  - Überprüfen Sie, ob die VM läuft:
    ```bash
    crc status
    ```
  - Überprüfen Sie die Netzwerkverbindung:
    ```bash
    ping <CRC-IP>
    ```

### 2. Zurücksetzen der Umgebung
```bash
crc delete
crc start
```
- Löscht die aktuelle Instanz und startet eine neue Umgebung.

### 3. Logs analysieren
Nutzen Sie die Logs, um spezifische Probleme zu identifizieren:
```bash
crc logs
```
Suchen Sie nach spezifischen Fehlermeldungen oder Warnungen.

### 4. Monitoring der CRC-Umgebung
- **OpenShift-Webkonsole**:
  - Zugriff über die URL aus der Ausgabe von `crc start` (z. B. `https://api.crc.testing:6443`).
  - Standardanmeldedaten:
    - Benutzername: `kubeadmin`.
    - Passwort: Wird beim Start angezeigt.

- **CLI-Überwachung**:
  - Überprüfen Sie Pods und Deployments in OpenShift:
    ```bash
    oc get pods -A
    ```

---

## Best Practices für CRC

1. **Regelmäßiges Update der CRC CLI**:
   - Halten Sie die CLI aktuell, um von neuen Funktionen und Fehlerbehebungen zu profitieren.
   ```bash
   crc version
   crc update
   ```

2. **Ressourcenoptimierung**:
   - Passen Sie die Ressourcenzuweisung je nach Bedarf an:
     ```bash
     crc config set cpus <Anzahl>
     crc config set memory <Größe in MB>
     ```

3. **Backup und Wiederherstellung**:
   - Exportieren Sie wichtige Konfigurationen oder Ressourcen, bevor Sie die Umgebung löschen.

---