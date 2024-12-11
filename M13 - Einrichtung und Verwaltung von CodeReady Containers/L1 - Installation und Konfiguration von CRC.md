
# Installation und Konfiguration von CRC
## Voraussetzungen und unterstützte Plattformen

Bevor Sie mit der Installation von CRC beginnen, stellen Sie sicher, dass Ihr System die folgenden Anforderungen erfüllt.

### Unterstützte Plattformen:
- **Linux**: Aktuelle Versionen von CentOS, RHEL, Fedora und Ubuntu werden unterstützt.
- **macOS**: Version 10.13 (High Sierra) oder höher.
- **Windows**: Windows 10 Pro, Enterprise oder Education (64-Bit).

### Systemvoraussetzungen:
- **Prozessor**: 4 CPU-Kerne
- **Arbeitsspeicher**: Mindestens 8 GB RAM (empfohlen: 16 GB)
- **Festplattenspeicher**: Mindestens 35 GB freier Speicherplatz
- **Virtualisierung**:
  - VirtualBox, Hyper-V, KVM oder VMware müssen installiert und aktiviert sein.
  - Auf macOS wird HyperKit empfohlen.

### Abhängigkeiten:
- **crc CLI**: Das Tool muss heruntergeladen und installiert werden.
- **Pull Secret**: Ein gültiges Pull Secret von Red Hat ist erforderlich, um OpenShift-Container-Images herunterzuladen.

## Schritt-für-Schritt-Installation von CRC

### 1. Laden Sie CRC herunter
Besuchen Sie die [offizielle CRC-Website](https://developers.redhat.com/products/codeready-containers) und laden Sie die passende Version für Ihr Betriebssystem herunter.

### 2. Installieren Sie CRC
#### Linux:
1. Extrahieren Sie das Archiv:
   ```bash
   tar -xvf crc-linux-amd64.tar.xz
   mv crc-linux-*-amd64/crc /usr/local/bin/
   ```
2. Überprüfen Sie die Installation:
   ```bash
   crc version
   ```

#### macOS:
1. Extrahieren Sie das Archiv:
   ```bash
   tar -xvf crc-macos-amd64.tar.xz
   mv crc /usr/local/bin/
   ```
2. Überprüfen Sie die Installation:
   ```bash
   crc version
   ```

#### Windows:
1. Entpacken Sie das Archiv.
2. Fügen Sie den entpackten Ordner zur PATH-Umgebungsvariable hinzu.
3. Überprüfen Sie die Installation:
   ```powershell
   crc version
   ```

### 3. Pull Secret hinzufügen
1. Laden Sie das Pull Secret von der Red Hat Developer-Website herunter.
2. Fügen Sie das Pull Secret hinzu:
   ```bash
   crc setup
   ```

### 4. Starten Sie CRC
1. Starten Sie CRC mit dem folgenden Befehl:
   ```bash
   crc start
   ```
2. Folgen Sie den Anweisungen, um die OpenShift-Konsole zu starten.

### 5. Zugriff auf die OpenShift-Webkonsole
- Webkonsole: [https://console-openshift-console.apps-crc.testing](https://console-openshift-console.apps-crc.testing)
- Anmeldedaten:
  - Benutzername: `developer`
  - Passwort: `developer`

### 6. Zugriff über die OpenShift CLI
Verwenden Sie die `oc` CLI, um mit OpenShift zu interagieren:
1. Laden Sie die OpenShift-Konfiguration:
   ```bash
   eval $(crc oc-env)
   ```
2. Testen Sie die Verbindung:
   ```bash
   oc login -u developer -p developer https://api.crc.testing:6443
   ```

## Optimierung der Ressourcennutzung für lokale Umgebungen

Da CRC eine ressourcenintensive Anwendung ist, sollten Sie die Ressourcennutzung optimieren, um die Leistung auf Ihrem lokalen Rechner zu verbessern.

### 1. Reduzieren der zugewiesenen Ressourcen
Passen Sie die zugewiesenen Ressourcen in der CRC-Konfiguration an:
```bash
crc config set memory 8192
crc config set cpus 4
```

### 2. Deaktivieren nicht benötigter Dienste
Schalten Sie nicht benötigte Dienste aus, um Ressourcen zu sparen. Beispielsweise können Monitoring-Operatoren deaktiviert werden:
```bash
oc scale --replicas=0 deployment.apps/monitoring-operator -n openshift-monitoring
```

### 3. Persistent Volume Storage minimieren
Reduzieren Sie die Anzahl der automatisch erstellten Persistent Volumes:
```bash
oc patch storageclass standard -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```

### 4. Verwendung im Headless-Modus
Falls die Weboberfläche nicht benötigt wird, starten Sie CRC im Headless-Modus:
```bash
crc start --headless
```