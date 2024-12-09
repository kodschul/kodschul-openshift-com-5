
# Grundlagen der Speicherverwaltung
## Einfache Volumes und persistente Volumes erstellen und verwenden

### Was sind Volumes in Kubernetes?
In Kubernetes bieten Volumes eine Möglichkeit, Daten in Containern über deren Lebensdauer hinaus zu speichern. Sie werden an Pods gebunden und können für verschiedene Container im Pod freigegeben werden.

### Einfache Volumes
Ein einfaches Volume wird direkt im Pod-Manifest definiert. Es ist nicht persistent und wird gelöscht, wenn der Pod entfernt wird.

#### Beispiel:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-volume-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: simple-volume
  volumes:
  - name: simple-volume
    emptyDir: {}
```
In diesem Beispiel wird ein `emptyDir`-Volume verwendet, das für temporäre Daten innerhalb des Pods geeignet ist.

### Persistente Volumes (PV) und Persistente Volume Claims (PVC)
Persistente Volumes sind von Pods entkoppelt und bieten eine langlebige Speicherlösung.

#### Komponenten:
1. **Persistentes Volume (PV)**: Eine physische Speicherressource, die im Cluster verfügbar ist.
2. **Persistenter Volume Claim (PVC)**: Eine Anfrage von einem Pod nach Speicherplatz.

#### Beispiel:
1. **Erstellen eines PV**:
   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: example-pv
   spec:
     capacity:
       storage: 1Gi
     accessModes:
       - ReadWriteOnce
     hostPath:
       path: "/mnt/data"
   ```

2. **Erstellen eines PVC**:
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
         storage: 500Mi
   ```

3. **Verwendung des PVC in einem Pod**:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pvc-pod
   spec:
     containers:
     - name: app
       image: nginx
       volumeMounts:
       - mountPath: "/usr/share/nginx/html"
         name: storage-volume
     volumes:
     - name: storage-volume
       persistentVolumeClaim:
         claimName: example-pvc
   ```

### Vorteile von persistenten Volumes:
- **Unabhängigkeit vom Pod-Lebenszyklus**: Die Daten bleiben auch nach dem Löschen des Pods erhalten.
- **Flexibilität**: Ermöglicht die Anbindung externer Speicherressourcen wie NFS, AWS EBS, oder Google Persistent Disk.

## Unterschiede und Anwendungsfälle von Volumetypen

Kubernetes unterstützt verschiedene Volumetypen, die jeweils für spezifische Anwendungsfälle geeignet sind.

### `emptyDir`
- **Beschreibung**: Temporäres Verzeichnis, das gelöscht wird, wenn der Pod entfernt wird.
- **Anwendungsfälle**: Zwischenspeicherung, temporäre Daten.

### `hostPath`
- **Beschreibung**: Bindet einen Pfad auf dem Node an den Pod.
- **Anwendungsfälle**: Zugriff auf Node-spezifische Daten wie Logs oder Konfigurationen.
- **Einschränkungen**: Nicht für Multi-Node-Cluster geeignet.

### `nfs`
- **Beschreibung**: Verbindet ein Network File System (NFS) mit dem Pod.
- **Anwendungsfälle**: Gemeinsame Daten zwischen mehreren Pods in einem Cluster.

### `persistentVolume`
- **Beschreibung**: Bietet persistenten Speicher, der von verschiedenen Providern unterstützt wird (z. B. AWS EBS, Azure Disk, Google PD).
- **Anwendungsfälle**: Datenbankanwendungen, die persistenten Speicher benötigen.

### `configMap` und `secret`
- **Beschreibung**: Speichern Konfigurationsdaten bzw. sensible Informationen.
- **Anwendungsfälle**: Verwaltung von Umgebungsvariablen, Zertifikaten, Passwörtern.

### `csi` (Container Storage Interface)
- **Beschreibung**: Ermöglicht die Integration von Drittanbieter-Speicherlösungen.
- **Anwendungsfälle**: Erweiterte Speicherlösungen und dynamische Bereitstellung.
