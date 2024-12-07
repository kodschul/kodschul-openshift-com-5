
# Vergleich zur Virtualisierung
## Unterschiede zwischen Containern und virtuellen Maschinen

Container und virtuelle Maschinen (VMs) sind Technologien zur Isolation von Anwendungen, die jedoch auf unterschiedlichen Konzepten basieren.

### Virtuelle Maschinen
- **Architektur**: Jede VM enthält ein vollständiges Betriebssystem, eine virtuelle Kopie der Hardware und Anwendungen.
- **Ressourcenverbrauch**: VMs sind ressourcenintensiv, da sie komplette Betriebssysteme laden.
- **Startzeit**: VMs benötigen mehr Zeit zum Starten, da das gesamte Betriebssystem initialisiert wird.
- **Flexibilität**: Bieten vollständige Isolierung, da jede VM eine eigene Kernel-Instanz verwendet.

Beispiel für eine typische VM-Umgebung:
```
Hypervisor
├── VM 1 (OS + App)
├── VM 2 (OS + App)
└── VM 3 (OS + App)
```

### Container
- **Architektur**: Container teilen sich denselben Betriebssystem-Kernel, isolieren jedoch Anwendungen und deren Abhängigkeiten.
- **Ressourcenverbrauch**: Container sind leichtgewichtig, da sie kein eigenes Betriebssystem benötigen.
- **Startzeit**: Container starten nahezu sofort, da nur die Anwendung und deren Abhängigkeiten geladen werden.
- **Flexibilität**: Container sind portabler, da sie weniger Ressourcen benötigen und plattformunabhängig sind.

Beispiel für eine Container-Umgebung:
```
Docker Engine
├── Container 1 (App + Abhängigkeiten)
├── Container 2 (App + Abhängigkeiten)
└── Container 3 (App + Abhängigkeiten)
```

### Vergleichstabelle
| Merkmal                 | Virtuelle Maschinen       | Container             |
|-------------------------|---------------------------|-----------------------|
| Betriebssystem          | Eigenes OS pro VM         | Gemeinsamer Kernel    |
| Startzeit               | Minuten                   | Sekunden              |
| Ressourcenverbrauch     | Hoch                      | Gering                |
| Portabilität            | Eingeschränkt             | Hoch                  |
| Isolierung              | Vollständig               | Betriebssystem-Level  |

## Container-Layering und Speicher-Management

Docker verwendet ein Schichtensystem (Layering), um Speicher effizient zu nutzen und die Wiederverwendbarkeit von Images zu ermöglichen.

### Wie funktioniert Container-Layering?
1. **Basis-Image**: Jedes Docker-Image beginnt mit einer Basisschicht, z. B. einem Betriebssystem wie `ubuntu`.
2. **Read-Only-Schichten**: Weitere Schichten enthalten Änderungen oder neue Dateien, die durch Docker-Befehle wie `RUN`, `ADD` oder `COPY` hinzugefügt werden.
3. **Writable-Schicht**: Jeder laufende Container hat eine zusätzliche Schreib-Schicht (Writable Layer), in der Änderungen vorgenommen werden.

Beispiel:
```
Base Layer (Ubuntu)
├── Layer 1: Python installiert (RUN apt-get install python)
├── Layer 2: Anwendungscode kopiert (COPY . /app)
└── Writable Layer: Laufzeitdaten des Containers
```

### Vorteile des Layering
- **Effizienz**: Gemeinsame Schichten werden zwischen Containern wiederverwendet, was Speicher spart.
- **Portabilität**: Änderungen an einem Container erfordern nicht den Neuaufbau des gesamten Images.
- **Caching**: Docker speichert Schichten im Cache, um Builds zu beschleunigen.

### Speicher-Management in Docker
Docker verwendet verschiedene Speichertreiber, um Container-Daten zu verwalten:
1. **OverlayFS**: Kombiniert mehrere Schichten zu einem einheitlichen Dateisystem.
2. **aufs**: Unterstützt Layering, ist jedoch weniger verbreitet.
3. **Device Mapper**: Nutzt Block-Level-Speicher für die Verwaltung.

#### Persistente Daten
- Daten, die nach dem Neustart eines Containers erhalten bleiben sollen, werden in **Volumes** gespeichert.
- **Bind Mounts** und **Volumes**:
  - Bind Mounts: Verknüpfen Container mit einem Host-Verzeichnis.
  - Volumes: Von Docker verwaltete Speicherorte.

Beispiel für ein Volume:
```bash
docker volume create app-data
docker run -v app-data:/app/data my-container
```
