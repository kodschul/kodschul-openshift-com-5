
# inführung in CI/CD
## CRC als Plattform für lokale Pipeline-Tests nutzen

### Was ist CRC?
CRC (CodeReady Containers) ist eine lokale OpenShift-Umgebung, die speziell für Entwickler entwickelt wurde. Mit CRC können Sie OpenShift-Cluster lokal auf Ihrem Rechner betreiben, um Anwendungen zu entwickeln und CI/CD-Pipelines zu testen.

### Vorteile von CRC:
- **Lokal und eigenständig**: Keine Abhängigkeit von einer Cloud- oder Remote-Umgebung.
- **Einfaches Setup**: Installation und Konfiguration sind schnell erledigt.
- **Repliziert Produktionsumgebungen**: Erlaubt realistische Tests in einer OpenShift-ähnlichen Umgebung.

### Installation von CRC:
1. Laden Sie CRC von der [offiziellen Seite](https://developers.redhat.com/products/openshift-local/overview) herunter.
2. Installieren Sie CRC:
   ```bash
   crc setup
   crc start
   ```
3. Loggen Sie sich in die lokale OpenShift-Umgebung ein:
   ```bash
   oc login -u developer -p developer https://api.crc.testing:6443
   ```

### CI/CD-Pipelines mit CRC:
OpenShift nutzt Tekton als Standard-Tool für CI/CD-Pipelines. Mit CRC können Sie Tekton-Pipelines lokal erstellen, testen und debuggen.

#### Beispiel einer Tekton-Pipeline:
Erstellen Sie eine Datei `pipeline.yaml`:
```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: example-pipeline
spec:
  tasks:
    - name: build
      taskRef:
        name: build-task
    - name: deploy
      taskRef:
        name: deploy-task
```

Deployen Sie die Pipeline:
```bash
oc apply -f pipeline.yaml
```

Führen Sie die Pipeline aus:
```bash
tkn pipeline start example-pipeline
```

#### Vorteile der lokalen Pipeline-Tests:
- Schnelle Iterationen durch lokale Tests.
- Fehler können frühzeitig erkannt und behoben werden.
- Reduziert Abhängigkeiten von produktiven Ressourcen.

## Integration von GitOps-Workflows: Grundlagen und Vorteile

### Was ist GitOps?
GitOps ist ein Ansatz zur Anwendungsbereitstellung und -verwaltung, bei dem Git als Single Source of Truth verwendet wird. Änderungen an der Infrastruktur oder an Anwendungen werden über Git-Commits verwaltet.

### Grundlagen von GitOps:
1. **Git als zentrale Steuerungsebene**:
   - Änderungen an der Infrastruktur oder an Anwendungen erfolgen durch Pull-Requests.
2. **Automatische Synchronisierung**:
   - Tools wie ArgoCD oder Flux überprüfen kontinuierlich den Git-Status und synchronisieren die gewünschte Konfiguration mit der Zielumgebung.
3. **Versionierung**:
   - Alle Änderungen sind nachvollziehbar, da sie in Git protokolliert werden.

### Vorteile von GitOps:
- **Transparenz und Nachvollziehbarkeit**:
  - Änderungen sind dokumentiert und leicht nachzuvollziehen.
- **Automatisierung**:
  - Minimiert manuelle Eingriffe und Fehler durch automatisches Rollout und Rollback.
- **Sicherheit**:
  - Nur genehmigte Änderungen aus Git werden in die Produktion übernommen.

### GitOps in OpenShift mit ArgoCD:
ArgoCD ist ein beliebtes Tool zur Implementierung von GitOps in Kubernetes und OpenShift.

#### Installation von ArgoCD:
1. Installieren Sie ArgoCD:
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
2. Zugriff auf das ArgoCD-Dashboard:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```

#### Erstellen eines GitOps-Repository:
1. Konfigurieren Sie eine Anwendung in ArgoCD:
   ```bash
   argocd app create my-app      --repo https://github.com/my-repo.git      --path manifests      --dest-server https://kubernetes.default.svc      --dest-namespace default
   ```
2. Synchronisieren Sie die Anwendung:
   ```bash
   argocd app sync my-app
   ```

### GitOps und CI/CD:
GitOps ergänzt CI/CD-Workflows, indem es die Bereitstellung nach einem erfolgreichen Build automatisiert. CRC bietet die ideale Testumgebung, um diese Workflows lokal zu simulieren.
