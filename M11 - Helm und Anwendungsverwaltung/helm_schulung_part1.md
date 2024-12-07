
# Komplette Helm-Schulung: Vom Anfänger zum Experten

## 1. Einführung in Helm (ca. 60 Minuten)

### 1.1 Was ist Helm?
Helm ist ein Paketmanager für Kubernetes. Wie klassische Paketmanager (z.B. `apt`, `yum`) ermöglicht Helm das Installieren, Aktualisieren und Verwalten von Software auf Kubernetes-Clustern. Helm vereinfacht den Prozess des Deployments, indem es:
- **Charts** verwendet, die wiederverwendbare, versionskontrollierte Pakete von Kubernetes-Ressourcen sind.
- **Release Management** bietet, d.h., man kann Anwendungen installieren, aktualisieren, löschen und zu früheren Versionen zurückrollen.

### 1.2 Warum Helm?
Kubernetes bietet native Funktionen zum Bereitstellen von Anwendungen, aber Helm bietet:
- **Wiederverwendbarkeit**: Einmal erstellte Charts können für viele Kubernetes-Deployments verwendet werden.
- **Versionierung**: Releases können versioniert und jederzeit zurückgerollt werden.
- **Automatisierung**: Helm hilft, den Bereitstellungsprozess zu standardisieren und zu automatisieren.

### 1.3 Installation von Helm v3
1. **Linux/macOS**:
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
   ```
2. **Windows**:
   ```bash
   choco install kubernetes-helm
   ```
3. **Überprüfe die Installation**:
   ```bash
   helm version
   ```
   Es sollte eine Ausgabe wie `version.BuildInfo{Version:"v3.x.x"}` erscheinen.

### 1.4 Praktische Übung: Helm installieren und nutzen
1. **Repository hinzufügen**:
   - Helm verwendet Repositories, um Charts zu verwalten.
   - Füge ein beliebtes Helm-Repository hinzu:
     ```bash
     helm repo add bitnami https://charts.bitnami.com/bitnami
     helm repo update
     ```
2. **Installiere ein Chart**:
   - Installiere Nginx mit Helm:
     ```bash
     helm install my-nginx bitnami/nginx
     ```
     Dieser Befehl installiert Nginx in deinem Cluster. Überprüfe die Installation:
     ```bash
     kubectl get all
     ```

## 2. Helm Charts verstehen (ca. 90 Minuten)

### 2.1 Was ist ein Helm Chart?
Ein **Helm Chart** ist ein Paket, das alles enthält, um eine Anwendung oder einen Dienst in Kubernetes bereitzustellen. Es enthält:
- **Chart.yaml**: Metadaten des Charts (Name, Version, Beschreibung).
- **values.yaml**: Standardwerte, die beim Deployment verwendet werden.
- **templates/**: Verzeichnisse mit Kubernetes-Ressourcen, die als Go-Templates definiert sind.

Ein einfaches Beispiel für eine `Chart.yaml`:
```yaml
apiVersion: v2
name: nginx
description: A Helm chart for Kubernetes
version: 1.0.0
```

### 2.2 Werte und Templates in Helm
- **Go-Templating**: Helm verwendet die Templating-Engine von Go, um dynamische Werte in die Kubernetes-Ressourcen einzufügen. Dies macht es flexibel, Konfigurationen zu definieren, die zur Laufzeit angepasst werden können.
- **values.yaml**: Diese Datei enthält die Standardwerte, die in den Templates verwendet werden. Sie kann angepasst werden, um benutzerdefinierte Werte zu setzen.

Beispiel einer `values.yaml`:
```yaml
replicaCount: 2
image:
  repository: nginx
  tag: stable
service:
  type: ClusterIP
  port: 80
```

### 2.3 Praktische Übung: Ein einfaches Chart verstehen
1. **Lade ein bestehendes Chart herunter**:
   ```bash
   helm pull bitnami/nginx --untar
   ```
   - Dies lädt das Chart für Nginx herunter und entpackt es in ein Verzeichnis.
2. **Modifiziere die `values.yaml`**:
   - Ändere den `service`-Typ von `ClusterIP` auf `NodePort`:
     ```yaml
     service:
       type: NodePort
     ```
3. **Installiere das geänderte Chart**:
   ```bash
   helm install my-nginx ./nginx
   ```
