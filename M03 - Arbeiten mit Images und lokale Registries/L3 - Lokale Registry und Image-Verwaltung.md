
# Lokale Registry und Image-Verwaltung
## Einrichten und Verwenden einer lokalen Registry

Eine lokale Docker-Registry ermöglicht das Speichern und Abrufen von Docker-Images innerhalb eines privaten Netzwerks, ohne dass eine externe Registry wie Docker Hub verwendet werden muss.

### Schritte zur Einrichtung einer lokalen Registry:

1. **Docker Registry Container starten**:
   Docker stellt ein offizielles Image für die Registry bereit.
   ```bash
   docker run -d -p 5000:5000 --name registry registry:2
   ```
   - `-d`: Startet den Container im Hintergrund.
   - `-p 5000:5000`: Bindet die Registry an den lokalen Port 5000.

2. **Prüfen, ob die Registry läuft**:
   Überprüfen Sie, ob die Registry erfolgreich gestartet wurde:
   ```bash
   curl http://localhost:5000/v2/
   ```
   Die Antwort `{}` zeigt an, dass die Registry läuft.

3. **Konfiguration der Sicherheit** (optional):
   - Sie können die Registry mit einem SSL-Zertifikat sichern, um HTTPS-Verbindungen zu ermöglichen.
   - Für einfache lokale Tests können Sie den Zugriff unverschlüsselt lassen.

### Vorteile einer lokalen Registry:
- Kontrolle über gespeicherte Images.
- Schnellere Bereitstellung, da Images nicht aus dem Internet heruntergeladen werden müssen.
- Vertraulichkeit durch Vermeidung öffentlicher Registries.

## Images in einer Registry speichern und wiederverwenden

### Schritte zum Speichern von Images in einer lokalen Registry:

1. **Image taggen**:
   Taggen Sie das Image, um es mit der lokalen Registry zu verknüpfen:
   ```bash
   docker tag <image-name> localhost:5000/<image-name>
   ```
   Beispiel:
   ```bash
   docker tag my-app localhost:5000/my-app
   ```

2. **Image in die Registry pushen**:
   Verwenden Sie den `docker push`-Befehl, um das Image in die Registry hochzuladen:
   ```bash
   docker push localhost:5000/my-app
   ```

3. **Überprüfen der gespeicherten Images**:
   Sie können die gespeicherten Images in der Registry auflisten:
   ```bash
   curl http://localhost:5000/v2/_catalog
   ```

### Wiederverwenden von Images:

1. **Image aus der Registry pullen**:
   Ziehen Sie ein gespeichertes Image aus der Registry:
   ```bash
   docker pull localhost:5000/my-app
   ```

2. **Image lokal ausführen**:
   Starten Sie einen Container mit dem gepullten Image:
   ```bash
   docker run -d localhost:5000/my-app
   ```

### Löschen von Images aus der Registry:
Das Docker-Registry-Image unterstützt standardmäßig kein direktes Löschen von Images. Sie können jedoch das `registry:2` Image mit einem Garbage-Collector-Tool erweitern.

## Best Practices für die Image-Verwaltung

- **Image-Versionierung**:
  Taggen Sie Images mit eindeutigen Versionen (z. B. `localhost:5000/my-app:1.0`), um Änderungen nachzuvollziehen.

- **Optimierung von Images**:
  Verwenden Sie schlanke Basis-Images und entfernen Sie ungenutzte Dateien, um die Image-Größe zu reduzieren.

- **Regelmäßige Bereinigung**:
  Entfernen Sie veraltete Images aus der Registry, um Speicherplatz zu sparen.
