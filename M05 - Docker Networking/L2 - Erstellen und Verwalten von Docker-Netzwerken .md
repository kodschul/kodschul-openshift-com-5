
# Erstellen und Verwalten von Docker-Netzwerken
## Grundlagen von Docker-Netzwerken

Docker stellt standardmäßig drei Netzwerkarten bereit:
1. **Bridge-Netzwerk**: Standardnetzwerktyp für Container, die auf demselben Host kommunizieren.
2. **Host-Netzwerk**: Teilt den Netzwerkstack des Hosts mit dem Container.
3. **None-Netzwerk**: Der Container hat keinen Netzwerkzugriff.

Zusätzlich können benutzerdefinierte Netzwerke erstellt werden, um mehr Kontrolle über die Netzwerkkommunikation zu erhalten.

### Standardnetzwerke überprüfen
Liste der vorhandenen Netzwerke anzeigen:
```bash
docker network ls
```

Beispielausgabe:
```
NETWORK ID     NAME      DRIVER    SCOPE
e8b6d46c3f1d   bridge    bridge    local
09ad06e2b123   host      host      local
b7f2f5c34d91   none      null      local
```

## Erstellen eines benutzerdefinierten Netzwerks

### Bridge-Netzwerk
Ein benutzerdefiniertes Bridge-Netzwerk ermöglicht die Kommunikation zwischen Containern mit einer isolierten Netzwerkumgebung.
```bash
docker network create my_bridge_network
```

Container zu diesem Netzwerk hinzufügen:
```bash
docker run -dit --name container1 --network my_bridge_network nginx
docker run -dit --name container2 --network my_bridge_network alpine
```

Testen Sie die Kommunikation:
```bash
docker exec container2 ping container1
```

### Overlay-Netzwerk
Overlay-Netzwerke werden für Multi-Host-Kommunikation verwendet (z. B. in Docker Swarm).
```bash
docker network create --driver overlay my_overlay_network
```

**Hinweis**: Overlay-Netzwerke erfordern die Aktivierung von Swarm-Mode:
```bash
docker swarm init
```

## Verwalten von Netzwerken

### Netzwerkinformationen anzeigen
Details zu einem bestimmten Netzwerk anzeigen:
```bash
docker network inspect my_bridge_network
```

Beispielausgabe:
```json
[
    {
        "Name": "my_bridge_network",
        "Driver": "bridge",
        "Containers": {
            "container1": {
                "IPv4Address": "172.18.0.2/16"
            }
        }
    }
]
```

### Netzwerk entfernen
Netzwerk löschen:
```bash
docker network rm my_bridge_network
```

### Container aus einem Netzwerk entfernen
Einen Container aus einem Netzwerk entfernen:
```bash
docker network disconnect my_bridge_network container1
```

Einen Container zu einem Netzwerk hinzufügen:
```bash
docker network connect my_bridge_network container1
```

## Best Practices für Docker-Netzwerke

1. **Trennung von Umgebungen**:
   - Verwenden Sie separate Netzwerke für unterschiedliche Anwendungen oder Umgebungen (z. B. Produktion und Entwicklung).

2. **Sicherheitsgruppen und Firewall-Regeln**:
   - Kontrollieren Sie den Zugriff auf Netzwerke mit Firewall-Regeln und Sicherheitsgruppen.

3. **DNS-Namen verwenden**:
   - Docker-Netzwerke bieten eine eingebaute DNS-Funktionalität. Verwenden Sie Containernamen, um die Kommunikation zu vereinfachen.

4. **Netzwerküberwachung**:
   - Überwachen Sie die Netzwerkaktivität mit Tools wie `docker stats` oder externen Monitoring-Tools.
