
# Aufbau und Speichern von Images

In dieser Schulung wird das Verständnis von Container-Images und deren Speicherstruktur behandelt. Sie lernen, wie das Union Filesystem funktioniert und wie die Layer-Architektur von Docker-Images aufgebaut ist.

## Container-Images und das Union Filesystem verstehen

Ein Docker-Image ist ein unveränderliches, leichtgewichtiges und ausführbares Paket, das alles enthält, was für die Ausführung einer Anwendung erforderlich ist:
- Quellcode
- Bibliotheken und Abhängigkeiten
- Laufzeitumgebung

### Das Union Filesystem
Docker verwendet ein Union Filesystem (auch als Overlay Filesystem bekannt), das mehrere Dateisysteme übereinanderlegt. Dadurch wird das effiziente Speichern und Teilen von Daten zwischen Containern ermöglicht.

#### Vorteile des Union Filesystems:
- **Wiederverwendbarkeit**: Gemeinsame Layer können von mehreren Images verwendet werden.
- **Speichereffizienz**: Nur Änderungen werden gespeichert, wodurch der Speicherbedarf reduziert wird.
- **Geschwindigkeit**: Der Aufbau eines Images ist schneller, da nur neue oder geänderte Layer erstellt werden.

#### Layer in einem Union Filesystem:
- **Basislayer**: Enthält das Grundsystem (z. B. ein minimales Linux).
- **Änderungslayer**: Jede Änderung (z. B. Installation eines Pakets) wird in einem neuen Layer gespeichert.

Beispiel:
```bash
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y python3
COPY app.py /app/app.py
CMD ["python3", "/app/app.py"]
```
In diesem Dockerfile:
1. Der Basislayer ist `ubuntu:20.04`.
2. Der erste Änderungslayer installiert Python3.
3. Der nächste Layer fügt `app.py` hinzu.

## Speicherstruktur und Layer eines Images

### Aufbau eines Images
Ein Docker-Image besteht aus mehreren Layern, die übereinander gestapelt sind. Diese Layer sind read-only. Wenn ein Container gestartet wird, wird ein schreibbarer Layer auf das Image angewendet.

#### Wichtige Konzepte:
- **Read-only Layer**: Bestehende Layer, die nicht verändert werden können.
- **Writable Layer**: Wird dem Container hinzugefügt, um Änderungen während der Laufzeit zu ermöglichen.

### Speicherung eines Images
Docker speichert Images und deren Layer im lokalen Speicher unter `/var/lib/docker`. Jeder Layer wird als einheitlicher Blob gespeichert und referenziert.

#### Layer-Architektur:
1. **Digest**: Ein eindeutiger Hash, der jeden Layer identifiziert.
2. **Manifest**: Enthält Metadaten über das Image und eine Liste aller Layer.

### Image-Caching
Docker verwendet ein Caching-System, um redundante Operationen zu vermeiden. Wenn ein Layer bereits vorhanden ist, wird dieser erneut verwendet.

Beispiel:
- Wenn Sie das folgende Dockerfile ändern:
  ```dockerfile
  FROM ubuntu:20.04
  RUN apt-get update && apt-get install -y python3
  ```
  und nur die `COPY`-Anweisung ändern, wird der `RUN`-Layer aus dem Cache verwendet.

### Wichtige Docker-Befehle
- **Image anzeigen**:
  ```bash
  docker images
  ```
- **Image-Details anzeigen**:
  ```bash
  docker inspect <image_id>
  ```
- **Verwendete Speichergröße anzeigen**:
  ```bash
  docker system df
  ```

### Image-Speicherung und Distribution
Docker-Images können in Container-Registries gespeichert und verteilt werden. Beispiele für Registries:
- **Docker Hub**: Die Standardregistry.
- **Private Registry**: Ermöglicht das Speichern von Images in einer privaten Umgebung.
- **AWS ECR oder Azure ACR**: Cloudbasierte Registries.

Speichern eines Images in einer Registry:
1. Erstellen Sie ein Repository auf Docker Hub.
2. Taggen Sie das Image:
   ```bash
   docker tag <image_id> username/repository:tag
   ```
3. Pushen Sie das Image:
   ```bash
   docker push username/repository:tag
   ```