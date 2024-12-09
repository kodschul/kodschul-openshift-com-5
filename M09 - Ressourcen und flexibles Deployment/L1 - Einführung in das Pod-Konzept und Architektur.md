
# Einführung in Kubernetes Ressourcen
## Ressourcenarten in Kubernetes und ihre Funktion

Kubernetes-Ressourcen sind die Bausteine, mit denen Anwendungen in Kubernetes verwaltet werden. Jede Ressource hat eine spezifische Rolle und Funktion im Cluster.

### Wichtige Ressourcenarten

1. **Pods**
   - **Definition**: Die kleinste ausführbare Einheit in Kubernetes. Ein Pod enthält einen oder mehrere Container, die gemeinsam ausgeführt werden.
   - **Verwendung**: Pods dienen als Laufzeitumgebung für Anwendungen.
   - **Beispiel**:
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: my-pod
     spec:
       containers:
       - name: nginx
         image: nginx:latest
     ```

2. **Deployments**
   - **Definition**: Verwalten und steuern den Lebenszyklus von Pods.
   - **Verwendung**: Ermöglichen Rollouts, Updates und Rollbacks.
   - **Beispiel**:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: my-deployment
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: nginx
       template:
         metadata:
           labels:
             app: nginx
         spec:
           containers:
           - name: nginx
             image: nginx:latest
     ```

3. **Services**
   - **Definition**: Ermöglichen die Kommunikation zwischen verschiedenen Komponenten in einem Cluster.
   - **Verwendung**: Stellen eine stabile Netzwerkadresse für Pods bereit.
   - **Beispiel**:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       selector:
         app: nginx
       ports:
       - protocol: TCP
         port: 80
         targetPort: 80
     ```

4. **ConfigMaps und Secrets**
   - **Definition**: Ressourcen zur Verwaltung von Konfigurationsdaten.
   - **Verwendung**:
     - `ConfigMap`: Speicherung von Konfigurationsdaten.
     - `Secret`: Speicherung von sensiblen Daten wie Passwörtern.
   - **Beispiel ConfigMap**:
     ```yaml
     apiVersion: v1
     kind: ConfigMap
     metadata:
       name: my-config
     data:
       key: value
     ```
   - **Beispiel Secret**:
     ```yaml
     apiVersion: v1
     kind: Secret
     metadata:
       name: my-secret
     data:
       password: cGFzc3dvcmQ=
     ```

5. **Namespaces**
   - **Definition**: Logische Abgrenzungen im Cluster zur Isolierung von Ressourcen.
   - **Verwendung**: Organisieren von Ressourcen für verschiedene Teams oder Projekte.
   - **Beispiel**:
     ```yaml
     apiVersion: v1
     kind: Namespace
     metadata:
       name: my-namespace
     ```

6. **Ingress**
   - **Definition**: Ermöglicht externen Zugriff auf Dienste im Cluster.
   - **Verwendung**: Verwaltung von HTTP/HTTPS-Zugriffen.
   - **Beispiel**:
     ```yaml
     apiVersion: networking.k8s.io/v1
     kind: Ingress
     metadata:
       name: my-ingress
     spec:
       rules:
       - host: example.com
         http:
           paths:
           - path: /
             pathType: Prefix
             backend:
               service:
                 name: my-service
                 port:
                   number: 80
     ```

## Konzepte für effektives Ressourcenmanagement

Ein gutes Ressourcenmanagement ist entscheidend, um die Leistung und Zuverlässigkeit eines Kubernetes-Clusters sicherzustellen.

### 1. **Ressourcenanforderungen und -grenzen**
- **Definition**:
  - `requests`: Gibt an, wie viele Ressourcen ein Container mindestens benötigt.
  - `limits`: Gibt an, wie viele Ressourcen ein Container maximal verwenden darf.
- **Beispiel**:
  ```yaml
  resources:
    requests:
      memory: "64Mi"
      cpu: "250m"
    limits:
      memory: "128Mi"
      cpu: "500m"
  ```

### 2. **Autoscaling**
- **Horizontal Pod Autoscaler (HPA)**:
  - Skaliert die Anzahl der Pods basierend auf der CPU-Auslastung oder anderen Metriken.
  - **Beispiel**:
    ```yaml
    apiVersion: autoscaling/v2
    kind: HorizontalPodAutoscaler
    metadata:
      name: my-hpa
    spec:
      scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: my-deployment
      minReplicas: 1
      maxReplicas: 5
      metrics:
      - type: Resource
        resource:
          name: cpu
          target:
            type: Utilization
            averageUtilization: 50
    ```

### 3. **Namespaces und Quotas**
- **Ressourcenquoten**: Begrenzen die Nutzung von Ressourcen innerhalb eines Namespace.
- **Beispiel**:
  ```yaml
  apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: my-quota
    namespace: my-namespace
  spec:
    hard:
      pods: "10"
      requests.cpu: "4"
      requests.memory: "8Gi"
  ```

### 4. **Monitoring und Logging**
- **Tools**: Verwenden Sie Tools wie Prometheus, Grafana und Elasticsearch, um Metriken und Logs zu überwachen.
- **Best Practice**: Analysieren Sie die Ressourcennutzung regelmäßig und passen Sie Ressourcenanforderungen entsprechend an.
