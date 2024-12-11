
# API-Ressourcen verstehen
## Ressourcenmodelle in Kubernetes und OpenShift

Sowohl Kubernetes als auch OpenShift basieren auf einem deklarativen Ansatz zur Verwaltung von Container-Orchestrierung. Die zugrunde liegende Architektur verwendet API-Ressourcen, um die gewünschten Zustände zu definieren und zu verwalten.

### Kubernetes Ressourcenmodell

Das Kubernetes-Objektmodell ist die Grundlage für alle Operationen. Jedes Objekt wird durch eine API-Ressource repräsentiert, die in einer YAML- oder JSON-Datei beschrieben wird.

#### Beispiele für Ressourcen:
1. **Pods**: Die kleinste ausführbare Einheit, die einen oder mehrere Container enthält.
2. **Deployments**: Verwalten von Replikation und Rollouts von Pods.
3. **Services**: Bieten Netzwerkschnittstellen und Load-Balancing für Pods.
4. **ConfigMaps** und **Secrets**: Verwaltung von Konfigurationsdaten.

### OpenShift Erweiterungen

OpenShift baut auf Kubernetes auf und bietet zusätzliche API-Ressourcen:
1. **BuildConfigs**: Ermöglicht die Automatisierung von Builds.
2. **ImageStreams**: Verfolgen Container-Images und unterstützen Continuous Integration.
3. **Routes**: Stellen externen Zugriff auf Dienste bereit.

Beispiel einer Deployment-Ressource in YAML:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image:latest
        ports:
        - containerPort: 80
```

## Zugriff und Interaktion mit der OpenShift-API

Die OpenShift-API ermöglicht es Ihnen, Ressourcen zu erstellen, zu verwalten und zu überwachen. Der Zugriff erfolgt über den OpenShift CLI (`oc`), die Webkonsole oder direkte API-Aufrufe.

### 1. Authentifizierung und API-Endpunkte

#### Anmeldung:
Melden Sie sich bei einer OpenShift-Instanz an, um Zugriff auf die API zu erhalten:
```bash
oc login https://openshift.example.com --token=<TOKEN>
```

#### API-Endpunkt finden:
Den API-Endpunkt einer OpenShift-Instanz können Sie durch Aufrufen der API-Wurzel ermitteln:
```bash
curl -k https://openshift.example.com:6443
```

### 2. Interaktion mit der OpenShift-API

#### Verwendung von `oc` CLI
Die `oc` CLI ist das primäre Werkzeug für die Interaktion mit der OpenShift-API.

Beispiele:
- **Ressourcen abrufen**:
   ```bash
   oc get pods
   ```
- **Neue Ressourcen erstellen**:
   ```bash
   oc create -f resource.yaml
   ```
- **Ressourcen löschen**:
   ```bash
   oc delete pod my-pod
   ```

#### Direkte API-Aufrufe
Alternativ können Sie direkte API-Aufrufe mit Werkzeugen wie `curl` oder `Postman` ausführen.

Beispiel:
- **GET-Anfrage für Pods**:
   ```bash
   curl -k -H "Authorization: Bearer <TOKEN>" https://openshift.example.com:6443/api/v1/pods
   ```

#### Zugriff über Skripte
Sie können die API auch programmgesteuert nutzen, z. B. mit Python:
```python
import requests

url = "https://openshift.example.com:6443/api/v1/pods"
headers = {"Authorization": "Bearer <TOKEN>"}
response = requests.get(url, headers=headers, verify=False)

print(response.json())
```

### 3. Erweiterte Interaktionen

#### Labels und Selektoren verwenden:
Mit Labels können Sie Ressourcen organisieren und selektieren.
```bash
oc get pods -l app=my-app
```

#### Ressourcen patchen:
Führen Sie kleine Änderungen an Ressourcen durch:
```bash
oc patch deployment my-app --type='json' -p='[{"op": "replace", "path": "/spec/replicas", "value":5}]'
```

#### Überwachung:
- **Logs anzeigen**:
   ```bash
   oc logs my-pod
   ```
- **Ressourcenstatus prüfen**:
   ```bash
   oc describe pod my-pod
   ```

## Best Practices

1. **Verwenden Sie deklarative Konfigurationen**:
   - Speichern Sie alle Ressourcen in YAML-Dateien und verwalten Sie sie in Versionskontrollsystemen wie Git.
2. **Rollenbasierte Zugriffskontrolle (RBAC)**:
   - Richten Sie Zugriffskontrollen ein, um Ressourcen zu schützen.
3. **Regelmäßige Überwachung**:
   - Nutzen Sie Werkzeuge wie Prometheus oder die OpenShift Monitoring-Konsole, um die API und Ressourcen im Blick zu behalten.
4. **Automatisieren Sie Workflows**:
   - Integrieren Sie die API in CI/CD-Pipelines.
