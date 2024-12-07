
## 3. Helm Releases und Rollbacks (ca. 60 Minuten)

### 3.1 Was ist ein Helm Release?
Ein **Release** ist eine Instanz eines Charts, die in einem Kubernetes-Cluster installiert wurde. Wenn ein Chart installiert wird, erzeugt Helm ein Release, das versioniert und verwaltet wird.

### 3.2 Release Management
Wichtige Befehle für das Management von Releases:
- **Installieren** eines Charts:
  ```bash
  helm install my-app bitnami/nginx
  ```
- **Upgraden** eines Releases:
  ```bash
  helm upgrade my-app bitnami/nginx
  ```
  - Dabei können Werte angepasst oder Charts aktualisiert werden.
- **Rollback** eines Releases:
  ```bash
  helm rollback my-app 1
  ```
  - Setzt das Release auf eine frühere Version zurück.

### 3.3 Praktische Übung: Installation, Upgrade und Rollback
1. **Installiere das Nginx-Chart**:
   ```bash
   helm install my-app bitnami/nginx
   ```
2. **Upgrade das Chart**, indem du die `values.yaml` änderst (z.B. erhöhe die Replikate auf 3):
   ```yaml
   replicaCount: 3
   ```
   - Führe das Upgrade aus:
     ```bash
     helm upgrade my-app bitnami/nginx
     ```
3. **Rollback** zur vorherigen Version:
   ```bash
   helm rollback my-app 1
   ```

## 4. Erstellen eines eigenen Helm Charts (ca. 90 Minuten)

### 4.1 Helm Chart Struktur
Um ein eigenes Chart zu erstellen, verwendet man den Befehl:
```bash
helm create my-chart
```
Dieser Befehl erzeugt eine Grundstruktur für ein Helm Chart. Das generierte Verzeichnis enthält:
- **`Chart.yaml`**: Metadaten des Charts.
- **`values.yaml`**: Standardwerte für das Chart.
- **`templates/`**: Templating-Dateien für Kubernetes-Ressourcen.

### 4.2 Anpassen eines Charts
Passe die `values.yaml` oder die Templates an, um dein Chart zu modifizieren. Beispiel für ein Deployment-Template:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
      - name: nginx
        image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        ports:
        - containerPort: {{ .Values.service.port }}
```

### 4.3 Praktische Übung: Eigenes Helm Chart erstellen
1. **Erstelle ein neues Chart**:
   ```bash
   helm create my-app
   ```
2. **Passe die `values.yaml` an**, z.B.:
   ```yaml
   image:
     repository: my-custom-image
     tag: 1.0.0
   replicaCount: 3
   ```
3. **Installiere dein Chart**:
   ```bash
   helm install custom-app ./my-app
   ```

## 5. Helm Repositories und Produktionseinsatz (ca. 60 Minuten)

### 5.1 Was ist ein Helm Repository?
Ein **Helm Repository** ist eine Sammlung von Helm Charts, die über HTTP oder HTTPS zugänglich sind. Bekannte öffentliche Repositories sind:
- **Offizielles Helm Repository**: [https://charts.helm.sh/stable](https://charts.helm.sh/stable)
- **Bitnami Repository**: [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)

### 5.2 Arbeiten mit Helm Repositories
- **Repository hinzufügen**:
  ```bash
  helm repo add bitnami https://charts.bitnami.com/bitnami
  helm repo update
  ```
- **Charts suchen**:
  ```bash
  helm search repo nginx
  ```

### 5.3 Eigene Helm Repositories erstellen
1. Ein Helm-Repository kann auf einem einfachen Webserver oder über Tools wie GitHub Pages gehostet werden.
2. **Eigenes Repository** mit GitHub Pages:
   - Lade dein Chart in ein GitHub-Repository hoch und hoste es über GitHub Pages. Füge es dann mit Helm als Repository hinzu.

### 5.4 Best Practices für Produktion
- **Signierte Charts**: Signiere Charts, um sicherzustellen, dass sie nicht manipuliert wurden.
- **CI/CD Integration**: Integriere Helm in CI/CD-Pipelines, um den Bereitstellungsprozess zu automatisieren.

### 5.5 Praktische Übung: Repository verwenden
1. **Füge ein Repository hinzu**:
   - Füge das Bitnami Repository hinzu, das eine große Sammlung von populären Charts enthält:
     ```bash
     helm repo add bitnami https://charts.bitnami.com/bitnami
     helm repo update
     ```
2. **Verwende ein Chart aus dem Repository**:
   - Installiere eine Anwendung, z.B. WordPress, aus dem Repository:
     ```bash
     helm install my-wordpress bitnami/wordpress
     ```
3. **Erstelle ein eigenes Repository** mit einem simplen Webserver oder GitHub Pages.
