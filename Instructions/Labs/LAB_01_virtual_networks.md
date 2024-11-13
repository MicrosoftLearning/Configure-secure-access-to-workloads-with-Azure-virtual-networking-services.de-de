---
lab:
  title: 'Übung 01: Erstellen und Konfigurieren virtueller Netzwerke'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Übung 01: Erstellen und Konfigurieren virtueller Netzwerke

## Szenario

Ihre Organisation migriert eine webbasierte Anwendung zu Azure. Ihre erste Aufgabe besteht darin, die virtuellen Netzwerke und Subnetze zu platzieren. Außerdem müssen die virtuellen Netzwerke sicher gepeert werden. Sie ermitteln diese Anforderungen. 
+ Zwei virtuelle Netzwerke sind erforderlich, **app-vnet** und **hub-vnet**. Dies simuliert eine Hub-and-Spoke-Netzwerkarchitektur. 
+ Das app-vnet wird die Anwendung hosten. Für dieses virtuelle Netzwerk sind zwei Subnetze erforderlich. Das **Frontend-Subnetz** wird die Webserver hosten. Das **Backend-Subnetz** wird die Datenbankserver hosten.
+ Das hub-vnet erfordert nur ein Subnetz für die Firewall. 
+ Die beiden virtuellen Netzwerke müssen in der Lage sein, über **virtuelles Netzwerk Peering** sicher und privat miteinander zu kommunizieren. 
+ Beide virtuellen Netze sollten sich in derselben Region befinden. 

## Qualifikationsaufgabe

+ Erstellen Sie ein virtuelles Netzwerk.
+ Erstellen Sie ein Subnetz.
+ Konfigurieren des VNet-Peerings.

## Architekturdiagramm

![Diagramm, das zwei gepeerte virtuelle Netzwerke zeigt](../Media/task-1.png)

## Übungsanweisungen

**Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/) mit der zugewiesenen RBAC-Rolle **Contributor**. Wenn Sie in diesem Lab aufgefordert werden, eine Ressource zu erstellen, verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.

### Erstellen von Hub-and-Spoke-VNets und -Subnetzen

Ein [Azure Virtual Network](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview) aktiviert viele Typen von Azure-Ressourcen für die sichere Kommunikation untereinander, mit dem Internet und mit lokalen Netzwerken. Alle Azure-Ressourcen in einem virtuellen Netzwerk werden in [Subnetzen](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-subnet?tabs=azure-portal) innerhalb des virtuellen Netzwerks bereitgestellt. 

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.
   
1. Suchen Sie nach `Virtual Networks`, und wählen Sie diese Option aus.
   
1. Wählen Sie **+ Erstellen** und schließen Sie die Konfiguration des **app-vnet** ab. Dieses virtuelle Netzwerk benötigt zwei Subnetze, **Frontend** und **Backend**. 

    | Eigenschaft             | Wert           |
    | :------------------- | :-------------- |
    | Ressourcengruppe       | **RG1**         |
    | Name des virtuellen Netzwerks | `app-vnet`    |
    | Region               | **USA, Osten**     |
    | IPv4-Adressraum   | **10.1.0.0/16** |
    | Subnetzname          | `frontend`    |
    | Subnetzadressbereich | **10.1.0.0/24** |
    | Subnetzname          | `backend`     |
    | Subnetzadressbereich | **10.1.1.0/24** |

    **Hinweis**:Belassen Sie alle anderen Einstellungen auf ihren Standardwerten. Wählen Sie nach Abschluss **Überprüfen + erstellen** und danach **Erstellen**.
   
1. Erstellen Sie die virtuelle Netzwerkkonfiguration **Hub-vnet**. Dieses virtuelle Netzwerk enthält das Firewall-Subnetz. 

    | Eigenschaft             | Wert                    |
    | :------------------- | :----------------------- |
    | Ressourcengruppe       | **RG1**                  |
    | Name                 | `hub-vnet` |
    | Region               | **USA, Osten**              |
    | IPv4-Adressraum   | **10.0.0.0/16**          |
    | Subnetzname          | **AzureFirewallSubnet**  |
    | Subnetzadressbereich | **10.0.0.0/24**          |

1. Sobald die Bereitstellung abgeschlossen ist, suchen Sie nach Ihren „virtuellen Netzwerken“ und wählen diese aus.

1. Überprüfen Sie, ob Ihre virtuellen Netzwerke und Subnetze bereitgestellt wurden. 

### Konfigurieren einer Peer-Beziehung zwischen den virtuellen Netzwerken

[Peering virtueller Netzwerke](https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview) aktiviert die nahtlose Verbindung von zwei oder mehr virtuellen Netzwerken in Azure. 

1. Suchen Sie das virtuelle Netzwerk `app-vnet` und wählen Sie es aus.
   
1. Wählen Sie im Blatt **Einstellungen** die Option **Peerings**.
   
1. Wählen Sie die Schaltfläche **+ Hinzufügen** eines Peerings zwischen den beiden virtuellen Netzwerken. 

    | Eigenschaft                                 | Wert                          |
    | :--------------------------------------- | :----------------------------- |
    | Name des Remote-Peeringlinks              | `app-vnet-to-hub` |
    | Virtuelles Netzwerk    | `hub-vnet` |
    | Name der Peering-Verbindung des lokalen virtuellen Netzwerks | `hub-to-app-vnet` |

    **Hinweis**: Behalten Sie für alle anderen Einstellungen die Standardwerte bei. Wählen Sie **Hinzufügen** aus, um ein neues Peering virtueller Netzwerke zu erstellen.

1. Sobald die Bereitstellung abgeschlossen ist, überprüfen Sie, ob der **Peering-Status** **Verbunden** lautet.

## Erfahren Sie mehr in Onlineschulungen

+ [Einführung in Azure Virtual Network](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/): In diesem Modul erfahren Sie, wie Sie Azure-Netzwerkdienste entwerfen und implementieren. Sie erfahren mehr über virtuelle Netzwerke, öffentliche und private IPs, DNS, virtuelles Netzwerk-Peering, Routing und Azure Virtual NAT.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum Abschluss der Übung. Hier sind die wichtigsten Erkenntnisse:

+ Virtuelle Netzwerke (VNets) von Azure stellen eine sichere und isolierte Cloudumgebung für Ihre Ressourcen bereit. Sie können pro Region und Abonnement mehrere virtuelle Netzwerke erstellen.
+ Beim Entwurf virtueller Netzwerke sollten Sie darauf achten, dass sich der VNet-Adressraum (CIDR-Block) nicht mit den anderen Netzwerkbereichen Ihres Unternehmens überschneidet.
+ Ein Subnetz ist ein Bereich von IP-Adressen im VNet. Sie können VNets in unterschiedlich große Subnetze unterteilen und so viele Subnetze erstellen, wie Sie für die Organisation und Sicherheit innerhalb des Abonnementlimits benötigen. Jedes Subnetz muss einen eindeutigen Adressbereich haben.
+ Bestimmte Azure-Dienste, wie z. B. Azure Firewall, erfordern ein eigenes Subnetz.
+ Das Peering virtueller Netzwerke ermöglicht das nahtlose Verbinden zweier virtueller Azure-Netzwerke. Die virtuellen Netzwerke werden für Verbindungszwecke als einzelnes Element angezeigt.
