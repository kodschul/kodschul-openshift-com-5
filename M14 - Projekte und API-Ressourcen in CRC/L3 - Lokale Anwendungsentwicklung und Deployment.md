
# Lokale Anwendungsentwicklung und Deployment
## Lokale Anwendungsentwicklung und Deployment

### Was ist CRC (CodeReady Containers)?
CRC (CodeReady Containers) ist ein Tool, das es Entwicklern ermöglicht, OpenShift lokal auf ihrem Computer zu betreiben. Es stellt eine leichtgewichtige Version von OpenShift bereit und ist ideal für Entwicklungs- und Testumgebungen.

#### Vorteile:
- **Einfaches Setup**: Keine zusätzliche Infrastruktur erforderlich.
- **Entwicklung in Echtzeit**: Lokale Tests und Debugging.
- **Konsistenz**: Entspricht der OpenShift-Produktionsumgebung.

### Installation von CRC:
1. Laden Sie CRC herunter:
   [CodeReady Containers Download](https://developers.redhat.com/products/codeready-containers/overview)
2. Installieren Sie CRC:
   ```bash
   crc setup
   crc start
   ```
3. Melden Sie sich bei der OpenShift-Webkonsole an:
   ```bash
   crc console
   ```

## Deployment von Container-Anwendungen in CRC

### Erstellen eines Projekts:
Ein Projekt ist eine isolierte Umgebung für Anwendungen und Ressourcen.
```bash
oc new-project my-project
```

### Deployment mit einem Docker-Image:
1. Deployment mit `oc` erstellen:
   ```bash
   oc new-app my-docker-image --name=my-app
   ```
2. Status des Deployments überprüfen:
   ```bash
   oc status
   oc get pods
   ```
3. Zugriff auf die Anwendung konfigurieren:
   ```bash
   oc expose svc/my-app
   oc get routes
   ```

### Deployment mit einer YAML-Datei:
Beispielkonfiguration:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
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
        image: my-docker-image
        ports:
        - containerPort: 8080
```
Deployment bereitstellen:
```bash
oc apply -f deployment.yaml
```

## Einführung in Core Workloads

### Deployments
Deployments sind der Standard-Controller in OpenShift zur Verwaltung von Anwendungen. Sie bieten:
- Skalierbarkeit
- Rollbacks
- Automatische Updates

#### Beispiel:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
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
        image: nginx
```

### StatefulSets
StatefulSets werden für Anwendungen verwendet, die stabile Netzwerk-Identitäten und persistente Speicher benötigen.
- **Einsatzbereiche**: Datenbanken, verteilte Systeme.
- **Eigenschaften**:
  - Stabile Namen für Pods
  - Geordnete Bereitstellung

#### Beispiel:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-statefulset
spec:
  serviceName: "my-service"
  replicas: 2
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
        image: redis
```

### DaemonSets
DaemonSets stellen sicher, dass auf jedem Node ein Pod ausgeführt wird.
- **Einsatzbereiche**: Überwachung, Protokollierung, Netzwerkkonfiguration.

#### Beispiel:
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
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
        image: fluentd
```