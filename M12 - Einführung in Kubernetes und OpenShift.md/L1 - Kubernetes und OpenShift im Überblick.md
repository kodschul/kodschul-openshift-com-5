# Kubernetes und OpenShift im Überblick

## Was ist Kubernetes? Grundlagen des Container-Orchestrators

Kubernetes ist eine Open-Source-Plattform zur Orchestrierung von containerisierten Anwendungen. Es automatisiert die Bereitstellung, Skalierung und Verwaltung von Containern.

### Hauptfunktionen von Kubernetes:
1. **Container-Orchestrierung**:
   - Verwaltung und Koordination von Containern in einem Cluster.

2. **Self-Healing**:
   - Kubernetes startet fehlerhafte Container automatisch neu.
   - Es entfernt Container, die die gewünschten Bedingungen nicht erfüllen.

3. **Automatische Skalierung**:
   - Kubernetes kann basierend auf der Auslastung Pods horizontal skalieren.

4. **Ressourcenverwaltung**:
   - Kubernetes verwaltet die Ressourcenverteilung im Cluster effizient.

### Wichtige Komponenten von Kubernetes:
- **Master-Node**:
  - Enthält die Steuerungsebene, die für die Verwaltung des Clusters zuständig ist (z. B. API-Server, Scheduler, Controller-Manager).
- **Worker-Nodes**:
  - Führen die containerisierten Anwendungen aus.
- **Pods**:
  - Kleinste Einheit, die in Kubernetes bereitgestellt und verwaltet wird.

### Kubernetes Architektur:
```plaintext
+-------------------+            +-------------------+
|     Master Node   |            |   Worker Nodes    |
|-------------------|            |-------------------|
| API Server        |<--->       | Kubelet          |
| Controller Manager|            | Container Runtime|
| Scheduler         |            | Pods             |
| etcd              |            +-------------------+
+-------------------+
```

### Vorteile von Kubernetes:
- Plattformunabhängigkeit.
- Breite Unterstützung durch die Community.
- Nahtlose Integration in CI/CD-Pipelines.

## OpenShift: Aufbauend auf Kubernetes – Mehrwert und Erweiterungen

OpenShift ist eine Kubernetes-basierte Plattform von Red Hat, die zusätzliche Funktionen und Erweiterungen für Entwickler und Administratoren bietet.

### Hauptmerkmale von OpenShift:
1. **Integrierte Entwickler-Tools**:
   - OpenShift enthält eine Benutzeroberfläche und CLI-Tools, die die Bereitstellung und Verwaltung von Anwendungen erleichtern.

2. **Security by Default**:
   - OpenShift verwendet strikte Sicherheitsrichtlinien, wie z. B. die Ausführung von Containern mit reduzierten Berechtigungen.

3. **Operator Framework**:
   - Unterstützt die Automatisierung und Verwaltung komplexer Anwendungen.

4. **Source-to-Image (S2I)**:
   - Automatisiert den Build-Prozess, indem Quellcode direkt in ausführbare Container-Images umgewandelt wird.

5. **Red Hat Ecosystem**:
   - Unterstützung durch kommerzielle Red Hat-Dienste und zertifizierte Container-Images.

### OpenShift Architektur:
Zusätzlich zu den Kubernetes-Komponenten bietet OpenShift:
- **Router**:
  - Unterstützt die automatische Weiterleitung von Anfragen zu den richtigen Pods.
- **Registry**:
  - Interne Container-Registry zur Speicherung und Verwaltung von Images.

## Vergleich: Vanilla Kubernetes vs. OpenShift

| **Aspekt**               | **Vanilla Kubernetes**            | **OpenShift**                    |
|--------------------------|-----------------------------------|-----------------------------------|
| **Sicherheitsrichtlinien** | Erfordert manuelle Konfiguration | Strikte Sicherheitsvorgaben       |
| **Benutzeroberfläche**     | Minimal                         | Umfangreiche Web-Oberfläche       |
| **Support**                | Community-Driven                | Red Hat Support                   |
| **Zertifizierte Images**   | Keine                           | Red Hat zertifizierte Images      |
| **Source-to-Image (S2I)**  | Nicht vorhanden                 | Integriert                        |
| **Updates**                | Manuell                        | Automatische Upgrades             |
