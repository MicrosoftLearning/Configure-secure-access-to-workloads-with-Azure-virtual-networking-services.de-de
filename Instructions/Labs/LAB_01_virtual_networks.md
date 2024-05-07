---
lab:
  title: 'Übung: Bereitstellen von Netzwerkisolation und -segmentierung für die Webanwendung'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Lab: Bereitstellen von Netzwerkisolation und -segmentierung für die Webanwendung.

## Szenario

Die IT-Abteilung benötigt Netzwerkisolation und -segmentierung für die Webanwendung. Um die Netzwerkisolation und -segmentierung für die Webanwendung bereitzustellen, müssen Sie ein virtuelles Azure-Netzwerk mit Subnetzen erstellen, die das IT-Team bereitgestellt hat. Sobald das virtuelle Netzwerk erstellt ist, wird im nächsten Schritt das Peering virtueller Netzwerke konfiguriert. So können die virtuellen Netzwerke sicher und privat miteinander kommunizieren.

### Architekturdiagramm

![Diagramm, das zwei gepeerte virtuelle Netzwerke zeigt](../Media/task-1.png)

### Qualifikationsaufgaben

- Erstellen eines virtuellen Netzwerks
- Erstellen eines Subnetzes
- Konfigurieren des VNet-Peerings

## Übungsanweisungen

>**Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/) mit der zugewiesenen RBAC-Rolle **Contributor**.
> Wenn Sie in diesem Lab aufgefordert werden, eine Ressource zu erstellen, verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.

### Erstellen virtueller Netzwerke und Subnetze

Beginnen Sie mit dem Erstellen der im obigen Diagramm gezeigten Netzwerke.

1. Öffnen Sie einen Browser, navigieren Sie zum <a href="https://portal.azure.com/#home">Azure-Portal</a>, und melden Sie sich an.
1. Geben Sie zum Erstellen eines virtuellen Netzwerks oben im Portal in die Suchleiste **Virtuelle Netzwerke** ein, und wählen Sie in den Suchergebnissen **Virtuelle Netzwerke** aus.
1. Wählen Sie im Portalbereich **Virtuelle Netzwerke** die Option **+ Erstellen** aus.
1. Füllen Sie alle Registerkarten des Erstellungsprozesses aus, indem Sie die Werte in der folgenden Tabelle verwenden:

    | Eigenschaft             | Wert           |
    | :------------------- | :-------------- |
    | Ressourcengruppe       | **RG1**         |
    | Name                 | **app-vnet**    |
    | Region               | **USA, Osten**     |
    | IPv4-Adressraum   | **10.1.0.0/16** |
    | Subnetzname          | **frontend**    |
    | Subnetzadressbereich | **10.1.0.0/24** |
    | Subnetzname          | **backend**     |
    | Subnetzadressbereich | **10.1.1.0/24** |

    **Hinweis**: Behalten Sie für alle anderen Einstellungen die Standardwerte bei. Wählen Sie **Weiter** aus, um zur nächsten Registerkarte zu wechseln, und dann **Erstellen**, um das virtuelle Netzwerk zu erstellen.
1. Führen Sie die Schritte oben erneut aus, und Erstellen Sie das virtuelle Azure-Netzwerk **shared-services-vnet** mithilfe der Werte in der folgenden Tabelle:

    | Eigenschaft             | Wert                    |
    | :------------------- | :----------------------- |
    | Ressourcengruppe       | **RG1**                  |
    | Name                 | **shared-services-vnet** |
    | Region               | **USA, Osten**              |
    | IPv4-Adressraum   | **10.0.0.0/16**          |
    | Subnetzname          | **frontend**             |
    | Subnetzadressbereich | **10.0.0.0/24**          |

1. Nach Abschluss der Bereitstellung: Navigieren Sie zurück zum Portal, geben Sie in die Suchleiste **Ressourcengruppen** ein, und wählen Sie **Ressourcengruppen** in den Ergebnisse aus. Wählen Sie im Hauptbereich **RG1** aus, und bestätigen Sie, dass beide virtuellen Netzwerke bereitgestellt wurden.

### Einrichten einer Peerbeziehung zwischen den virtuellen Netzwerken

1. Durch das Einrichten einer Peer-Beziehung zwischen den beiden virtuellen Netzwerken wird Datenverkehr zwischen den virtuellen Netzwerken **app-vnet** und **shared-services-vnet** in beide Richtungen ermöglicht.
1. Im Portal in der Ressourcengruppenansicht für RG1. Wählen Sie als virtuelles Netzwerk **app-vnet** aus.
1. Scrollen Sie im Kontextmenü **app-vnet** auf der linken Seite des Portals nach unten, und wählen Sie **Peerings** aus.
1. Wählen Sie im Peerings-Bereich **app-vnet** die Option **+ Hinzufügen** aus.
1. Füllen Sie das Formular mit den Werten aus der folgenden Tabelle aus:

    | Eigenschaft                                 | Wert                          |
    | :--------------------------------------- | :----------------------------- |
    | Dieses virtuelle Netzwerk: Name des Peeringlinks   | **app-vnet-to-sharedservices** |
    | Virtuelles Remotenetzwerk: Name des Peeringlinks | **sharedservices-to-app-vnet** |
    | Virtuelles Netzwerk                          | **shared-services-vnet**       |

    **Hinweis**: Behalten Sie für alle anderen Einstellungen die Standardwerte bei. Wählen Sie **Hinzufügen** aus, um ein neues Peering virtueller Netzwerke zu erstellen.

    [Erfahren Sie mehr über das Peering virtueller Netzwerke](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal)

1. Nach Abschluss des Vorgangs und nach dem Update der Konfiguration. Überprüfen Sie, ob der **Peeringstatus** auf **Verbunden** festgelegt ist. (Möglicherweise müssen Sie die Seite aktualisieren, damit der aktualisierte Status angezeigt wird.)
