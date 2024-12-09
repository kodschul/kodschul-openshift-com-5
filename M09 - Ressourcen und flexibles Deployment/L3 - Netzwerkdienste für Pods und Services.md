
# Netzwerkdienste für Pods und Services
## Netzwerkgrundlagen in Kubernetes

Kubernetes implementiert ein Container-Netzwerkmodell, bei dem jedes Pod eine eigene IP-Adresse hat. Dies ermöglicht eine direkte Kommunikation zwischen Pods, unabhängig davon, auf welchem Knoten sie sich befinden.

### Grundlegende Konzepte:
1. **Pod**:
   - Die kleinste ausführbare Einheit in Kubernetes.
   - Jeder Pod erhält eine eigene IP-Adresse.
   - Pods kommunizieren über IP-Adressen oder DNS.

2. **Service**:
   - Ein Abstraktionsobjekt, das eine stabile IP-Adresse für eine Gruppe von Pods bereitstellt.
   - Ermöglicht die Lastverteilung und das Routing zu den Pods.

3. **Cluster Networking**:
   - Ermöglicht die Kommunikation zwischen Pods über verschiedene Knoten hinweg.
   - Erfordert ein Container-Netzwerk-Plugin (z. B. Calico, Flannel).

## Netzwerkdienste für Pods

### Kommunikation zwischen Pods
Pods können direkt über ihre IP-Adressen kommunizieren. Allerdings ist dies aufgrund der dynamischen Natur von Pods (z. B. bei Neustarts) selten praktikabel.

#### Verwendung eines Services:
Ein Service abstrahiert die Pods und bietet eine stabile IP-Adresse und DNS-Namen:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

- **`selector`**: Verbindet den Service mit den Pods, die das Label `app: my-app` haben.
- **`port`**: Der externe Port des Services.
- **`targetPort`**: Der Port, den der Pod hört.

### Service-Typen:
1. **ClusterIP** (Standard):
   - Der Service ist nur innerhalb des Clusters erreichbar.
   - Verwendung: Interne Kommunikation zwischen Anwendungen.

2. **NodePort**:
   - Öffnet einen Port auf jedem Cluster-Knoten.
   - Verwendung: Ermöglicht den Zugriff von außerhalb des Clusters.

3. **LoadBalancer**:
   - Erstellt einen externen Load Balancer (meist in Cloud-Umgebungen verfügbar).
   - Verwendung: Externer Zugriff auf Anwendungen.

4. **ExternalName**:
   - Mappt den Service auf einen externen DNS-Namen.
   - Verwendung: Verknüpfung mit externen Diensten.

## DNS in Kubernetes

Kubernetes verwendet ein integriertes DNS-System, um die Auflösung von Service-Namen zu erleichtern.

### Beispiel:
Ein Service mit dem Namen `my-service` im Namespace `default` kann über `my-service.default.svc.cluster.local` aufgelöst werden.

- **Namespace-Isolation**: Jeder Namespace hat seine eigene DNS-Domain.
- **Kurzform**: Innerhalb desselben Namespaces reicht `my-service`.

### Praktische Übung:
1. Erstellen Sie einen Pod und Service:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
     labels:
       app: my-app
   spec:
     containers:
       - name: my-container
         image: nginx
         ports:
           - containerPort: 80
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     selector:
       app: my-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
   ```

2. Testen Sie die DNS-Auflösung:
   - Öffnen Sie eine Shell in einem beliebigen Pod.
   - Führen Sie aus:
     ```bash
     nslookup my-service
     ```

## Netzwerk-Plugins

Kubernetes erfordert ein CNI (Container Network Interface)-Plugin, um Netzwerkfunktionen bereitzustellen. Beliebte Optionen sind:
1. **Calico**: Netzwerk-Sicherheitsrichtlinien und IP-Routing.
2. **Flannel**: Einfaches Overlay-Netzwerk.
3. **Weave Net**: Selbstheilendes Overlay-Netzwerk.

### Einrichtung eines Plugins (Beispiel mit Calico):
1. Laden Sie das Calico-Manifest herunter:
   ```bash
   kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
   ```

2. Überprüfen Sie die Installation:
   ```bash
   kubectl get pods -n kube-system
   ```

## Best Practices für Kubernetes-Netzwerke

1. **Trennung von Traffic**:
   - Verwenden Sie Namespaces zur Isolierung von Services.
   - Implementieren Sie Netzwerk-Richtlinien für Traffic-Kontrolle.

2. **Sicherheitsrichtlinien**:
   - Beschränken Sie den Zugriff auf Pods mit `NetworkPolicies`.
   - Blockieren Sie unnötigen Ingress- und Egress-Traffic.

3. **Monitoring**:
   - Verwenden Sie Tools wie Prometheus oder Grafana zur Überwachung von Netzwerkmetriken.

4. **Fehlertoleranz**:
   - Nutzen Sie mehrere Replicas für Pods, um hohe Verfügbarkeit zu gewährleisten.
