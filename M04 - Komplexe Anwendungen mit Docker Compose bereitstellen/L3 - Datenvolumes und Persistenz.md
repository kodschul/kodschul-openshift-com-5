
# Datenvolumes und Persistenz
## Was sind Docker-Volumes?

Docker-Volumes sind eine der besten Methoden, um persistente Daten in Docker zu speichern. Im Gegensatz zu bind mounts, die an einen spezifischen Ort im Host-Dateisystem gebunden sind, werden Volumes von Docker verwaltet und bieten eine einfachere und sicherere Handhabung.

### Vorteile von Volumes:
- **Persistenz**: Daten bleiben auch dann bestehen, wenn der Container gelöscht wird.
- **Portabilität**: Volumes können einfach zwischen Containern geteilt werden.
- **Unabhängigkeit vom Host-Dateisystem**: Docker verwaltet die Volumes, wodurch Plattformunterschiede abstrahiert werden.
- **Backup und Wiederherstellung**: Volumes können leicht gesichert und wiederhergestellt werden.

## Volumes erstellen und persistente Daten speichern

### 1. Erstellen eines Volumes
Mit dem Befehl `docker volume create` kann ein neues Volume erstellt werden:
```bash
docker volume create my_volume
```

Überprüfen Sie die erstellten Volumes:
```bash
docker volume ls
```

### 2. Verwenden eines Volumes in einem Container
Binden Sie das Volume an einen Pfad im Container, um Daten zu speichern:
```bash
docker run -d --name my_container -v my_volume:/data alpine
```

In diesem Beispiel:
- **`-v my_volume:/data`**: Bindet das Volume `my_volume` an den Pfad `/data` im Container.
- **`alpine`**: Ein leichter Container, der für Tests verwendet wird.

### 3. Schreiben und Lesen von Daten
Speichern Sie Daten im Container:
```bash
docker exec my_container sh -c "echo 'Persistente Daten' > /data/test.txt"
```

Überprüfen Sie die gespeicherten Daten:
```bash
docker exec my_container cat /data/test.txt
```

### 4. Persistenz nach Container-Löschung
Löschen Sie den Container:
```bash
docker rm -f my_container
```

Starten Sie einen neuen Container mit dem gleichen Volume:
```bash
docker run -d --name new_container -v my_volume:/data alpine
docker exec new_container cat /data/test.txt
```

Die gespeicherten Daten sind weiterhin verfügbar.

### 5. Volume entfernen
Wenn ein Volume nicht mehr benötigt wird, können Sie es entfernen:
```bash
docker volume rm my_volume
```

> **Achtung**: Entfernen Sie ein Volume nur, wenn Sie sicher sind, dass die Daten nicht mehr benötigt werden.

## Fortgeschrittene Techniken mit Volumes

### Daten zwischen mehreren Containern teilen
Sie können ein Volume mit mehreren Containern teilen, um Daten auszutauschen:
```bash
docker run -d --name container1 -v shared_volume:/data alpine
docker run -d --name container2 -v shared_volume:/data alpine
docker exec container1 sh -c "echo 'Daten von Container1' > /data/shared.txt"
docker exec container2 cat /data/shared.txt
```

### Backups und Wiederherstellung von Volumes
Um Daten aus einem Volume zu sichern:
```bash
docker run --rm -v my_volume:/data -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz -C /data .
```

Zur Wiederherstellung der Daten:
```bash
docker run --rm -v my_volume:/data -v $(pwd):/backup alpine tar xzf /backup/backup.tar.gz -C /data
```
