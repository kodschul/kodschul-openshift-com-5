
# Storage und Persistenz in CRC
## Storage-Optionen in CRC: Möglichkeiten und Einschränkungen

### Möglichkeiten von Storage in CRC
CodeReady Containers (CRC) bietet eine leichtgewichtige OpenShift-Umgebung für lokale Entwicklungs- und Testzwecke. CRC unterstützt verschiedene Speicheroptionen, die sich jedoch von einer produktiven OpenShift-Umgebung unterscheiden.

#### Unterstützte Storage-Typen:
1. **EmptyDir**: Temporärer Speicher, der mit dem Lebenszyklus des Pods gelöscht wird.
2. **HostPath**: Bindet ein Verzeichnis des Host-Systems in den Pod ein.
3. **Persistent Volumes (PV)**: Werden für persistente Daten verwendet und erfordern Persistent Volume Claims (PVC).
4. **Ephemeral Volumes**: Für temporäre, nicht persistente Daten.

#### Einschränkungen:
- CRC ist für lokale Entwicklungsumgebungen gedacht und hat daher eingeschränkte Storage-Optionen.
- Keine direkte Integration in externe Storage-Systeme wie Ceph, GlusterFS oder AWS EBS.
- Performance ist begrenzt, da CRC auf einer lokalen VM läuft.

### Einsatzmöglichkeiten von Storage:
- **Entwicklung und Tests**: Persistente Daten für Testdatenbanken oder Konfigurationsdateien.
- **Simulierung produktiver Umgebungen**: Testen von Anwendungen, die Persistenz erfordern, bevor sie in die Cloud migriert werden.

## Einrichtung und Nutzung von Persistent Volumes in CRC

### Was sind Persistent Volumes (PV) und Persistent Volume Claims (PVC)?
- **Persistent Volume (PV)**: Eine Speicherressource, die von der OpenShift-Umgebung bereitgestellt wird.
- **Persistent Volume Claim (PVC)**: Eine Anfrage, mit der ein Pod auf ein PV zugreift.

### Schritte zur Einrichtung:

#### 1. Erstellen eines Persistent Volumes (PV)
Erstellen Sie eine YAML-Datei zur Definition eines Persistent Volumes:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```
Anwenden der Konfiguration:
```bash
kubectl apply -f pv.yaml
```

#### 2. Erstellen eines Persistent Volume Claims (PVC)
Definieren Sie einen PVC, um Speicher aus dem PV zu beanspruchen:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
Anwenden der Konfiguration:
```bash
kubectl apply -f pvc.yaml
```

#### 3. Verwendung des PVC in einem Pod
Binden Sie den PVC in einen Pod ein:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: storage-test
spec:
  containers:
  - name: test-container
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: storage-volume
  volumes:
  - name: storage-volume
    persistentVolumeClaim:
      claimName: example-pvc
```
Anwenden der Konfiguration:
```bash
kubectl apply -f pod.yaml
```

### Überprüfung:
1. Überprüfen Sie den Status des PV und PVC:
   ```bash
   kubectl get pv
   kubectl get pvc
   ```
2. Überprüfen Sie, ob der Pod den Speicher erfolgreich verwendet:
   ```bash
   kubectl get pods
   kubectl logs storage-test
   ```

### Dynamische Speicherbereitstellung
In produktiven OpenShift-Umgebungen wird dynamischer Speicher oft mit StorageClasses verwaltet. In CRC ist dies jedoch eingeschränkt.

Beispiel einer StorageClass:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: Immediate
```

## Best Practices für Storage in CRC

1. **Limitierung der Datenmenge**:
   - CRC ist nicht für große Datenmengen ausgelegt. Beschränken Sie Speicheranforderungen auf Entwicklungs- und Testzwecke.

2. **HostPath mit Vorsicht verwenden**:
   - HostPath bindet Verzeichnisse direkt vom Host und sollte nur für nicht-kritische Workloads verwendet werden.

3. **Regelmäßige Backups**:
   - Sichern Sie wichtige Daten regelmäßig, da CRC keine hochverfügbare Umgebung bietet.

4. **Speicherplatz überwachen**:
   - CRC-VMs haben begrenzten Speicherplatz. Nutzen Sie Tools wie `df -h`, um die Speichernutzung zu überwachen.
