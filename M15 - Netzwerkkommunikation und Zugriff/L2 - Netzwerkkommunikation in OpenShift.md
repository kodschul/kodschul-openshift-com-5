
# OpenShift Schulung: Grundlagen der Netzwerkkommunikation
## Einführung in Services und Ingress

### Services

Ein **Service** ist eine Abstraktion in Kubernetes und OpenShift, die eine Gruppe von Pods als einheitlichen Endpunkt zusammenfasst. Services ermöglichen die Kommunikation zwischen Pods und stellen sicher, dass der Datenverkehr zu den richtigen Endpunkten weitergeleitet wird, auch wenn sich die Pods dynamisch ändern.

#### Typen von Services:
1. **ClusterIP** (Standard):
   - Interner Zugriff innerhalb des Clusters.
   - Beispiel:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       selector:
         app: my-app
       ports:
       - protocol: TCP
         port: 80
         targetPort: 8080
     ```

2. **NodePort**:
   - Macht den Service auf einem bestimmten Port des Nodes verfügbar.
   - Beispiel:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       type: NodePort
       selector:
         app: my-app
       ports:
       - port: 80
         targetPort: 8080
         nodePort: 30007
     ```

3. **LoadBalancer**:
   - Nutzt einen Cloud Load Balancer für externen Zugriff (abhängig von der Infrastruktur).

### Ingress

**Ingress** ist eine API-Ressource, die HTTP- und HTTPS-Zugriff auf Services ermöglicht. Es bietet Funktionen wie Load-Balancing, SSL-Terminierung und virtuellen Host-Routing.

#### Beispiel eines Ingress:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: my-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

#### Vorteile von Ingress:
- Effiziente Handhabung von HTTP(S)-Datenverkehr.
- Möglichkeit zur Konfiguration von SSL-Zertifikaten.

## Routes: Zugriff auf Anwendungen in CRC ermöglichen

**Routes** sind eine OpenShift-spezifische Funktion, die den externen Zugriff auf Services ermöglicht. Sie erweitern die Funktionalität von Ingress und bieten eine einfache Möglichkeit, Anwendungen im Cluster zugänglich zu machen.

### Aufbau einer Route:
Eine Route verbindet einen OpenShift-Service mit einem öffentlichen Hostnamen und stellt so den Zugriff auf die Anwendung sicher.

#### Beispiel einer Route:
```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: my-route
spec:
  host: my-app.apps-crc.testing
  to:
    kind: Service
    name: my-service
  port:
    targetPort: 8080
  tls:
    termination: edge
```

#### Erklärung:
- **`host`**: Der DNS-Name, unter dem die Anwendung erreichbar ist.
- **`to`**: Der Service, an den der Traffic weitergeleitet wird.
- **`tls`**: Optionale TLS-Konfiguration für HTTPS-Zugriff.

### Verwendung von Routes in CRC:
1. **Zugriff auf die Standard-Domain**:
   CRC stellt automatisch eine Entwicklungsumgebung mit einer Standard-Domain bereit (z. B. `*.apps-crc.testing`).
2. **Route erstellen**:
   - Mit `oc expose` können Sie schnell eine Route erstellen:
     ```bash
     oc expose service my-service
     ```
   - Überprüfen Sie die Route:
     ```bash
     oc get routes
     ```
   - Die Anwendung ist unter dem angegebenen Hostnamen erreichbar.

### SSL/TLS in Routes:
- OpenShift unterstützt verschiedene Arten der TLS-Terminierung:
  - **Edge**: TLS wird an der Route terminiert.
  - **Passthrough**: TLS wird direkt an den Pod weitergeleitet.
  - **Reencrypt**: TLS wird an der Route und erneut an den Pod terminiert.

#### Beispiel:
```yaml
tls:
  termination: edge
  insecureEdgeTerminationPolicy: Redirect
```

## Best Practices für Netzwerkkommunikation in OpenShift

1. **Service-Selector richtig konfigurieren**:
   - Stellen Sie sicher, dass Ihre Services die Pods anhand korrekter Labels auswählen.

2. **Richtige Route-Terminierung wählen**:
   - Wählen Sie je nach Anwendungsanforderungen zwischen `edge`, `passthrough` und `reencrypt`.

3. **DNS-Einträge prüfen**:
   - Überprüfen Sie, ob der Hostname der Route korrekt auf die OpenShift-Umgebung verweist.

4. **SSL-Zertifikate verwalten**:
   - Nutzen Sie automatisierte Tools wie Let's Encrypt für Zertifikate oder importieren Sie manuell eigene Zertifikate.

5. **Netzwerksicherheitsrichtlinien anwenden**:
   - Konfigurieren Sie NetworkPolicies, um den Datenverkehr zu kontrollieren und ungewollte Zugriffe zu vermeiden.
