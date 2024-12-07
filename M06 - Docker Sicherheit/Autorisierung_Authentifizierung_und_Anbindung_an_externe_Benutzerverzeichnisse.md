
# Autorisierung, Authentifizierung und Anbindung an externe Benutzerverzeichnisse

## 1. Authentifizierung (AuthN) in Kubernetes
- **Externe Authentifizierungsdienste**: Kubernetes kann in bestehende Authentifizierungsmechanismen wie OpenID, OAuth2, LDAP und Active Directory integriert werden.
- **Konfiguration**: Die Authentifizierung wird auf API-Server-Ebene implementiert und kann durch Token, Zertifikate oder Plugins realisiert werden.

## 2. Autorisierung (AuthZ) in Kubernetes
- **Role-Based Access Control (RBAC)**: Feingranulare Steuerung von Zugriffsrechten über RBAC. Zugriff kann auf Benutzer oder Gruppen beschränkt werden.
- **Pod Security Policies (PSP)**: Einschränkungen für die Sicherheitskonfiguration von Pods können durch PSP festgelegt werden.
- **Admission Controllers**: Diese Kubernetes-Komponenten entscheiden, ob Anfragen an den API-Server akzeptiert oder abgelehnt werden.

## 3. Anbindung an externe Benutzerverzeichnisse
- **LDAP und Active Directory**: Integration von Kubernetes mit LDAP und AD für die Benutzerverwaltung und Authentifizierung.
- **OAuth und OpenID Connect**: Verwendung von OAuth2 oder OIDC für die Authentifizierung von Benutzern über externe Identity Provider (IDPs).

## 4. Audit Logs und Monitoring
- **Überwachung von Authentifizierungs- und Autorisierungsereignissen**: Sicherstellen, dass alle Zugriffe überwacht und protokolliert werden.
- **Audit-Logs zur Einhaltung von Compliance**: Logs ermöglichen eine transparente Nachverfolgung von Zugriffen und Änderungen.
