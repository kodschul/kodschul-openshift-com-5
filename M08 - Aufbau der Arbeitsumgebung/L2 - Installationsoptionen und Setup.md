
# Installationsoptionen und Setup

## Installationsoptionen für Kubernetes

Kubernetes kann auf verschiedene Arten installiert werden, abhängig von den Anforderungen, der Umgebung und dem Anwendungsfall. Die gängigsten Optionen sind:

### 1. Lokale Installation
Für Entwicklung und Tests ist eine lokale Installation ideal. Sie ermöglicht es, Kubernetes auf einem lokalen Rechner oder einer VM auszuführen.

- **Minikube**:
  - Ein beliebtes Werkzeug zur Installation von Kubernetes auf einem lokalen Rechner.
  - Unterstützt mehrere Virtualisierungstools wie VirtualBox und Hyper-V.
  - Einfach zu bedienen und ideal für erste Schritte.
  - Installation:
    ```bash
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    minikube start
    ```

- **Kind (Kubernetes in Docker)**:
  - Nutzt Docker-Container, um eine lokale Kubernetes-Umgebung bereitzustellen.
  - Besonders leichtgewichtig und ideal für schnelle Tests.
  - Installation:
    ```bash
    GO111MODULE="on" go install sigs.k8s.io/kind@latest
    kind create cluster
    ```

### 2. Managed Kubernetes
Managed Kubernetes wird von Cloud-Anbietern bereitgestellt, wodurch der Aufwand für das Setup und die Verwaltung reduziert wird.

- **Google Kubernetes Engine (GKE)**:
  - Vollständig verwaltete Kubernetes-Cluster auf Google Cloud.
  - Unterstützt automatische Updates und Skalierung.

- **Amazon Elastic Kubernetes Service (EKS)**:
  - Ein Kubernetes-Service von AWS mit hoher Integration in andere AWS-Dienste.

- **Azure Kubernetes Service (AKS)**:
  - Kubernetes-Cluster in der Microsoft Azure Cloud mit einfacher Einrichtung und Integration.

### 3. Bare-Metal-Installation
Für Unternehmen, die volle Kontrolle über ihre Kubernetes-Umgebung benötigen, ist eine Bare-Metal-Installation geeignet.

- **kubeadm**:
  - Ein Werkzeug, um Kubernetes-Cluster auf Bare-Metal-Servern oder VMs zu installieren.
  - Installation:
    ```bash
    sudo apt-get update
    sudo apt-get install -y apt-transport-https ca-certificates curl
    sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
    echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    kubeadm init
    ```

## Kubernetes Setup

### Voraussetzungen
1. **Hardware-Anforderungen**:
   - Mindestens 2 CPUs.
   - 2 GB RAM.
   - 20 GB freier Festplattenspeicher.
2. **Netzwerkzugriff**:
   - Stellen Sie sicher, dass Ihr System Zugriff auf das Internet hat, um Docker-Images und Kubernetes-Pakete herunterzuladen.

### Kubernetes-Setup mit Minikube
1. **Minikube starten**:
   ```bash
   minikube start
   ```
2. **Cluster-Status prüfen**:
   ```bash
   kubectl cluster-info
   ```
3. **Testen des Clusters**:
   Erstellen und starten Sie eine Beispiel-Anwendung:
   ```bash
   kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
   kubectl expose deployment hello-minikube --type=NodePort --port=8080
   minikube service hello-minikube
   ```

### Kubernetes-Setup mit kubeadm
1. **Cluster initialisieren**:
   ```bash
   sudo kubeadm init
   ```
2. **Kubeconfig einrichten**:
   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```
3. **Netzwerk-Plugin installieren**:
   Wählen Sie ein Netzwerk-Plugin wie Calico oder Flannel:
   ```bash
   kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
   ```