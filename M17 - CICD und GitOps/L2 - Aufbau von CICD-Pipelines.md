
# Aufbau von CI/CD-Pipelines in CRC
## Aufbau von CI/CD-Pipelines in CRC

### Was ist CRC?
CodeReady Containers (CRC) ist eine lokale OpenShift-Instanz, die es Entwicklern ermöglicht, Anwendungen zu entwickeln und zu testen. CRC ist ideal für die Entwicklung und Simulation von CI/CD-Pipelines in einer kontrollierten Umgebung.

### Vorteile von CRC:
- **Lokale Entwicklungsumgebung**: Entwickeln und testen Sie Pipelines, ohne eine vollständige Cluster-Infrastruktur bereitzustellen.
- **Einfache Einrichtung**: Schnell einsatzbereit mit vorgefertigten OpenShift-Clustern.
- **Simulation produktionsähnlicher Workloads**.

### Schritte zum Aufbau einer CI/CD-Pipeline in CRC:
1. **Installation von CRC**:
   - Laden Sie CRC herunter und installieren Sie es:
     ```bash
     crc setup
     crc start
     ```
   - Greifen Sie auf die OpenShift-Konsole zu:
     ```bash
     crc console
     ```

2. **Einrichten einer Pipeline**:
   - Verwenden Sie Tools wie Tekton oder Jenkins, um die Pipeline zu konfigurieren.
   - Installieren Sie Tekton in CRC:
     ```bash
     kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
     ```

3. **Erstellen von Ressourcen für die Pipeline**:
   - Definieren Sie Tasks, Pipelines und PipelineRuns.

### Beispiel: Einfache Tekton-Pipeline
Erstellen Sie eine Pipeline, die Code aus einem Git-Repository zieht und einen Build ausführt.
```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: simple-pipeline
spec:
  tasks:
  - name: fetch-repo
    taskRef:
      name: git-clone
    params:
    - name: url
      value: https://github.com/example/repo.git
  - name: build-image
    taskRef:
      name: buildah
    runAfter:
    - fetch-repo
    params:
    - name: IMAGE
      value: quay.io/example/image:latest
```

## Beispiele für CI/CD mit gängigen Tools

### Jenkins
1. **Installation von Jenkins**:
   - Installieren Sie Jenkins in Ihrem CRC-Cluster:
     ```bash
     oc new-app jenkins-ephemeral
     ```
   - Zugriff auf die Jenkins-UI:
     ```bash
     oc get routes
     ```

2. **Erstellen eines Jenkins-Pipelineskripts**:
   ```groovy
   pipeline {
       agent any
       stages {
           stage('Build') {
               steps {
                   echo 'Building...'
               }
           }
           stage('Test') {
               steps {
                   echo 'Testing...'
               }
           }
           stage('Deploy') {
               steps {
                   echo 'Deploying...'
               }
           }
       }
   }
   ```

3. **Integrieren mit OpenShift**:
   - Verwenden Sie das OpenShift Jenkins Plugin, um Deployments aus Jenkins heraus zu triggern.

### Tekton
1. **Definieren von Tasks**:
   ```yaml
   apiVersion: tekton.dev/v1beta1
   kind: Task
   metadata:
     name: build-task
   spec:
     steps:
     - name: build
       image: quay.io/buildah/stable
       script: |
         #!/bin/sh
         buildah build-using-dockerfile -t $(params.image) .
   ```

2. **Pipeline ausführen**:
   ```bash
   tkn pipeline start simple-pipeline
   ```

## Lokale Tests und Simulation von Deployment-Pipelines

### Testing mit CRC
CRC bietet die Möglichkeit, CI/CD-Pipelines lokal zu testen, bevor sie in Produktionsumgebungen bereitgestellt werden.
- **Simulieren Sie Git-Triggers**: Verwenden Sie Webhooks, um CI/CD-Pipelines auszulösen.
- **Testen Sie verschiedene Deployment-Szenarien**: Führen Sie Rollouts und Blue-Green-Deployments in der lokalen Umgebung durch.

### Vorteile der lokalen Tests:
- Schnellere Entwicklungszyklen.
- Geringere Kosten durch die Nutzung lokaler Ressourcen.
- Frühzeitiges Erkennen von Problemen.

### Beispiel: Lokaler Test einer Tekton-Pipeline
1. Starten Sie die Pipeline:
   ```bash
   tkn pipeline start simple-pipeline
   ```
2. Überwachen Sie den Fortschritt:
   ```bash
   tkn pipelinerun logs
   ```

3. Testen Sie Rollback-Szenarien:
   - Rollbacks können simuliert werden, indem fehlerhafte Images absichtlich bereitgestellt werden.
