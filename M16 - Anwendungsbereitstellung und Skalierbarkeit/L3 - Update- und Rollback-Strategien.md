
# Update- und Rollback-Strategien
## Updates für Anwendungen durchführen

In OpenShift können Anwendungen einfach aktualisiert werden, ohne den Betrieb zu unterbrechen. Dies wird durch Deployment-Konfigurationen und Rolling Updates ermöglicht.

### Arten von Updates in OpenShift:
1. **Rolling Updates**:
   - Aktualisiert Pods schrittweise.
   - Stellt sicher, dass zu jeder Zeit genügend Pods verfügbar sind, um den Betrieb aufrechtzuerhalten.

2. **Recreate Deployment**:
   - Stoppt alle bestehenden Pods und startet neue Pods mit der aktualisierten Version.
   - Eignet sich für stateful Anwendungen oder größere Änderungen.

3. **Blue-Green Deployment**:
   - Führt eine neue Version in einer parallelen Umgebung ein.
   - Schaltet nach Validierung auf die neue Version um.

4. **Canary Deployment**:
   - Veröffentlicht eine neue Version für einen kleinen Teil der Nutzer.
   - Nach erfolgreicher Validierung wird die neue Version für alle Nutzer ausgerollt.

### Durchführung eines Updates:
1. **Vorbereitung des Updates**:
   - Aktualisieren Sie die Image-Version oder die Konfiguration.
   ```bash
   oc set image deployment/my-app my-container=my-image:v2
   ```

2. **Überprüfung des Deployment-Status**:
   - Stellen Sie sicher, dass das Update ordnungsgemäß durchgeführt wurde.
   ```bash
   oc rollout status deployment/my-app
   ```

3. **Monitoring**:
   - Überwachen Sie die Logs und Metriken während des Updates.
   ```bash
   oc logs deployment/my-app
   ```

4. **Validierung**:
   - Prüfen Sie die Anwendung mit automatisierten Tests oder manuell.

## Rollback auf vorherige Versionen: Best Practices

Falls ein Update zu Problemen führt, bietet OpenShift integrierte Mechanismen, um schnell auf eine frühere Version zurückzukehren.

### Grundlagen des Rollbacks:
1. Jede neue Version wird als Revision gespeichert.
2. Ein Rollback stellt die Konfiguration und das Image einer früheren Revision wieder her.
3. Sie können wählen, ob alle Pods sofort auf die alte Version umgestellt werden oder ein Rolling Back durchgeführt wird.

### Schritte für einen Rollback:
1. **Überprüfen der Revisionen**:
   - Zeigt die Deployment-Historie an.
   ```bash
   oc rollout history deployment/my-app
   ```

2. **Rollback ausführen**:
   - Stellen Sie eine bestimmte Revision wieder her.
   ```bash
   oc rollout undo deployment/my-app --to-revision=2
   ```

3. **Status prüfen**:
   - Überprüfen Sie, ob der Rollback erfolgreich war.
   ```bash
   oc rollout status deployment/my-app
   ```

### Best Practices für Rollbacks:
1. **Automatische Backups**:
   - Sichern Sie Konfigurationen und persistenten Speicher vor Updates.

2. **Testing in Staging**:
   - Testen Sie neue Versionen in einer Staging-Umgebung, bevor sie in die Produktion gelangen.

3. **Canary Releases nutzen**:
   - Veröffentlichen Sie Updates zunächst für eine kleine Nutzergruppe, um Probleme frühzeitig zu erkennen.

4. **Rollback-Trigger automatisieren**:
   - Implementieren Sie Health Checks, die automatisch einen Rollback auslösen, wenn die neue Version fehlschlägt.

### Beispiel für eine Canary-Strategie:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 5
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
        image: my-image:v2
  strategy:
    rollingUpdate:
      maxSurge: 2
      maxUnavailable: 1
    type: RollingUpdate
```