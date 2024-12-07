
# Kommunikation zwischen Containern
## Grundlagen der Kommunikation zwischen Containern

Docker verwendet ein internes Netzwerkmodell, um die Kommunikation zwischen Containern zu ermöglichen. Standardmäßig werden Container, die auf demselben Host laufen und demselben Netzwerk angehören, miteinander verbunden.

### Arten von Netzwerken in Docker:
1. **Bridge-Netzwerk** (Standard):
   - Container können innerhalb eines Bridge-Netzwerks miteinander kommunizieren.
   - Container verwenden ihre Namen als DNS-Einträge.

2. **Host-Netzwerk**:
   - Container verwenden das Netzwerk des Hosts direkt. Dies kann die Netzwerkperformance verbessern, bietet aber keine Isolation.

3. **Overlay-Netzwerk**:
   - Ermöglicht die Kommunikation zwischen Containern auf verschiedenen Hosts in einem Swarm-Cluster.

4. **None-Netzwerk**:
   - Container sind vollständig isoliert und haben keinen Netzwerkzugriff.

## Erstellen eines benutzerdefinierten Netzwerks

Ein benutzerdefiniertes Bridge-Netzwerk bietet Vorteile wie eine automatische DNS-Auflösung der Containernamen.

### Schritte:
1. **Erstellen eines Netzwerks**:
   ```bash
   docker network create my_network
   ```

2. **Container zu einem Netzwerk hinzufügen**:
   Beim Start eines Containers können Sie das Netzwerk angeben:
   ```bash
   docker run -d --name container1 --network my_network alpine sleep 1000
   docker run -d --name container2 --network my_network alpine sleep 1000
   ```

3. **Überprüfen des Netzwerks**:
   Listen Sie die Netzwerke auf:
   ```bash
   docker network ls
   ```

4. **Test der Kommunikation**:
   Verwenden Sie den Containernamen zur Kommunikation:
   ```bash
   docker exec container1 ping container2
   ```

## Beispiel: Kommunikation zwischen einem Webserver und einer Datenbank

### Ziel:
Ein Nginx-Webserver kommuniziert mit einer MySQL-Datenbank innerhalb eines benutzerdefinierten Netzwerks.

### 1. Erstellen des Netzwerks:
```bash
docker network create web_db_network
```

### 2. Starten der MySQL-Datenbank:
```bash
docker run -d   --name mysql   --network web_db_network   -e MYSQL_ROOT_PASSWORD=root   -e MYSQL_DATABASE=testdb   mysql:latest
```

### 3. Starten des Nginx-Webservers:
```bash
docker run -d   --name nginx   --network web_db_network   nginx:latest
```

### 4. Zugriffstests:
- Der Webserver kann die Datenbank über den Hostnamen `mysql` ansprechen.
- Testen Sie die Konnektivität:
  ```bash
  docker exec nginx ping mysql
  ```

## Troubleshooting der Kommunikation

### Häufige Probleme:
1. **Container sind nicht im selben Netzwerk**:
   - Überprüfen Sie die Netzwerkkonfiguration:
     ```bash
     docker network inspect <network_name>
     ```

2. **Firewall blockiert den Traffic**:
   - Stellen Sie sicher, dass keine Firewall-Regeln den Traffic blockieren.

3. **DNS-Auflösung funktioniert nicht**:
   - Verwenden Sie die Container-IP, wenn der Name nicht aufgelöst wird:
     ```bash
     docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name>
     ```
