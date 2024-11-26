---
lab:
  title: 'Übung 04: Konfigurieren des Netzwerkroutings'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Übung 04: Konfigurieren des Netzwerkroutings

## Szenario

Um sicherzustellen, dass die Firewallrichtlinien erzwungen werden, muss ausgehender Anwendungsdatenverkehr über die Firewall geleitet werden. Sie identifizieren diese Anforderungen. 
+ Es ist eine Routentabelle erforderlich. Diese Routingtabelle wird den Frontend- und Back-End-Subnetzen zugeordnet.  
+ Ein Arbeitsplan ist erforderlich, um den gesamten ausgehenden IP-Datenverkehr von den Subnetzen zur Firewall zu filtern. Es wird die private IP-Adresse der Firewall verwendet. 

## Qualifikationsaufgaben

+ Erstellen und Konfigurieren einer Routingtabelle.
+ Verknüpfen Sie eine Routingtabelle mit einem Subnetz.
  
## Architekturdiagramm

![Diagramm, das ein virtuelles Netzwerk mit einer Firewall und einer Routingtabelle zeigt](../Media/task-3.png)


## Übungsanweisungen

### Erstellen einer Routingtabelle

Azure erstellt automatisch eine [Routentabelle](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) für jedes Subnetz innerhalb eines virtuellen Azure-Netzwerks. Die Tabelle enthält die Standard-[Systemrouten](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#system-routes). Sie können Routentabellen und Routen erstellen, um die standardmäßigen Systemrouten von Azure außer Kraft zu setzen.

**Aufzeichnen der privaten IP-Adresse von app-vnet-firewall**

1. Geben Sie in das Suchfeld am oberen Rand des Portals **Firewall** ein. Wählen Sie **Firewall** in den Suchergebnissen aus.

1. Wählen Sie **app-vnet-firewall** aus.

1. Wählen Sie **Übersicht** aus und notieren Sie sich die **Private IP-Adresse**.

**Hinzufügen der Routentabelle**

1. Geben Sie **Routingtabellen** in das Suchfeld ein. Wählen Sie „Routingtabelle“ aus, wenn diese Option in den Suchergebnissen angezeigt wird.

1. Wählen Sie auf der Seite Routentabelle **+ Erstellen** und erstellen Sie die Routentabelle. 

    | Eigenschaft       | Wert                        |
    | :------------- | :--------------------------- |
    | Abonnement   | **Wählen Sie Ihr Abonnement aus** |
    | Ressourcengruppe | **RG1**                      |
    | Region         | **USA, Osten**                  |
    | Name           | `app-vnet-firewall-rt`     |

1. Wählen Sie **Überprüfen und erstellen** und anschließend **Erstellen** aus.

1. Warten Sie, bis die Tabelle bereitgestellt wurde, und wählen Sie dann **Zur Ressource gehen** aus.  

### Zuordnen der Routingtabelle zu Subnetze

1. Arbeiten Sie im Portal weiter mit der Routentabelle, wählen Sie **app-vnet-firewall-rt**.

1. Im Blatt **Einstellungen** wählen Sie **Subnetze** und dann **+ Zuordnen**.

1. Konfigurieren Sie eine Zuordnung zum Frontend-Subnetz und wählen Sie dann **OK**.  

    | Eigenschaft        | Wert              |
    | :-------------- | :----------------- |
    | Virtuelles Netzwerk | **app-vnet** (RG1) |
    | Subnetz          | **frontend**       |

1. Konfigurieren Sie eine Zuordnung zum Backend-Subnetz und wählen Sie dann **OK**.  

    | Eigenschaft        | Wert              |
    | :-------------- | :----------------- |
    | Virtuelles Netzwerk | **app-vnet** (RG1) |
    | Subnetz          | **frontend**       |

### Erstellen einer Route in einer Routingtabelle

1. Arbeiten Sie im Portal weiter mit der Routentabelle, wählen Sie **app-vnet-firewall-rt**.

1. Im Blatt **Einstellungen** wählen Sie **Routen** und dann **+ Hinzufügen**.

1. Konfigurieren Sie die Route und wählen Sie dann **Hinzufügen**. 

    | Eigenschaft                            | Wert                                                   |
    | :---------------------------------- | :------------------------------------------------------ |
    | Routenname                          | **outbound-firewall**                                   |
    | Zieltyp                    | **IP-Adressen**                                        |
    | Ziel-IP-Adressen/CIDR-Bereich | **0.0.0.0/0**                                           |
    | Typ des nächsten Hops                       | **Virtuelles Gerät**                                   |
    | Adresse des nächsten Hops                    | **private IP-Adresse der Firewall** |


### Erfahren Sie mehr in Onlineschulungen

+ [Verwalten und Steuern des Datenverkehrsflusses mit Routen in Ihrer Azure-Bereitstellung](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/): In diesem Modul lernen Sie, wie Sie den virtuellen Netzwerkverkehr von Azure durch die Implementierung benutzerdefinierter Routen steuern können. Dieses Modul verfügt über zwei Sandbox-Umgebungen. 

### Wichtige Erkenntnisse

Herzlichen Glückwunsch zum Abschluss der Übung. Hier sind die wichtigsten Erkenntnisse:

+ Der Netzwerkdatenverkehr wird automatisch über Azure-Subnetze, virtuelle Netzwerke und lokale Netzwerke geleitet. Systemrouten steuern dieses Routing.
+ Benutzerdefinierte Routen setzen die Standard-Systemrouten außer Kraft, so dass der Datenverkehr über virtuelle Netzwerkgeräte (NVAs) geleitet werden kann. 
+ Virtuelle Netzwerkgeräte (NVAs) kontrollieren den Flow des Netzwerkverkehrs. Beispiele für NVAs sind Firewalls, Lastverteiler und Router.
+ Routentabellen enthalten Routing-Informationen und sind einem Subnetz zugeordnet. 
