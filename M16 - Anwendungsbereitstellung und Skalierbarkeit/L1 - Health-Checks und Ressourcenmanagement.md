
# Health-Checks und Ressourcenmanagement
## Health-Checks in OpenShift

Health-Checks stellen sicher, dass Anwendungen korrekt funktionieren und dass fehlerhafte Pods identifiziert und gegebenenfalls neu gestartet werden.

### Arten von Health-Checks:
1. **Liveness-Probe**:
   - Überprüft, ob der Pod noch läuft.
   - Wenn der Check fehlschlägt, wird der Pod neu gestartet.

2. **Readiness-Probe**:
   - Überprüft, ob der Pod Anfragen verarbeiten kann.
   - Pods, die nicht bereit sind, werden aus dem Load Balancer entfernt.

3. **Startup-Probe**:
   - Überprüft, ob die Anwendung erfolgreich gestartet wurde.
   - Verhindert unnötige Neustarts bei langsamen Starts.

### Beispielkonfiguration für Health-Checks:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - name: my-container
    image: my-image
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /readiness
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
    startupProbe:
      httpGet:
        path: /startup
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
```

### Erklärung:
- **`httpGet`**: Führt einen HTTP-Request auf den angegebenen Pfad und Port aus.
- **`initialDelaySeconds`**: Wartezeit nach dem Start des Containers, bevor der Check ausgeführt wird.
- **`periodSeconds`**: Zeitintervall zwischen den Checks.

### Testen von Health-Checks:
1. Bereitstellen des Pods:
   ```bash
   oc apply -f health-checks.yaml
   ```
2. Überprüfen des Status:
   ```bash
   oc describe pod my-app
   ```
   Hier sehen Sie, ob die Probes erfolgreich sind.

## Ressourcenmanagement in OpenShift

Die effiziente Nutzung von CPU und Arbeitsspeicher ist entscheidend für die Stabilität und Leistung von Anwendungen. OpenShift ermöglicht die Verwaltung durch **Ressourcenlimits** und **-anfragen**.

### Ressourcenanfragen und Limits:
- **Ressourcenanfragen (`requests`)**:
  - Minimale Ressourcen, die ein Container benötigt.
  - OpenShift verwendet diese Werte, um Ressourcen auf Nodes zu planen.

- **Ressourcenlimits (`limits`)**:
  - Maximale Ressourcen, die ein Container nutzen darf.
  - Überschreitungen führen zu Drosselung oder Beendigung.

### Beispiel für Ressourcenlimits:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - name: my-container
    image: my-image
    resources:
      requests:
        memory: "128Mi"
        cpu: "500m"
      limits:
        memory: "256Mi"
        cpu: "1000m"
```

### Erklärung:
- **`requests`**: Der Container wird mindestens 128 MiB Arbeitsspeicher und 500 Millicores CPU erhalten.
- **`limits`**: Der Container kann maximal 256 MiB Arbeitsspeicher und 1000 Millicores CPU nutzen.

### Optimierung von Ressourcen:
1. **Überwachung**:
   - Nutzen Sie OpenShift Monitoring (z. B. Prometheus), um den Ressourcenverbrauch zu analysieren:
     ```bash
     oc adm top pods
     ```
2. **Anpassen der Ressourcenwerte**:
   - Setzen Sie realistische `requests` und `limits`, basierend auf der tatsächlichen Nutzung.

### QoS-Klassen:
Die Kombination von `requests` und `limits` bestimmt die **Quality of Service (QoS)**-Klasse des Pods:
1. **Guaranteed**: `requests` = `limits` für alle Ressourcen.
2. **Burstable**: `requests` < `limits`.
3. **BestEffort**: Keine `requests` oder `limits` definiert.

### Ressourcenquoten:
OpenShift ermöglicht die Einschränkung von Ressourcen für Projekte/Namensräume.
#### Beispiel:
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "4Gi"
    limits.cpu: "4"
    limits.memory: "8Gi"
```
- Definiert, dass alle Pods im Namensraum maximal 4 CPUs und 8 GiB Speicher beanspruchen dürfen.

### LimitRanges:
Zusätzlich zu Quoten können Standardwerte für `requests` und `limits` festgelegt werden.
#### Beispiel:
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: resource-limits
spec:
  limits:
  - default:
      cpu: "500m"
      memory: "256Mi"
    defaultRequest:
      cpu: "200m"
      memory: "128Mi"
    type: Container
```