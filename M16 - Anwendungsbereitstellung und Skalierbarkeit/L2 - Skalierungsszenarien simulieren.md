
# Skalierungsszenarien simulieren
## Auto-Skalierung in CRC: Lokale Tests und Simulationen

### Was ist CRC (CodeReady Containers)?
CodeReady Containers (CRC) ist eine lokale OpenShift-Umgebung, die Entwickler nutzen können, um OpenShift-Cluster auf ihren lokalen Maschinen zu simulieren. Es ist ideal für Tests und Entwicklung, einschließlich der Simulation von Skalierungsszenarien.

### Voraussetzungen:
- Installierte CRC-Umgebung ([Installationsanleitung](https://crc.dev)).
- OpenShift CLI (`oc`) installiert und konfiguriert.

### Auto-Skalierung simulieren:
In OpenShift wird Auto-Skalierung durch die Verwendung von Horizontal Pod Autoscalern (HPA) ermöglicht.

#### 1. Horizontal Pod Autoscaler (HPA) aktivieren
Der HPA passt die Anzahl der Pods basierend auf CPU- und Speicherauslastung an.

**Beispielkonfiguration:**
```bash
# Erstellen Sie ein Deployment
oc create deployment autoscale-example --image=nginx

# Setzen Sie Ressourcenanforderungen für das Deployment
oc set resources deployment autoscale-example --limits=cpu=200m --requests=cpu=100m

# Erstellen Sie einen Horizontal Pod Autoscaler
oc autoscale deployment autoscale-example --min=1 --max=5 --cpu-percent=50
```

#### 2. Last simulieren
Nutzen Sie `kubectl` oder ein Lasttest-Tool wie `hey`, um CPU-Last zu generieren:
```bash
kubectl run -i --tty load-generator --image=busybox -- /bin/sh -c "while true; do wget -q -O- http://autoscale-example; done"
```

#### 3. Überprüfen der Skalierung
Überwachen Sie die Skalierung des Deployments:
```bash
oc get hpa
oc get pods
```

### Manuelles Skalieren in CRC
Simulieren Sie manuelle Skalierung, indem Sie die Anzahl der Pods für ein Deployment oder StatefulSet ändern:
```bash
oc scale deployment autoscale-example --replicas=3
oc get pods
```

## Skalierung von Deployments und StatefulSets

### Skalierung von Deployments
Deployments sind die häufigste Ressource für skalierbare Anwendungen. Sie bieten einfache Skalierungsoptionen und sind ideal für stateless Anwendungen.

#### Beispiel:
1. Erstellen eines Deployments:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-deployment
   spec:
     replicas: 2
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
           image: nginx
           resources:
             requests:
               cpu: "100m"
             limits:
               cpu: "200m"
   ```
2. Bereitstellen des Deployments:
   ```bash
   oc apply -f deployment.yaml
   ```

3. Skalieren des Deployments:
   ```bash
   oc scale deployment my-deployment --replicas=5
   oc get pods
   ```

### Skalierung von StatefulSets
StatefulSets sind für stateful Anwendungen ausgelegt und bieten stabile Netzwerk-Identitäten und persistente Speicher. Die Skalierung erfordert zusätzliche Überlegungen, insbesondere bei speicherintensiven Anwendungen.

#### Beispiel:
1. Erstellen eines StatefulSets:
   ```yaml
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: my-statefulset
   spec:
     serviceName: "my-service"
     replicas: 2
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
           image: nginx
           volumeMounts:
           - name: data
             mountPath: /data
     volumeClaimTemplates:
     - metadata:
         name: data
       spec:
         accessModes: ["ReadWriteOnce"]
         resources:
           requests:
             storage: 1Gi
   ```

2. Bereitstellen des StatefulSets:
   ```bash
   oc apply -f statefulset.yaml
   ```

3. Skalieren des StatefulSets:
   ```bash
   oc scale statefulset my-statefulset --replicas=3
   oc get pods
   ```

4. Überprüfen der persistierten Volumes:
   ```bash
   oc get pvc
   ```

## Best Practices für Skalierungsszenarien

1. **Ressourcenanforderungen definieren**:
   - Setzen Sie realistische CPU- und Speichervorgaben, um die Auto-Skalierung effizient zu nutzen.

2. **Monitoring und Alerts**:
   - Verwenden Sie Tools wie Prometheus und Grafana, um die Ressourcennutzung zu überwachen und auf Skalierungsereignisse zu reagieren.

3. **Cluster-Kapazität berücksichtigen**:
   - Stellen Sie sicher, dass der Cluster genügend Ressourcen bereitstellt, um Skalierungsanforderungen zu erfüllen.

4. **Skalierung testen**:
   - Testen Sie Skalierungsszenarien in einer sicheren Umgebung wie CRC, bevor Sie Änderungen in der Produktion vornehmen.
