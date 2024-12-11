
# Cluster-Grundlagen unter CRC
## Verständnis der Control-Plane und Worker-Knoten in CRC

Ein OpenShift Cluster basiert auf der Kubernetes-Architektur und besteht aus zwei Hauptkomponenten:
1. **Control-Plane**:
   - Verantwortlich für die Verwaltung und Überwachung des Clusters.
   - Enthält Komponenten wie `etcd`, den API-Server und den Scheduler.

2. **Worker-Knoten**:
   - Führen die Container-Workloads aus, die von der Control-Plane geplant werden.
   - Enthalten den Kubelet-Dienst und das Container-Laufzeitsystem.

### Cluster in CRC
- **CodeReady Containers (CRC)**:
  - CRC ermöglicht das lokale Ausführen eines OpenShift Clusters für Entwicklungs- und Testzwecke.
  - Es handelt sich um einen Cluster mit einer vereinfachten Architektur (Single-Node-Cluster), der Control-Plane- und Worker-Komponenten auf demselben Host vereint.

### Vorteile von CRC:
- Einfacher Einstieg in OpenShift.
- Leichte Einrichtung auf lokalen Entwicklungsmaschinen.
- Ideal für Tests und Schulungszwecke.

## Verwaltung des Clusters mit CLI-Tools (oc und kubectl)

### OpenShift CLI (`oc`)
Das `oc`-Tool bietet eine Erweiterung der Kubernetes-CLI `kubectl` mit spezifischen OpenShift-Funktionen.

#### Häufig verwendete Befehle:
1. **Anmelden beim Cluster**:
   ```bash
   oc login -u kubeadmin -p <password> https://<api-server>
   ```

2. **Cluster-Status anzeigen**:
   ```bash
   oc status
   ```

3. **Namespaces anzeigen**:
   ```bash
   oc get projects
   ```

4. **Anwendungen bereitstellen**:
   ```bash
   oc new-app <image> --name=<app-name>
   ```

5. **Ressourcen anzeigen**:
   ```bash
   oc get pods
   oc get services
   oc get routes
   ```

### Kubernetes CLI (`kubectl`)
Die `kubectl`-CLI kann ebenfalls verwendet werden, um grundlegende Kubernetes-Funktionen im OpenShift Cluster zu verwalten.

#### Häufig verwendete Befehle:
1. **Pods anzeigen**:
   ```bash
   kubectl get pods
   ```

2. **Details eines Pods anzeigen**:
   ```bash
   kubectl describe pod <pod-name>
   ```

3. **Logs eines Pods abrufen**:
   ```bash
   kubectl logs <pod-name>
   ```

4. **Manifeste anwenden**:
   ```bash
   kubectl apply -f <file.yaml>
   ```

#### Unterschied zwischen `oc` und `kubectl`:
- `kubectl` bietet generische Kubernetes-Funktionalität.
- `oc` umfasst zusätzliche Befehle, die spezifisch für OpenShift sind, wie die Verwaltung von Projekten und Routen.

## Einführung in die OpenShift Console: GUI-Zugang einrichten

Die OpenShift Console ist die grafische Benutzeroberfläche (GUI) für die Verwaltung eines OpenShift Clusters. Sie bietet eine benutzerfreundliche Oberfläche zur Visualisierung und Verwaltung von Ressourcen.

### Schritte zur Einrichtung:
1. **Zugangsdaten abrufen**:
   - Nach der Installation von CRC finden Sie die Anmeldedaten mit:
     ```bash
     crc console --credentials
     ```

2. **Starten der Console**:
   - Öffnen Sie die OpenShift Console in Ihrem Browser:
     ```bash
     crc console
     ```
   - Der Befehl gibt eine URL aus (z. B. `https://console-openshift-console.apps-crc.testing`).

3. **Anmelden**:
   - Verwenden Sie die Anmeldedaten (`kubeadmin` und Passwort), um Zugriff auf die GUI zu erhalten.

### Funktionen der OpenShift Console:
- **Dashboard**: Übersicht über Cluster-Status und Ressourcen.
- **Projektverwaltung**: Erstellen und Verwalten von Projekten und Namespaces.
- **Anwendungsbereitstellung**: Erstellen und Verwalten von Deployments, Pods und Services.
- **Monitoring**: Anzeige von Logs und Metriken.

### Vorteile der Console:
- Intuitive Benutzeroberfläche.
- Visualisierung von Ressourcen und deren Status.
- Einfache Navigation für Entwickler und Administratoren.
