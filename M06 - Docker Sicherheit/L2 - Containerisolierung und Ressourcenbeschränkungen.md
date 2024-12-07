
# Containerisolierung und Ressourcenbeschränkungen
## Containerisolierung

Containerisolierung ist ein zentraler Aspekt von Docker. Jeder Container wird in einer eigenen, isolierten Umgebung ausgeführt. Diese Isolierung wird durch die Kombination verschiedener Kernel-Technologien erreicht.

### Wichtige Konzepte der Isolierung

1. **Namespaces**:
   - Namespaces isolieren die Sichtbarkeit von Ressourcen zwischen Containern.
   - Beispiele für Namespaces:
     - `pid`: Trennt die Prozess-IDs.
     - `net`: Isoliert Netzwerkschnittstellen.
     - `mnt`: Isoliert das Dateisystem.
     - `ipc`: Isoliert die Interprozesskommunikation.

   Beispiel: Überprüfung des Hostnames innerhalb eines Containers:
   ```bash
   docker run --rm alpine sh -c "echo Hostname: $(hostname)"
   ```

2. **Control Groups (cgroups)**:
   - Ermöglichen die Beschränkung und Überwachung der Ressourcen (CPU, Speicher, I/O) eines Containers.
   - Wird oft zusammen mit Namespaces verwendet, um eine vollständige Isolierung zu erreichen.

3. **Union File System**:
   - Docker verwendet ein Layer-basiertes Dateisystem, das die Dateisysteme von Containern voneinander trennt.

### Vorteile der Isolierung
- **Sicherheit**: Anwendungen laufen in getrennten Umgebungen, wodurch die Auswirkungen potenzieller Sicherheitslücken minimiert werden.
- **Flexibilität**: Mehrere Container können auf demselben Host ausgeführt werden, ohne sich gegenseitig zu beeinflussen.
- **Stabilität**: Fehler in einem Container haben keine Auswirkungen auf andere Container.

## Ressourcenbeschränkungen

Docker ermöglicht es, die Ressourcen eines Containers zu begrenzen, um sicherzustellen, dass ein Container nicht den gesamten Host belegt.

### CPU-Beschränkungen

1. **CPU-Anteil (`--cpu-shares`)**:
   - Legt den relativen Anteil an CPU-Zeit fest.
   ```bash
   docker run --rm --cpu-shares 512 alpine top
   ```

2. **CPU-Auslastung begrenzen (`--cpus`)**:
   - Beschränkt die CPU-Auslastung eines Containers.
   ```bash
   docker run --rm --cpus="1.5" alpine top
   ```

3. **CPU-Bindung (`--cpuset-cpus`)**:
   - Legt fest, welche CPU-Kerne ein Container verwenden darf.
   ```bash
   docker run --rm --cpuset-cpus="0,1" alpine top
   ```

### Speicherbeschränkungen

1. **Maximaler Arbeitsspeicher (`--memory`)**:
   - Legt die maximale Menge an RAM fest, die ein Container verwenden darf.
   ```bash
   docker run --rm --memory="256m" alpine top
   ```

2. **Swap-Speicherlimit (`--memory-swap`)**:
   - Legt den gesamten Speicher (RAM + Swap) fest.
   ```bash
   docker run --rm --memory="256m" --memory-swap="512m" alpine top
   ```

3. **Speicherreservierung (`--memory-reservation`)**:
   - Reserviert Speicher für den Container, der bei Ressourcenknappheit bevorzugt behandelt wird.
   ```bash
   docker run --rm --memory-reservation="128m" alpine top
   ```

### I/O-Beschränkungen

1. **Maximale I/O-Rate (`--blkio-weight`)**:
   - Steuert den Zugriff auf Blockgeräte.
   ```bash
   docker run --rm --blkio-weight=500 alpine dd if=/dev/zero of=/dev/null
   ```

2. **Maximale Lese-/Schreibrate (`--device-read-bps`, `--device-write-bps`)**:
   - Begrenzt die Lese-/Schreibrate von Geräten.
   ```bash
   docker run --rm --device-read-bps=/dev/sda:1mb alpine dd if=/dev/sda of=/dev/null
   ```

## Praktische Übung

### Aufgabe 1: Erstellen eines Containers mit Ressourcenbeschränkungen
- Starten Sie einen Container mit 1 CPU und 256 MB Arbeitsspeicher:
  ```bash
  docker run --rm --cpus="1" --memory="256m" alpine top
  ```

### Aufgabe 2: Überprüfung der Ressourcenbeschränkungen
- Verwenden Sie den Befehl `docker stats`, um die Ressourcennutzung des Containers zu überwachen:
  ```bash
  docker stats
  ```

### Aufgabe 3: Test der CPU-Beschränkungen
- Starten Sie zwei Container mit unterschiedlichen `--cpu-shares` und beobachten Sie, wie sich die CPU-Zeit verteilt:
  ```bash
  docker run --rm --cpu-shares=512 alpine top
  docker run --rm --cpu-shares=1024 alpine top
  ```
