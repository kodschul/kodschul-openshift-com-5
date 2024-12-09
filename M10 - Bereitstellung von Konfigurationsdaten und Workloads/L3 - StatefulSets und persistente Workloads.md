
# StatefulSets und persistente Workloads
## Was sind StatefulSets?

StatefulSets sind Kubernetes-Objekte, die sicherstellen, dass Pods einer Anwendung mit stabilen Identitäten und persistentem Speicher verwaltet werden. Im Gegensatz zu Deployments, die sich auf stateless Anwendungen konzentrieren, sind StatefulSets für stateful Workloads konzipiert.

### Eigenschaften von StatefulSets:
1. **Stabile Netzwerk-Identität**:
   Jeder Pod in einem StatefulSet erhält einen stabilen Namen, z. B. `my-app-0`, `my-app-1`.

2. **Persistente Speicherung**:
   Volumes, die mit Pods verbunden sind, bleiben erhalten, auch wenn der Pod gelöscht oder neu erstellt wird.

3. **Geordnete Bereitstellung und Skalierung**:
   Pods werden nacheinander erstellt oder gelöscht, um die Konsistenz sicherzustellen.

4. **Geordnete Updates**:
   Pods werden nacheinander aktualisiert, was bei stateful Workloads wichtig ist.

### Einsatzbereiche von StatefulSets:
- Datenbanken (z. B. MySQL, PostgreSQL)
- Verteilte Dateisysteme (z. B. HDFS)
- Caches (z. B. Redis, Memcached)
- Anwendungen mit spezifischen Konfigurationsanforderungen

## Erstellen eines StatefulSets

Ein StatefulSet besteht aus:
- **Einheitlichen Pods**: Jeder Pod hat dieselbe Spezifikation, aber eine eindeutige Identität.
- **Persistenten Volumes**: Werden verwendet, um Daten zu speichern, die über Neustarts hinaus bestehen bleiben.

### Beispielkonfiguration:
Erstellen Sie eine YAML-Datei für ein StatefulSet:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-app
spec:
  serviceName: "my-service"
  replicas: 3
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
        image: my-image
        volumeMounts:
        - name: my-volume
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: my-volume
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

### Erklärung:
- **`serviceName`**: Verweist auf einen Headless-Service, der für die Netzwerk-Identität erforderlich ist.
- **`volumeClaimTemplates`**: Erstellt dynamisch persistente Volumes für jeden Pod.

### Bereitstellung des StatefulSets:
1. Erstellen Sie das StatefulSet:
   ```bash
   kubectl apply -f statefulset.yaml
   ```
2. Überprüfen Sie die Pods:
   ```bash
   kubectl get pods
   ```
   Sie sollten Pods mit stabilen Namen wie `my-app-0`, `my-app-1`, etc. sehen.

3. Überprüfen Sie die persistente Speicherung:
   ```bash
   kubectl get pvc
   ```

## Arbeiten mit persistenten Workloads

### Persistente Volumes (PV) und Persistent Volume Claims (PVC)
- **PV**: Repräsentiert den Speicher, der von Kubernetes verwaltet wird.
- **PVC**: Eine Anfrage von einem Pod, Speicher zu beanspruchen.

#### Beispiel eines Persistent Volume:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

#### Beispiel eines Persistent Volume Claim:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### Dynamische Speicherung mit StorageClasses
StorageClasses ermöglichen die automatische Bereitstellung von persistentem Speicher.
#### Beispiel:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```

## Best Practices für StatefulSets und persistente Workloads

1. **Headless Services verwenden**:
   - Erforderlich, um stabile DNS-Namen für StatefulSet-Pods bereitzustellen.
   - Beispiel:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       clusterIP: None
       selector:
         app: my-app
     ```

2. **Datenintegrität sicherstellen**:
   - Verwenden Sie Volumes mit geeigneten Zugriffsmodi (`ReadWriteOnce`, `ReadWriteMany`).

3. **Pod-Ordnung beibehalten**:
   - Halten Sie die Reihenfolge der Pods ein, insbesondere bei verteilten Systemen.

4. **Skalieren mit Vorsicht**:
   - Stellen Sie sicher, dass neue Pods korrekt initialisiert werden, bevor sie aktiv genutzt werden.
