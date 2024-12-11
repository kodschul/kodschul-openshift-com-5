
# Authentifizierung und Autorisierung in CRC
## Benutzer- und Rechteverwaltung in der lokalen Umgebung

### Was ist CRC?
CodeReady Containers (CRC) bietet eine lokale OpenShift-Umgebung für Entwickler. In einer CRC-Umgebung können Benutzer- und Rechteverwaltungsfunktionen wie in einer vollständigen OpenShift-Installation verwendet werden, allerdings mit einigen Einschränkungen für lokale Setups.

### Benutzerverwaltung in OpenShift
In OpenShift gibt es zwei Haupttypen von Benutzern:
1. **Cluster-Admins**: Haben Zugriff auf alle Ressourcen und Berechtigungen.
2. **Normale Benutzer**: Können nur auf Ressourcen zugreifen, für die sie Berechtigungen erhalten haben.

#### Benutzer erstellen:
In CRC können Benutzer mit dem Befehl `htpasswd` hinzugefügt werden:
```bash
htpasswd -b /path/to/htpasswd username password
```

#### Benutzer in OpenShift registrieren:
1. Bearbeiten Sie den OAuth-Operator:
   ```bash
   oc edit oauth cluster
   ```
2. Fügen Sie den neuen Benutzer der `htpasswd`-Konfiguration hinzu.

### Rollen und Rechteverwaltung
OpenShift verwendet Role-Based Access Control (RBAC), um Zugriffsrechte zu definieren.

#### Rollen erstellen:
Eine benutzerdefinierte Rolle kann wie folgt erstellt werden:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: my-namespace
  name: my-role
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

#### Rollen zuweisen:
Rollen werden Benutzern mit `RoleBinding` zugewiesen:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: my-rolebinding
  namespace: my-namespace
subjects:
  - kind: User
    name: username
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: my-role
  apiGroup: rbac.authorization.k8s.io
```

### Berechtigungen überprüfen:
Mit dem folgenden Befehl können Sie die Berechtigungen eines Benutzers überprüfen:
```bash
oc auth can-i <verb> <resource> --as=username
```

## OpenShift OAuth: Grundlagen der Authentifizierung

OpenShift verwendet OAuth 2.0 als Authentifizierungsprotokoll. OAuth stellt sicher, dass Benutzer Zugriff auf Ressourcen erhalten, ohne ihre Anmeldedaten direkt weiterzugeben.

### Standard-Authentifizierungsanbieter in OpenShift:
- **HTPasswd**: Für einfache lokale Benutzerverwaltung.
- **LDAP**: Für die Integration mit Unternehmensverzeichnisdiensten.
- **GitHub/Google**: Für OAuth-basierte Authentifizierung.
- **Keystone**: Für OpenStack-Integrationen.

### Konfiguration des OAuth-Servers:
Die Authentifizierungsstrategie wird über den `OAuth`-Operator konfiguriert. Ein Beispiel für die HTPasswd-Authentifizierung:
```yaml
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: my-htpasswd
    mappingMethod: claim
    type: HTPasswd
    htpasswd:
      fileData:
        name: htpasswd-secret
```

#### Erstellen des Secrets für HTPasswd:
```bash
oc create secret generic htpasswd-secret --from-file=htpasswd=/path/to/htpasswd -n openshift-config
```

#### Benutzer mit OAuth hinzufügen:
1. Bearbeiten Sie die OAuth-Konfiguration:
   ```bash
   oc edit oauth cluster
   ```
2. Fügen Sie die Konfigurationsdetails für den Authentifizierungsanbieter hinzu.

### OAuth-Token:
OAuth-Token werden verwendet, um die Identität eines Benutzers zu bestätigen. Sie können Token mit folgendem Befehl generieren:
```bash
oc whoami -t
```

### Ablauf eines OAuth-Prozesses:
1. Der Benutzer wird zur Authentifizierungsseite umgeleitet.
2. Nach erfolgreicher Anmeldung wird ein Token generiert.
3. Der Token wird zur Autorisierung von Anfragen verwendet.

## Best Practices für Authentifizierung und Autorisierung in CRC

1. **Einschränkung der Admin-Berechtigungen**:
   - Geben Sie Cluster-Admin-Rechte nur an vertrauenswürdige Benutzer weiter.
2. **Verwendung von RBAC**:
   - Verwenden Sie Rollen und Rollenbindungen, um den Zugriff granular zu steuern.
3. **Regelmäßiges Überprüfen von Berechtigungen**:
   - Überprüfen Sie regelmäßig, welche Benutzer auf welche Ressourcen zugreifen können.
4. **OAuth sicher konfigurieren**:
   - Verwenden Sie sichere Passwörter und verschlüsselte Verbindungen für HTPasswd.
5. **Token-Management**:
   - Stellen Sie sicher, dass abgelaufene Token regelmäßig entfernt werden.
