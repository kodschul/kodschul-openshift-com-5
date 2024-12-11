
# OpenShift-Varianten und Einsatzgebiete
## OpenShift Container Platform (OCP), OKD und CodeReady Containers (CRC)

### OpenShift Container Platform (OCP)
Die OpenShift Container Platform ist die kommerzielle Variante von OpenShift, die von Red Hat unterstützt wird. Sie basiert auf Kubernetes und bietet erweiterte Funktionen wie:
- **Unternehmenssupport**: Enthält SLAs und professionelle Unterstützung.
- **Erweiterte Sicherheitsfeatures**: Integriert Security Context Constraints (SCC) und rollenbasierte Zugriffskontrolle (RBAC).
- **Zertifizierte Operatoren**: Zugang zu Red Hat-zertifizierten Operatoren über den OperatorHub.
- **Multi-Cluster-Verwaltung**: Verwaltung von mehreren Kubernetes-Clustern über Red Hat Advanced Cluster Management (ACM).

### OKD (OpenShift Origin)
OKD ist die Community-Edition von OpenShift. Es bietet viele der gleichen Funktionen wie OCP, jedoch ohne kommerziellen Support.
- **Kostenfrei**: Ideal für nicht-produktive Umgebungen und Lernzwecke.
- **Flexibilität**: Ermöglicht die Verwendung von nicht zertifizierten, community-gepflegten Add-ons und Operatoren.

#### Unterschiede zwischen OCP und OKD:
- OCP: Enthält Red Hat Enterprise Linux CoreOS (RHCOS).
- OKD: Nutzt Fedora CoreOS (FCOS).

### CodeReady Containers (CRC)
CodeReady Containers ist eine leichtgewichtige Variante von OpenShift für Entwickler. Es ermöglicht das lokale Testen und Entwickeln mit OpenShift.
- **Einfache Einrichtung**: Läuft als einzelne VM auf einem lokalen Rechner.
- **Entwicklungsfreundlich**: Perfekt für die Erstellung und das Testen von Anwendungen vor dem Deployment in größeren OpenShift-Umgebungen.
- **Einschränkungen**: Nicht für Produktionsumgebungen geeignet.

#### Installation von CRC:
1. Laden Sie CRC von der offiziellen Webseite herunter.
2. Installieren und starten Sie CRC:
   ```bash
   crc setup
   crc start
   ```

## Unterschiede und typische Szenarien: Lokale vs. Cloud-Deployments

### Lokale Deployments
Lokale Deployments sind ideal für:
- **Entwicklung und Tests**: Entwickeln Sie Anwendungen, bevor Sie sie in die Produktion überführen.
- **Einzelne Entwickler oder kleine Teams**: Minimale Ressourcenanforderungen.
- **Schnelle Iterationen**: Lokale Umgebungen sind schneller eingerichtet und angepasst.

Typische Tools:
- **CodeReady Containers (CRC)**: Für lokale Entwicklungsumgebungen.
- **MiniShift**: Älteres Tool, das OKD lokal bereitstellt.

### Cloud-Deployments
Cloud-Deployments sind ideal für:
- **Produktionsumgebungen**: Skalierbar und hochverfügbar.
- **Multi-Cluster-Verwaltung**: Verwaltung komplexer, verteilter Umgebungen.
- **Kosteneffizienz**: Nutzen Sie die Vorteile von On-Demand-Ressourcen in der Cloud.

Typische Szenarien:
- **Hybrid-Cloud**: Kombination aus lokalen und Cloud-Umgebungen für maximale Flexibilität.
- **Public Cloud**: Verwendung von OpenShift-Angeboten wie Red Hat OpenShift Service on AWS (ROSA) oder Azure Red Hat OpenShift.

## Überblick über EUS Releases und Channels: Update- und Supportstrategien

### Extended Update Support (EUS)
EUS ist ein optionaler Supportplan für OpenShift-Kunden, der verlängerte Supportzeiten für bestimmte Versionen bietet. Es bietet:
- **Langfristige Stabilität**: Kunden können länger auf einer bestimmten Version bleiben, ohne sofort aktualisieren zu müssen.
- **Geplante Updates**: Ermöglicht strategische Planung von Upgrades.

#### Typischer EUS-Zyklus:
- EUS-Releases werden etwa alle 18 Monate bereitgestellt.
- Ein EUS-Release wird für 2 Jahre unterstützt.

### Channels für Updates
OpenShift verwendet Channels, um verschiedene Upgrade-Strategien zu unterstützen:
1. **Stable Channel**:
   - Enthält stabile, getestete Updates.
   - Empfohlen für Produktionsumgebungen.

2. **Fast Channel**:
   - Bietet Zugriff auf neuere Funktionen.
   - Geeignet für Entwicklung und Testumgebungen.

### Best Practices für Updates:
- Verwenden Sie EUS, wenn Sie langfristige Stabilität benötigen.
- Testen Sie Updates zuerst in einer Entwicklungsumgebung.
- Automatisieren Sie Upgrades mit `oc adm upgrade`.
