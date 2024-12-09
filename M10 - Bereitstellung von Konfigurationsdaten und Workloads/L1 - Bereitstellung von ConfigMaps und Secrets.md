
# Bereitstellung von ConfigMaps und Secrets

## ConfigMaps

### Was sind ConfigMaps?
ConfigMaps werden verwendet, um Konfigurationsdaten von Anwendungen zu speichern. Sie ermöglichen es, Konfigurationswerte unabhängig vom Container-Image zu verwalten und zu aktualisieren.

### Verwendung von ConfigMaps
ConfigMaps können auf verschiedene Arten erstellt und verwendet werden:
- **Direkt aus einer Datei**: Konfigurationsdaten werden aus einer Datei importiert.
- **Manuell**: Einzelne Schlüssel-Wert-Paare können definiert werden.

#### Beispiel: Erstellung einer ConfigMap
1. **ConfigMap aus einer Datei erstellen**:
   Erstellen Sie eine Datei `config.properties`:
   ```
   APP_NAME=KubernetesApp
   APP_ENV=production
   ```
   Erstellen Sie die ConfigMap mit:
   ```bash
   kubectl create configmap app-config --from-file=config.properties
   ```

2. **ConfigMap direkt definieren**:
   Erstellen Sie eine Datei `configmap.yaml`:
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: app-config
   data:
     APP_NAME: KubernetesApp
     APP_ENV: production
   ```
   Anwenden mit:
   ```bash
   kubectl apply -f configmap.yaml
   ```

### Verwendung in Pods
ConfigMaps können in Pods auf verschiedene Weisen eingebunden werden:
- **Als Umgebungsvariablen**:
  ```yaml
  env:
    - name: APP_NAME
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_NAME
  ```

- **Als Volumes**:
  ```yaml
  volumes:
    - name: config-volume
      configMap:
        name: app-config
  volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  ```

## Secrets

### Was sind Secrets?
Secrets sind ähnlich wie ConfigMaps, aber speziell für sensible Daten wie Passwörter, Tokens und Zertifikate. Secrets sind in Base64 codiert und bieten ein höheres Maß an Sicherheit.

### Erstellung von Secrets
Secrets können ähnlich wie ConfigMaps erstellt werden:
1. **Von der Kommandozeile**:
   ```bash
   kubectl create secret generic db-credentials --from-literal=username=admin --from-literal=password=secret
   ```

2. **Mit einer YAML-Datei**:
   Erstellen Sie eine Datei `secret.yaml`:
   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: db-credentials
   type: Opaque
   data:
     username: YWRtaW4=  # Base64-codiert
     password: c2VjcmV0
   ```
   Anwenden mit:
   ```bash
   kubectl apply -f secret.yaml
   ```

### Verwendung in Pods
Secrets können ebenfalls als Umgebungsvariablen oder Volumes eingebunden werden:
- **Als Umgebungsvariablen**:
  ```yaml
  env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: username
  ```

- **Als Volumes**:
  ```yaml
  volumes:
    - name: secret-volume
      secret:
        secretName: db-credentials
  volumeMounts:
    - name: secret-volume
      mountPath: /etc/secrets
  ```

## Best Practices

1. **Trennung von Konfiguration und Code**:
   - Bewahren Sie Konfigurationsdaten in ConfigMaps und Secrets auf, nicht im Container-Image.

2. **Sichere Speicherung von Secrets**:
   - Verwenden Sie ein sicheres Backend wie HashiCorp Vault oder AWS Secrets Manager, wenn möglich.
   - Stellen Sie sicher, dass der Zugriff auf Secrets eingeschränkt ist.

3. **Regelmäßige Aktualisierung**:
   - Aktualisieren Sie ConfigMaps und Secrets regelmäßig und testen Sie Ihre Anwendungen auf Reaktion auf Änderungen.

4. **Verwendung von RBAC**:
   - Implementieren Sie Role-Based Access Control (RBAC), um den Zugriff auf ConfigMaps und Secrets zu steuern.
