---
lab:
  title: 'Übung: Bereitstellen von Netzwerkisolation und -segmentierung für die Webanwendung'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Lab: Bereitstellen eines virtuellen Netzwerks für gemeinsame Dienste mit Isolation und Segmentierung

## Szenario

Sie wurden beauftragt, [Zero-Trust-Prinzipien](https://learn.microsoft.com/security/zero-trust/azure-infrastructure-networking) auf ein virtuelles Hubnetzwerk in Azure anzuwenden. Die IT-Abteilung benötigt Netzwerkisolation und -segmentierung für die Webanwendung in einem Spokenetzwerk. Um die Netzwerkisolation und -segmentierung für die Webanwendung bereitzustellen, müssen Sie ein virtuelles Azure-Netzwerk mit Subnetzen mit Adressraum erstellen, den das IT-Team bereitgestellt hat. Sobald das virtuelle Netzwerk erstellt ist, wird im nächsten Schritt das Peering virtueller Netzwerke konfiguriert. So können die virtuellen Netzwerke sicher und privat miteinander kommunizieren.

### Architekturdiagramm

![Diagramm, das zwei gepeerte virtuelle Netzwerke zeigt](../Media/task-1.png)

### Qualifikationsaufgaben

- Erstellen eines virtuellen Netzwerks
- Erstellen eines Subnetzes
- Konfigurieren des VNet-Peerings

## Übungsanweisungen

>**Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/) mit der zugewiesenen RBAC-Rolle **Contributor**.

> Wenn Sie in diesem Lab aufgefordert werden, eine Ressource zu erstellen, verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.

### Erstellen von Hub-and-Spoke-VNets und -Subnetzen

Beginnen Sie mit dem Erstellen der virtuellen Netzwerke, die im obigen Diagramm gezeigt werden.

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

    **Hinweis**: Behalten Sie für alle anderen Einstellungen die Standardwerte bei. Wählen Sie nach Abschluss **Überprüfen + erstellen** und danach **Erstellen**.
   
1. Erstellen Sie die virtuelle Netzwerkkonfiguration **Hub-vnet**. Dieses virtuelle Netzwerk enthält das Firewall-Subnetz. 

    | Eigenschaft             | Wert                    |
    | :------------------- | :----------------------- |
    | Ressourcengruppe       | **RG1**                  |
    | Name                 | `hub-vnet` |
    | Region               | **USA, Osten**              |
    | IPv4-Adressraum   | **10.0.0.0/16**          |
    | Subnetzname          | **AzureFirewallSubnet**  |
    | Subnetzadressbereich | **10.0.0.0/24**          |

1. Sobald die Bereitstellungen abgeschlossen sind, suchen Sie nach Ihrer **Ressourcengruppe** und wählen Sie diese aus. Vergewissern Sie sich, dass Ihre neuen virtuellen Netzwerke Teil der Ressourcengruppe sind. 

### Konfigurieren einer Peer-Beziehung zwischen den virtuellen Netzwerken

1. Suchen Sie das virtuelle Netzwerk `app-vnet` und wählen Sie es aus.
   
1. Wählen Sie im Blatt **Einstellungen** die Option **Peerings**.
   
1. Wählen Sie die Schaltfläche **+ Hinzufügen** eines Peerings zwischen den beiden virtuellen Netzwerken. 

    | Eigenschaft                                 | Wert                          |
    | :--------------------------------------- | :----------------------------- |
    | Name des Peeringlinks              | `app-vnet-to-hub` |
    | Virtuelles Netzwerk    | `hub-vnet` |
    | Name der Peering-Verbindung des lokalen virtuellen Netzwerks | `hub-to-app-vnet` |

    **Hinweis**: Behalten Sie für alle anderen Einstellungen die Standardwerte bei. Wählen Sie **Hinzufügen** aus, um ein neues Peering virtueller Netzwerke zu erstellen.

    [Erfahren Sie mehr über das Peering virtueller Netzwerke](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal)

1. Sobald die Bereitstellung abgeschlossen ist, überprüfen Sie, ob der **Peering-Status** als **Verbunden** angezeigt wird. 
