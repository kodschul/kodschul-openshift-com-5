
# Architektur und Hauptkomponenten von Kubernetes

## Architektur und Hauptkomponenten von Kubernetes

Kubernetes ist ein Open-Source-System zur Automatisierung der Bereitstellung, Skalierung und Verwaltung containerisierter Anwendungen. Seine Architektur basiert auf einem Master-Worker-Modell.

### System-Übersicht der Architektur

#### Master-Komponenten
Die Master-Komponenten verwalten den Kubernetes-Cluster und sorgen für die gewünschte Zustandsverwaltung der Anwendungen.

1. **API Server**:
   - Zentraler Einstiegspunkt für alle externen und internen Anfragen.
   - Verantwortlich für die Authentifizierung, Validierung und Weiterleitung von Anfragen.
   - Kommuniziert mit dem etcd-Cluster, um die Cluster-Daten zu speichern.

2. **Controller Manager**:
   - Läuft als Daemon und sorgt für die Überwachung und Regulierung des Systemstatus.
   - Beispiele für Controller:
     - **Node Controller**: Überwacht den Zustand der Worker-Nodes.
     - **Replication Controller**: Stellt sicher, dass die gewünschte Anzahl an Pod-Instanzen läuft.

3. **Scheduler**:
   - Verteilt Pods auf Nodes basierend auf Ressourcenanforderungen und -verfügbarkeit.
   - Entscheidet über die beste Platzierung eines Pods unter Berücksichtigung von:
     - Ressourcenlimits (CPU, RAM)
     - Affinität/Anti-Affinität
     - Nutzerdefinierten Regeln

4. **etcd** (Speichersystem):
   - Schlüssel-Wert-Datenbank für alle Cluster-Daten.
   - Speichert Konfigurationsdaten, Statusinformationen und Metadaten.

#### Worker-Komponenten
Die Worker-Nodes führen die containerisierten Anwendungen aus und kommunizieren mit den Master-Komponenten.

1. **Kubelet**:
   - Agent, der auf jedem Worker-Node läuft.
   - Überwacht Pods und Container.
   - Führt Anweisungen des API Servers aus.

2. **Kube-Proxy**:
   - Netzwerkkomponente, die für das Routing des Datenverkehrs zwischen Pods und externen Anfragen zuständig ist.
   - Unterstützt Load-Balancing und DNS-Integration.

3. **Container Runtime**:
   - Software zur Ausführung von Containern (z. B. Docker, containerd, CRI-O).

### Kubernetes Architekturdiagramm

Ein typisches Kubernetes-Setup umfasst mehrere Master- und Worker-Nodes, die miteinander kommunizieren, um Anwendungen bereitzustellen und zu skalieren.

```plaintext
                 Master Node
------------------------------------------------
| API Server  | Controller Manager | Scheduler |
------------------------------------------------
        |
        |
     etcd Cluster
        |
        |
-----------------------------------------
|          Worker Nodes                 |
-----------------------------------------
|  Kubelet | Kube-Proxy | Container Runtime |
-----------------------------------------
|                  Pods                   |
-----------------------------------------
```

## Einführung in die verschiedenen Installationsoptionen

Kubernetes kann auf verschiedene Arten installiert werden, abhängig von der gewünschten Umgebung und dem Anwendungsfall.

### 1. Cloud-Installationen
Cloud-Anbieter bieten Managed Kubernetes-Dienste, die die Verwaltung der Infrastruktur vereinfachen:
- **Google Kubernetes Engine (GKE)**:
  - Vollständig verwalteter Kubernetes-Dienst von Google.
  - Automatische Upgrades, Skalierung und Überwachung.
- **Amazon Elastic Kubernetes Service (EKS)**:
  - Verwalteter Kubernetes-Dienst von AWS.
  - Integration mit anderen AWS-Diensten wie IAM und CloudWatch.
- **Azure Kubernetes Service (AKS)**:
  - Verwalteter Dienst von Microsoft Azure.
  - Unterstützung für Hybrid-Workloads.

#### Vorteile:
- Keine Wartung der Steuerungsebene (Master-Komponenten).
- Einfache Skalierbarkeit.
- Integrierte Sicherheits- und Überwachungsfunktionen.

### 2. Lokale Installationen mit MiniKube
**MiniKube** ist ein leichtgewichtiges Tool, um Kubernetes auf einer einzelnen Maschine lokal auszuführen. Es eignet sich hervorragend für Lernzwecke und Entwicklung.

#### Installation:
1. **Installation von MiniKube**:
   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   ```

2. **Starten eines Kubernetes-Clusters**:
   ```bash
   minikube start
   ```

3. **Überprüfung des Clusters**:
   ```bash
   kubectl get nodes
   ```

#### Vorteile:
- Einfach einzurichten und zu nutzen.
- Keine Abhängigkeit von Cloud-Diensten.
- Unterstützt verschiedene Container-Runtimes.

### 3. Bare-Metal-Installationen
Für spezifische Anforderungen oder große Self-Hosted-Setups kann Kubernetes auf Bare-Metal-Servern installiert werden. Tools wie **kubeadm** erleichtern diesen Prozess.

#### Vorteile:
- Volle Kontrolle über die Infrastruktur.
- Anpassbar an spezielle Hardware- und Netzwerkanforderungen.
