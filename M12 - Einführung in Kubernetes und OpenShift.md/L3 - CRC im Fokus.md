
# CRC im Fokus
## Vorteile von CRC für die lokale Entwicklung

CRC ist eine leichtgewichtige OpenShift-Implementierung, die für lokale Entwicklungs- und Testzwecke konzipiert ist. Es bietet Entwicklern eine schnelle Möglichkeit, eine vollständige OpenShift-Umgebung auf ihrem lokalen Rechner zu nutzen.

### Vorteile:
1. **Einfache Einrichtung**:
   - CRC erfordert nur minimale Konfiguration und ist in wenigen Minuten einsatzbereit.
   - Ideal für Entwickler, die keine umfangreiche Infrastruktur benötigen.

2. **Kosteneffizienz**:
   - Keine zusätzlichen Cloud-Kosten, da CRC lokal ausgeführt wird.
   - Ressourcenintensität ist auf das Nötigste beschränkt.

3. **Isolierte Umgebung**:
   - Bietet eine sichere Sandbox für Experimente, ohne andere Umgebungen zu beeinflussen.

4. **Komplette OpenShift-Funktionalität**:
   - Alle Kernfunktionen von OpenShift, wie OperatorHub, Pods, Deployments und Services, sind verfügbar.

5. **Schnelles Feedback**:
   - Änderungen können lokal entwickelt und getestet werden, bevor sie in ein produktionsähnliches Cluster migriert werden.

### Installation von CRC:
1. **Download CRC**:
   Besuchen Sie [die offizielle CRC-Seite](https://developers.redhat.com/products/openshift-local/overview) und laden Sie die neueste Version herunter.

2. **Installation**:
   ```bash
   crc setup
   crc start
   ```
3. **Anmeldung**:
   Erhalten Sie die Anmeldeinformationen:
   ```bash
   crc console --credentials
   ```

## CRC vs. Cluster-Deployments in der Cloud

Obwohl CRC für lokale Entwicklung ideal ist, unterscheidet es sich von OpenShift-Cluster-Deployments in der Cloud, wie OKE (Oracle Kubernetes Engine), ARO (Azure Red Hat OpenShift) oder ROSA (Red Hat OpenShift on AWS).

### Vergleich:
| Feature                     | CRC                                 | Cloud-Deployments (z. B. OKE, ARO, ROSA) |
|-----------------------------|-------------------------------------|------------------------------------------|
| **Zweck**                   | Lokale Entwicklung und Tests       | Skalierbare, produktionsreife Umgebungen |
| **Kosten**                  | Keine zusätzlichen Kosten          | Cloud-Nutzungsgebühren                   |
| **Leistung**                | Beschränkt auf lokale Ressourcen   | Hohe Leistung durch skalierbare Ressourcen |
| **Skalierbarkeit**          | Nicht skalierbar                   | Horizontale und vertikale Skalierung     |
| **Verfügbarkeit**           | Lokal verfügbar                    | Global verfügbar, Hochverfügbarkeit      |
| **Netzwerkzugriff**         | Lokal isoliert                     | Vollständig vernetzt und integriert      |

### Wann CRC verwenden?
- Lokale Entwicklung und Testing.
- Evaluierung von OpenShift-Funktionen.
- Schulungen und Demonstrationen.

### Wann Cloud-Deployments verwenden?
- Produktive Workloads.
- Anwendungen mit hohen Verfügbarkeitsanforderungen.
- Skalierbare und global verteilte Dienste.

## Einsatzmöglichkeiten und Einschränkungen von CRC

### Einsatzmöglichkeiten:
1. **Lokale Entwicklung**:
   - Ideal für das Erstellen und Testen von Anwendungen in einer realistischen OpenShift-Umgebung.

2. **Schulungen und Demonstrationen**:
   - CRC bietet eine schnelle Möglichkeit, OpenShift für Trainings und Präsentationen bereitzustellen.

3. **Experimentelle Features**:
   - Entwickler können neue OpenShift-Funktionen oder -Operatoren sicher testen.

4. **CI/CD-Pipeline-Integration**:
   - CRC kann in lokale CI/CD-Workflows integriert werden.

### Einschränkungen:
1. **Ressourcenbeschränkungen**:
   - CRC ist auf die Ressourcen des lokalen Rechners begrenzt (RAM, CPU, Speicherplatz).

2. **Nicht für die Produktion**:
   - CRC ist nicht für den Betrieb produktiver Workloads geeignet.

3. **Keine Hochverfügbarkeit**:
   - CRC stellt keinen redundanten Cluster bereit.

4. **Eingeschränkte Skalierbarkeit**:
   - Pods und Anwendungen sind auf die Kapazitäten der lokalen Maschine beschränkt.

5. **Komplexe Netzwerkkonfigurationen**:
   - CRC ist auf lokale Netzwerke beschränkt und eignet sich nicht für komplexe Netzwerkszenarien.
