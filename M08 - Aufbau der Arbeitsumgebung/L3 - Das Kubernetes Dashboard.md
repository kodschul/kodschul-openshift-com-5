
# Das Kubernetes Dashboard
## Überblick über das Kubernetes Dashboard

Das Kubernetes Dashboard ist eine grafische Benutzeroberfläche, mit der Benutzer die Ressourcen in einem Kubernetes-Cluster anzeigen und verwalten können. Es bietet einen Überblick über die aktuellen Zustände der Cluster und ermöglicht es, Ressourcen wie Deployments, Pods und Services zu überwachen und zu steuern.

### Hauptfunktionen:
- **Cluster-Status-Ansicht**: Visualisierung des Zustands des Clusters, einschließlich verfügbarer Nodes und laufender Workloads.
- **Ressourcenmanagement**: Erstellung, Aktualisierung und Skalierung von Ressourcen wie Deployments und ReplicaSets.
- **Fehlerdiagnose**: Anzeigen von Logs und Debugging von Pods.
- **Benutzerverwaltung**: Konfiguration von Zugriffskontrollen (RBAC) und Service Accounts.

### Vorteile des Dashboards:
- **Benutzerfreundlichkeit**: Intuitive grafische Oberfläche zur Verwaltung komplexer Ressourcen.
- **Monitoring**: Übersicht über Cluster-Zustand und Workloads in Echtzeit.
- **Effizienz**: Reduziert den Bedarf an manuellen CLI-Kommandos für Standardaufgaben.

## Nutzungsmöglichkeiten und Zugriff auf das Dashboard

### Voraussetzungen
- Ein laufender Kubernetes-Cluster.
- `kubectl` CLI installiert und konfiguriert.
- Zugriff auf den Cluster mit ausreichenden Berechtigungen.

### Installation des Dashboards
Das Kubernetes Dashboard kann mit einem einfachen YAML-Manifest bereitgestellt werden:
1. Installieren Sie das Dashboard:
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.0/aio/deploy/recommended.yaml
   ```

2. Überprüfen Sie, ob die Dashboard-Pods erfolgreich erstellt wurden:
   ```bash
   kubectl get pods -n kubernetes-dashboard
   ```

### Zugriff auf das Dashboard
Das Dashboard ist standardmäßig nicht öffentlich zugänglich und erfordert Port-Forwarding oder eine spezielle Konfiguration.

#### 1. Zugriff per Port-Forwarding:
1. Starten Sie das Port-Forwarding:
   ```bash
   kubectl proxy
   ```
2. Öffnen Sie das Dashboard im Browser:
   ```
   http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
   ```

#### 2. Zugriff mit einem Token:
1. Erstellen Sie einen Service Account mit Dashboard-Zugriff:
   ```bash
   kubectl create serviceaccount dashboard-admin-sa
   kubectl create clusterrolebinding dashboard-admin-sa    --clusterrole=cluster-admin --serviceaccount=default:dashboard-admin-sa
   ```

2. Rufen Sie das Token ab:
   ```bash
   kubectl get secret $(kubectl get serviceaccount dashboard-admin-sa -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
   ```

3. Melden Sie sich im Dashboard mit dem Token an.

#### 3. Sicherer Zugriff (Optionen):
- **Ingress-Controller**: Konfigurieren Sie einen Ingress, um das Dashboard über HTTPS verfügbar zu machen.
- **Load Balancer**: Erstellen Sie einen Load Balancer-Service, um externen Zugriff zu ermöglichen.

### Nutzung des Dashboards
Nach der Anmeldung bietet das Dashboard die folgenden Funktionen:
- **Übersicht**: Anzeigen von Cluster-Ressourcen und ihrer Zustände.
- **Pod-Logs**: Anzeigen und Analysieren der Logs von Pods.
- **Deployments**: Skalieren, Aktualisieren und Erstellen neuer Deployments.
- **Jobs**: Verwalten und Überwachen von Batch-Jobs und CronJobs.

#### Beispiel: Deployment erstellen
1. Navigieren Sie zu "Deployments".
2. Klicken Sie auf "Create" und füllen Sie die Felder aus (z. B. Image, Anzahl der Replikas).
3. Überprüfen Sie die Deployment-Details nach der Erstellung.
