
# Kubernetes Distribution Glossar

## 1. Vanilla Kubernetes

### Befehle:
- **kubectl get pods**: Listet alle Pods im aktuellen Namespace auf.
- **kubectl describe pod <pod-name>**: Zeigt detaillierte Informationen über einen bestimmten Pod.
- **kubectl create deployment <name> --image=<image-name>**: Erstellt ein Deployment mit dem angegebenen Image.
- **kubectl delete pod <pod-name>**: Löscht einen Pod.
- **kubectl apply -f <filename>**: Wendet die Konfiguration in der angegebenen Datei auf das Cluster an.
- **kubectl expose pod <pod-name> --port=<port> --target-port=<target-port>**: Erstellt einen Service, der den Pod nach außen zugänglich macht.
- **kubectl scale deployment <name> --replicas=<number>**: Skaliert ein Deployment auf die angegebene Anzahl von Replikaten.
- **kubectl logs <pod-name>**: Zeigt die Logs eines Pods an.

### Fachbegriffe:
- **Pod**: Die kleinste und einfachste Einheit in Kubernetes, die einen oder mehrere Container umfasst.
- **Namespace**: Eine Möglichkeit, Kubernetes-Ressourcen zu isolieren und in verschiedenen Umgebungen zu organisieren.
- **Service**: Ein logisches Set von Pods, die über eine einzige IP-Adresse und einen DNS-Eintrag zugänglich gemacht werden.
- **ConfigMap**: Eine Methode, um Konfigurationsdaten von außen in ein Container-basiertes System zu laden.
- **Secret**: Eine sicherere Methode zur Speicherung von sensiblen Informationen wie Passwörtern und API-Schlüsseln.

## 2. Kubernetes.io (K8s.io)

### Befehle:
- **kubectl apply -f <filename>**: Führt die Konfiguration aus einer YAML-Datei aus.
- **kubectl get nodes**: Zeigt alle Knoten im Cluster an.
- **kubectl delete node <node-name>**: Entfernt einen Knoten aus dem Cluster.
- **kubectl cordon <node-name>**: Markiert einen Knoten als "nicht planbar", um keine neuen Pods darauf zuzulassen.
- **kubectl drain <node-name>**: Entfernt alle Pods von einem Knoten, um Wartungsarbeiten durchzuführen.

### Fachbegriffe:
- **Cluster**: Eine Gruppe von physischen oder virtuellen Maschinen, die zusammenarbeiten, um Anwendungen auszuführen.
- **Node**: Ein Arbeitsknoten in einem Kubernetes-Cluster, auf dem Pods ausgeführt werden.
- **Kubelet**: Ein Agent, der auf jedem Node im Cluster ausgeführt wird, um sicherzustellen, dass Container gemäß den Konfigurationsspezifikationen laufen.
- **Kubeadm**: Ein Tool zur einfachen Einrichtung eines Kubernetes-Clusters.
- **Helm**: Ein Paketmanager für Kubernetes, der die Installation und Verwaltung von Anwendungen erleichtert.

## 3. OpenShift

### Befehle:
- **oc new-project <project-name>**: Erstellt ein neues Projekt in OpenShift.
- **oc create buildconfig <build-config-name> --image=<image-name>**: Erstellt eine Build-Konfiguration.
- **oc get builds**: Zeigt den Status der Builds im aktuellen Projekt an.
- **oc delete buildconfig <build-config-name>**: Löscht eine Build-Konfiguration.
- **oc logs build/<build-name>**: Zeigt die Logs eines bestimmten Builds an.
- **oc start-build <build-config-name>**: Startet einen neuen Build für eine bestehende Build-Konfiguration.

### Fachbegriffe:
- **Source-to-Image (S2I)**: Eine OpenShift-Technologie, die den Quellcode direkt in ausführbare Container-Images umwandelt.
- **BuildConfig**: Eine Konfiguration in OpenShift, die beschreibt, wie eine Anwendung erstellt und in einem Container gepackt wird.
- **Route**: Ein OpenShift-Objekt, das externe HTTP-Anfragen an einen Service im Cluster weiterleitet.
- **Project**: Eine logische Gruppierung von OpenShift-Ressourcen, ähnlich wie ein Namespace in Kubernetes.

## 4. Rancher

### Befehle:
- **rancher login <URL> --token <token>**: Meldet sich in der Rancher-CLI an.
- **rancher kubectl get clusters**: Zeigt alle Cluster, die von Rancher verwaltet werden.
- **rancher kubectl describe cluster <cluster-id>**: Zeigt detaillierte Informationen zu einem Cluster an.
- **rancher kubectl scale deployment <deployment-name> --replicas=<number>**: Skaliert ein Deployment in einem Rancher-verwalten Cluster.
- **rancher kubectl delete cluster <cluster-id>**: Löscht einen Cluster über Rancher.

### Fachbegriffe:
- **Multi-Cluster-Management**: Rancher ermöglicht die Verwaltung mehrerer Kubernetes-Cluster über eine einzige Schnittstelle.
- **Rancher Apps**: Eine Sammlung von vorgefertigten Kubernetes-Anwendungen, die direkt aus dem Rancher-Katalog installiert werden können.
- **RKE (Rancher Kubernetes Engine)**: Eine leichte Kubernetes-Distribution, die von Rancher bereitgestellt wird und speziell für einfache Verwaltung und Bereitstellung entwickelt wurde.
- **Kontrollplane**: Rancher bietet eine zentrale Steuerungsebene für die Verwaltung von Kubernetes-Clustern.
- **Cluster Templates**: Vordefinierte Konfigurationen für die Erstellung von Kubernetes-Clustern mit Rancher.
