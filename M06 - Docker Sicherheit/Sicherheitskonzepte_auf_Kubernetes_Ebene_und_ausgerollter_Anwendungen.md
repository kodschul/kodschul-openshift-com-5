
# Sicherheitskonzepte auf Kubernetes-Ebene und auf der Ebene ausgerollter Anwendungen

## 1. Sicherheitskonzepte auf Kubernetes-Ebene
- **Pod Security Standards (PSS)**: Sicherheit für alle Pods durch restriktive Standards. Nur Pods, die bestimmte Anforderungen erfüllen, können im Cluster ausgeführt werden.
- **Kubernetes Secrets Management**: Sicherer Umgang mit sensiblen Daten (Passwörter, API-Schlüssel) in Kubernetes.

## 2. Sicherheitskonzepte auf der Ebene der Anwendungen
- **Sicheres Container-Image-Management**: Nur geprüfte und vertrauenswürdige Container-Images werden verwendet.
- **Application-Level Security**: TLS/SSL-Kommunikation zwischen Anwendungen, Authentifizierung und Autorisierung auf Anwendungsebene.
- **Zero-Trust-Modelle**: Sicherheitskonzepte, die sicherstellen, dass keine Anwendung automatisch vertraut wird. Jedes Kommunikations- und Datenereignis wird validiert.

## 3. Schutz der Netzwerkschicht
- **Service Meshes**: Einsatz von Service Meshes wie Istio, um Layer-7-Sicherheitsrichtlinien für Anwendungen durchzusetzen.
- **Application Firewalls**: Schutz der Anwendungen durch Web Application Firewalls (WAF) und Netzwerksicherheitsregeln.

