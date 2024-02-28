---
lab:
  title: 'Übung: Weiterleiten von Datenverkehr an die Firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Lab: Weiterleiten von Datenverkehr an die Firewall


## Szenario

Nachdem nun eine Firewall mit Richtlinien eingerichtet wurde, die die Sicherheitsanforderungen Ihrer Organisation erzwingen, müssen Sie Ihren Netzwerkdatenverkehr an das Firewallsubnetz weiterleiten, damit es den Datenverkehr filtern und überprüfen kann. Routingtabellen ermöglichen die Steuerung des Routings von Netzwerkdatenverkehr zu und von der Webanwendung. Die Firewallregeln werden auf den Netzwerkdatenverkehr angewendet, wenn Sie Ihren Netzwerkdatenverkehr als Subnetz-Standardgateway an die Firewall weiterleiten. 

### Architekturdiagramm


![Diagramm, das ein virtuelles Netzwerk mit einer Firewall und einer Routingtabelle zeigt](../Media/task-3.png)

### Qualifikationsaufgaben

- Erstellen und Konfigurieren einer Routingtabelle.
- Verknüpfen Sie eine Routingtabelle mit einem Subnetz.
  

## Übungsanweisungen

### Erstellen einer Routingtabelle

1. Notieren Sie die private und öffentliche IP-Adresse von **app-vnet-firewall**.

    1. Geben Sie in das Suchfeld am oberen Rand des Portals **Firewall** ein. Wählen Sie **Firewall** in den Suchergebnissen aus.

    1. Wählen Sie **app-vnet-firewall** aus.

    1. Wählen Sie **Übersicht** aus.

        1. Notieren Sie die **private IP-Adresse**.

    1. Wählen Sie im Bereich „Übersicht“ **fwpip** aus.

    1. Notieren Sie die **öffentliche IP-Adresse**. 


1. Geben Sie **Routingtabellen** in das Suchfeld ein. Wählen Sie „Routingtabelle“ aus, wenn diese Option in den Suchergebnissen angezeigt wird.

1. Wählen Sie auf der Seite „Routingtabellen“ **+ Erstellen** aus.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** von „Routingtabelle erstellen“ die Informationen aus der folgenden Tabelle ein.

    | Eigenschaft | Wert    |
    |:---------|:---------|
    |Abonnement|**Wählen Sie Ihr Abonnement aus**|
    |Ressourcengruppe|**RG1**|
    |Region|**USA, Osten**|
    |Name|**app-vnet-firewall-rt**|

    

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

    [Erfahren Sie mehr über das Erstellen von Routingtabellen](https://docs.microsoft.com/azure/virtual-network/manage-route-table) und das [Zuordnen einer Routingtabelle zu einem Subnetz](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#associate-a-route-table-to-a-subnet).

### Zuordnen der Routingtabelle zu Subnetze

1. Geben Sie **Routingtabellen** in das Suchfeld ein. Wählen Sie in den Suchergebnissen den Eintrag „Routingtabellen“ aus.

1. Wählen Sie **app-vnet-firewall-rt** aus.

1. Wählen Sie **Subnetze** aus.

1. Wählen Sie **+ Zuordnen** aus.

1. Geben Sie auf der Seite **Subnetz zuordnen** die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft | Wert    |
    |:---------|:---------|
    |Virtuelles Netzwerk|**app-vnet** (RG1)|
    |Subnetz|**frontend**|

1. Wählen Sie **OK** aus.

1. Wiederholen Sie die obigen Schritte, um die Routingtabelle **app-vnet-firewall-rt** dem Subnetz **backend** in **app-vnet** zuzuordnen.

### Erstellen einer Route in einer Routingtabelle

1. Geben Sie **Routingtabellen** in das Suchfeld ein. Wählen Sie in den Suchergebnissen den Eintrag „Routingtabellen“ aus.

1. Wählen Sie **app-vnet-firewall-rt** aus.

1. Wählen Sie die Option **Routen**.

1. Wählen Sie **+ Hinzufügen** aus.

1. Geben Sie auf der Seite **Route hinzufügen** die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft | Wert    |
    |:---------|:---------|
    |Routenname|**outbound-firewall**|
    |Zieltyp|**IP-Adressen**|
    |Ziel-IP-Adressen/CIDR-Bereich|**0.0.0.0/0**|
    |Typ des nächsten Hops|**Virtuelles Gerät**|
    |Adresse des nächsten Hops|**private IP-Adresse der zuvor erfassten Firewall**|


1. Wählen Sie **Hinzufügen** aus.

[Erfahren Sie mehr über das Erstellen von Routen](https://docs.microsoft.com/azure/virtual-network/manage-route-table#add-a-route).

Der ausgehende Datenverkehr vom Front-End- und Back-End-Subnetz wird nun an die Firewall weitergeleitet. 


