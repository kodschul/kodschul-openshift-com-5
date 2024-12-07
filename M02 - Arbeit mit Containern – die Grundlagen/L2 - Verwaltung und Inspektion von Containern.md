
# Verwaltung und Inspektion von Containern
## Container-Management: Prozesse, Speicher und Netzwerke verwalten

### Prozesse in Containern verwalten
Docker bietet Tools, um die Prozesse in einem Container zu überwachen und zu steuern:
- **`docker top`**: Zeigt die aktiven Prozesse in einem Container.
   ```bash
   docker top <container_name>
   ```
- **`docker exec`**: Führt einen Befehl in einem laufenden Container aus.
   ```bash
   docker exec -it <container_name> bash
   ```

### Speicherverwaltung in Containern
1. **Speicher überprüfen**:
   Nutzen Sie `docker stats`, um die Speicherauslastung eines Containers zu überwachen.
   ```bash
   docker stats <container_name>
   ```

2. **Volume Management**:
   Docker-Volumes sind persistent und können mit Containern geteilt werden:
   - Volume erstellen:
     ```bash
     docker volume create <volume_name>
     ```
   - Volume an einen Container anhängen:
     ```bash
     docker run -v <volume_name>:/path/in/container <image_name>
     ```

### Netzwerkverwaltung
Docker bietet verschiedene Netzwerkmodi für Container:
- **Bridge**: Standardnetzwerk, das Container miteinander und mit dem Host verbindet.
- **Host**: Container verwendet das Host-Netzwerk direkt.
- **None**: Der Container hat kein Netzwerk.

#### Befehle zur Netzwerkverwaltung:
- Netzwerke auflisten:
  ```bash
  docker network ls
  ```
- Netzwerk erstellen:
  ```bash
  docker network create <network_name>
  ```
- Container zu einem Netzwerk hinzufügen:
  ```bash
  docker network connect <network_name> <container_name>
  ```

## Container inspizieren, Logs überwachen und Ressourcen nutzen

### Container inspizieren
Mit dem Befehl `docker inspect` können Sie detaillierte Informationen über einen Container abrufen:
```bash
docker inspect <container_name>
```
Dies zeigt JSON-Ausgabe mit Informationen wie:
- Container-ID
- Netzwerkkonfiguration
- Mount-Punkte

### Logs überwachen
Docker-Logs ermöglichen es, die Ausgabe eines Containers zu überwachen:
- **Standard-Logs anzeigen**:
  ```bash
  docker logs <container_name>
  ```
- **Echtzeit-Logs streamen**:
  ```bash
  docker logs -f <container_name>
  ```
- **Logs nach Zeit filtern**:
  ```bash
  docker logs --since 1h <container_name>
  ```

### Ressourcennutzung überwachen
Verwenden Sie `docker stats`, um die Ressourcennutzung (CPU, Speicher, Netzwerk) zu überwachen:
```bash
docker stats
```

### Ressourcenlimits setzen
Docker erlaubt das Setzen von CPU- und Speicherlimits:
- **CPU-Limits setzen**:
  ```bash
  docker run --cpus="1.5" <image_name>
  ```
- **Speicherlimits setzen**:
  ```bash
  docker run --memory="512m" <image_name>
  ```

## Praktische Beispiele

### Beispiel 1: Einen Container inspizieren und Netzwerkdetails prüfen
1. Starten Sie einen Container:
   ```bash
   docker run -d --name my_container nginx
   ```
2. Inspizieren Sie den Container:
   ```bash
   docker inspect my_container
   ```
3. Extrahieren Sie die Netzwerkinformationen:
   ```bash
   docker inspect --format='{{.NetworkSettings.IPAddress}}' my_container
   ```

### Beispiel 2: Logs überwachen und Fehler analysieren
1. Starten Sie einen Container:
   ```bash
   docker run -d --name my_app my_image
   ```
2. Überwachen Sie die Logs in Echtzeit:
   ```bash
   docker logs -f my_app
   ```

### Beispiel 3: Ressourcennutzung optimieren
1. Starten Sie einen Container mit CPU- und Speicherlimits:
   ```bash
   docker run --cpus="1" --memory="256m" <image_name>
   ```
2. Überwachen Sie die Ressourcennutzung:
   ```bash
   docker stats
   ```