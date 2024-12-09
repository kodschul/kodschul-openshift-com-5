
# Kubernetes Schulung: Kubernetes Eigenschaften im Überblick

Diese Schulung bietet eine Einführung in die Kernfunktionen und Eigenschaften von Kubernetes sowie seine Rolle als Grundlage für Cloud Native Anwendungen.

## Die Kernfunktionen und Eigenschaften von Kubernetes

Kubernetes (oft als "K8s" abgekürzt) ist eine Open-Source-Plattform zur Automatisierung von Bereitstellung, Skalierung und Betrieb von containerisierten Anwendungen. Die Kernfunktionen machen Kubernetes zu einem der führenden Tools für Container-Orchestrierung.

### 1. **Container-Orchestrierung**
Kubernetes verwaltet mehrere Container-Anwendungen in einem Cluster und stellt sicher, dass sie effizient und zuverlässig laufen. Es bietet:
- **Automatische Platzierung**: Wählt die besten Knoten für die Bereitstellung von Containern.
- **Lastverteilung**: Verteilt die Arbeitslast gleichmäßig über alle Ressourcen.

### 2. **Self-Healing**
Kubernetes überwacht kontinuierlich den Zustand der Anwendungen und stellt sicher, dass sie den gewünschten Zustand erreichen. Dazu gehören:
- **Automatische Neustarts**: Neustart von fehlgeschlagenen Containern.
- **Replikat-Erstellung**: Sicherstellen, dass die gewünschte Anzahl von Pods läuft.
- **Automatisches Entfernen**: Löschen von nicht benötigten Ressourcen.

### 3. **Automatische Skalierung**
Kubernetes kann Anwendungen basierend auf der Ressourcennutzung oder benutzerdefinierten Metriken automatisch skalieren:
- **Horizontal Pod Autoscaling**: Skalierung der Anzahl von Pods.
- **Vertical Pod Autoscaling**: Anpassung der Ressourcen (CPU, RAM) für Pods.
- **Cluster Autoscaling**: Hinzufügen oder Entfernen von Knoten basierend auf dem Bedarf.

### 4. **Ressourcenverwaltung**
Kubernetes ermöglicht eine granulare Kontrolle über die Ressourcennutzung:
- **Ressourcenanforderungen und -limits**: Begrenzen Sie die Ressourcen (z. B. CPU, RAM), die Pods verwenden können.
- **Namespaces**: Trennen Sie Ressourcen für verschiedene Teams oder Projekte.

### 5. **Deklarative Konfiguration**
Kubernetes verwendet YAML- oder JSON-Dateien, um den gewünschten Zustand des Clusters zu deklarieren:
- **Beispiel-Pod-Konfiguration**:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: beispiel-pod
  spec:
    containers:
    - name: nginx
      image: nginx:latest
  ```

### 6. **Netzwerk- und Serviceverwaltung**
- **Services**: Kubernetes stellt stabile Endpunkte für Pods bereit, unabhängig von deren physischer Position.
- **Ingress**: Verwaltung von HTTP- und HTTPS-Zugriffen auf Services.
- **Netzwerk-Richtlinien**: Definieren von Zugriffsregeln zwischen Pods.

### 7. **Speicherverwaltung**
Kubernetes abstrahiert die Speicherung und ermöglicht eine nahtlose Integration mit verschiedenen Speicherlösungen:
- **Persistent Volumes (PV)**: Dauerhafter Speicher, unabhängig von Pods.
- **Dynamic Provisioning**: Automatisches Anlegen von Speicher basierend auf Anforderungen.

## Kubernetes als Grundlage für Cloud Native Anwendungen

Kubernetes spielt eine Schlüsselrolle in der Entwicklung und Bereitstellung von Cloud Native Anwendungen, indem es moderne Praktiken und Technologien unterstützt.

### Was bedeutet "Cloud Native"?
Cloud Native bezeichnet einen Ansatz zur Entwicklung und Betrieb von Anwendungen, die vollständig von der Cloud-Infrastruktur profitieren. Merkmale sind:
- **Containerisierung**: Anwendungen laufen in Containern.
- **Microservices**: Anwendungen sind in kleinere, unabhängige Dienste aufgeteilt.
- **Automatisierung**: Kontinuierliche Integration und Bereitstellung (CI/CD).

### Kubernetes für Cloud Native Anwendungen
1. **Skalierbarkeit und Verfügbarkeit**:
   Kubernetes bietet automatische Skalierung, Failover und Service-Discovery, um Anwendungen zuverlässig und performant zu halten.

2. **CI/CD-Integration**:
   Kubernetes lässt sich leicht in CI/CD-Pipelines integrieren, um automatisierte Deployments und Tests zu ermöglichen:
   - **Beispiel mit GitLab CI**:
     ```yaml
     deploy:
       stage: deploy
       script:
         - kubectl apply -f deployment.yaml
     ```

3. **Flexibilität und Portabilität**:
   Kubernetes abstrahiert die Infrastruktur, sodass Anwendungen auf verschiedenen Plattformen (Cloud, On-Premises) laufen können.

4. **Service Mesh Integration**:
   Kubernetes unterstützt Service Meshes wie Istio und Linkerd, um die Verwaltung von Microservices zu erleichtern:
   - Automatische Verschlüsselung mit mTLS.
   - Traffic-Management und Observability.

### Vorteile von Kubernetes für Cloud Native
- **Schnellere Entwicklung**: Reduzierte Time-to-Market durch Automatisierung.
- **Kostenoptimierung**: Effiziente Ressourcennutzung und Autoscaling.
- **Plattformunabhängigkeit**: Betrieb in verschiedenen Cloud-Umgebungen.

## Fazit

Kubernetes bietet ein umfangreiches Set an Funktionen, das die Verwaltung containerisierter Anwendungen vereinfacht. Es bildet die Grundlage für Cloud Native Architekturen, indem es Skalierbarkeit, Flexibilität und Automatisierung ermöglicht. Mit Kubernetes können Entwickler und Unternehmen moderne Anwendungen erstellen und betreiben, die den Anforderungen der heutigen digitalen Welt gerecht werden.
