
# OpenShift Projects und Namespaces
## OpenShift Projects und Namespaces

In OpenShift und Kubernetes dienen Namespaces zur Organisation und Trennung von Ressourcen innerhalb eines Clusters. OpenShift erweitert dieses Konzept durch **Projects**, die eine benutzerfreundlichere Art der Verwaltung von Namespaces bieten.

### Was ist ein Namespace?
- Ein Namespace ist ein logischer Container in Kubernetes, der Ressourcen wie Pods, Services und Deployments isoliert.
- Er wird verwendet, um Ressourcen zu gruppieren und Konflikte zwischen Benutzern oder Anwendungen zu vermeiden.

### Was ist ein Project in OpenShift?
- Ein Project ist eine Erweiterung des Namespace-Konzepts in OpenShift.
- Es bietet zusätzliche Funktionen wie:
  - **RBAC (Role-Based Access Control)**: Fein abgestufte Berechtigungen für Benutzer und Gruppen.
  - **Quotas**: Begrenzungen für Ressourcenverbrauch.
  - **Benutzerfreundliche Tools**: OpenShift Projects können einfach über die Webkonsole verwaltet werden.

## Projekte erstellen und verwalten

### Erstellen eines Projekts in OpenShift
Sie können ein Projekt über die OpenShift Webkonsole oder `oc`, das OpenShift Command Line Tool, erstellen.

#### Über die Webkonsole:
1. Melden Sie sich an der OpenShift-Webkonsole an.
2. Klicken Sie auf **Projekte** > **Neues Projekt erstellen**.
3. Füllen Sie die Felder aus:
   - **Name**: Eindeutiger Projektname.
   - **Anzeigename**: Optionaler, benutzerfreundlicher Name.
   - **Beschreibung**: Beschreibung des Projekts.

#### Über die Kommandozeile:
```bash
oc new-project my-project   --display-name="Mein Projekt"   --description="Ein Beispielprojekt in OpenShift"
```

### Anzeigen und Verwalten von Projekten
1. **Liste der Projekte anzeigen**:
   ```bash
   oc get projects
   ```
2. **Zu einem Projekt wechseln**:
   ```bash
   oc project my-project
   ```
3. **Ein Projekt löschen**:
   ```bash
   oc delete project my-project
   ```

### Ressourcen-Quotas und Limits
Projekte in OpenShift können Ressourcenquoten festlegen, um die Nutzung von Speicher, CPU und anderen Ressourcen zu begrenzen.
#### Beispiel für eine Quota-Konfiguration:
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
  namespace: my-project
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "4Gi"
    limits.cpu: "4"
    limits.memory: "8Gi"
```
Anwenden der Quota:
```bash
oc apply -f resource-quota.yaml
```

## Vergleich: OpenShift Projects vs. Kubernetes Namespaces

| Feature                        | OpenShift Projects            | Kubernetes Namespaces         |
|--------------------------------|--------------------------------|--------------------------------|
| **Erstellung**                 | Benutzerfreundlich (Web UI, CLI) | CLI-basiert (`kubectl create namespace`) |
| **Zugriffssteuerung (RBAC)**   | Fein abgestufte Benutzer- und Gruppenrechte | Benutzerdefinierte Rollen erforderlich |
| **Ressourcenmanagement**       | Unterstützt Quotas und Limits | Unterstützt Quotas und Limits |
| **Benutzeroberfläche**         | Verfügbar in der OpenShift-Webkonsole | Keine dedizierte GUI |
| **Integration**                | In OpenShift-Tools integriert | Teil des Kubernetes-Kerns |

### Fazit des Vergleichs
Während Kubernetes Namespaces ein grundlegendes Mittel zur Organisation von Ressourcen bieten, erweitern OpenShift Projects dieses Konzept durch benutzerfreundlichere Verwaltungswerkzeuge und zusätzliche Funktionen.
