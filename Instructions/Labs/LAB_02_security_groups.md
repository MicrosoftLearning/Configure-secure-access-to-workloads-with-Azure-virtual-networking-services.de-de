---
lab:
  title: 'Übung 02: Erstellen und Konfigurieren von Netzwerksicherheitsgruppen'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Übung 02: Erstellen und Konfigurieren von Netzwerksicherheitsgruppen

## Szenario

Ihre Organisation erfordert, dass der Netzwerkdatenverkehr in app-vnet streng kontrolliert wird. Sie identifizieren diese Anforderungen.
+ Das Front-End-Subnetz enthält Webserver, auf die über das Internet zugegriffen werden kann. Für diese Server ist eine **Anwendungssicherheitsgruppe** (ASG) erforderlich. Der ASG sollte jeder Schnittstelle des virtueller Computers zugeordnet werden, die Teil der Gruppe ist. Dadurch können die Webserver einfach verwaltet werden. 
+ Das Back-End-Subnetz enthält Datenbankserver, die von den Front-End-Webservern verwendet werden. Eine **Netzwerksicherheitsgruppe** (NSG) ist erforderlich, um diesen Verkehr zu kontrollieren. Die NSG sollte jeder VM-Schnittstelle zugeordnet werden, auf die von den Webservern zugegriffen wird. 
+ Zum Testen sollte ein virtueller Computer im Frontend-Subnetz (VM1) und im Backend-Subnetz (VM2) installiert werden.  Die IT-Gruppe hat eine Azure Resource Manager-Vorlage vorbereitet, um diese **Ubuntu-Server** bereitzustellen. 

## Qualifikationsaufgabe

+ Erstellen Sie eine Netzwerksicherheitsgruppe.
+ Erstellen von Regeln für Netzwerksicherheitsgruppen.
+ Zuordnen einer Netzwerksicherheitsgruppe zu einem Subnetz.
+ Erstellen und Verwenden von Anwendungssicherheitsgruppen in Netzwerksicherheitsgruppenregeln.

## Architekturdiagramm

![Diagramm, das eine ASG und eine NSG zeigt, die mit einem virtuellen Netzwerk verbunden sind](../Media/task-2.png)




## Übungsanweisungen

### Erstellen der Netzwerkinfrastruktur für die Übung

**Hinweis:** Für diese Übung ist es erforderlich, dass die virtuellen Netzwerke und Subnetze von Lab 01 installiert sind. Eine [Vorlage](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Allfiles/Labs/All-Labs/create-vnet-subnets-template.json) wird zur Verfügung gestellt, wenn Sie diese Ressourcen bereitstellen müssen.

1. Verwenden Sie das Symbol (oben rechts), um eine **Cloud Shell**-Sitzung zu starten. Navigieren Sie alternativ direkt zu `https://shell.azure.com`.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus.

1. Für diese Aufgabe ist kein Speicherplatz erforderlich Wählen Sie Ihr Abonnement aus. **Wenden Sie Ihre Änderungen an**. 

1. Verwenden Sie diese Befehle, um die für diese Übung erforderlichen virtuellen Computer bereitzustellen.

>**Hinweis**: Wenn die Bereitstellung aufgrund von Kapazitätsbeschränkungen fehlschlägt, bearbeiten Sie die Vorlage und ändern Sie den Wert „Standort“. 

   ```powershell
   $RGName = "RG1"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
   ```
  
1. Suchen Sie im Portal die Option `virtual machines`, und wählen Sie sie aus. Überprüfen Sie, ob sowohl vm1 als auch vm2 **Laufen** sind.

### Erstellen von Anwendungssicherheitsgruppen

[Anwendungssicherheitsgruppen (ASGs)](https://learn.microsoft.com/azure/virtual-network/application-security-groups) ermöglichen die Gruppierung von Servern mit ähnlichen Funktionalitäten. Zum Beispiel alle Webserver, die Ihre Anwendung hosten. 

1. Suchen Sie im Portal die Option `Application security groups`, und wählen Sie sie aus.
   
1. Wählen Sie **+ Erstellen** aus und konfigurieren Sie die Anwendungssicherheitsgruppe. 

    | Eigenschaft       | Wert                        |
    | :------------- | :--------------------------- |
    | Abonnement   | **Wählen Sie Ihr Abonnement aus** |
    | Ressourcengruppe | **RG1**                      |
    | Name           | `app-frontend-asg`          |
    | Region         | **USA, Osten**                  |

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

**Hinweis**: Sie erstellen die Anwendungssicherheitsgruppe in derselben Region, in der sich auch das vorhandene virtuelle Netzwerk befindet.

**Zuordnen der Anwendungssicherheitsgruppe zur Netzwerkschnittstelle der VM**

1. Suchen Sie im Azure-Portal nach `VM1` und wählen Sie es aus.

1. Wählen Sie im Blatt **Netzwerke** die Option **Anwendungssicherheitsgruppen** und anschließend die Option **Anwendungssicherheitsgruppen hinzufügen** aus.

1. Markieren Sie die **app-frontend-asg** und wählen Sie dann **Hinzufügen**.
   
### Erstellen uns Zuordnen der Netzwerksicherheitsgruppe

[Netzwerksicherheitsgruppen (NSGs)](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview) sichern den Netzwerkverkehr in einem virtuellen Netzwerk. 

1. Suchen Sie im Portal die Option `Network security group`, und wählen Sie sie aus.

1. Wählen Sie **+ Erstellen** und konfigurieren Sie die Netzwerksicherheitsgruppe. 

    | Eigenschaft       | Wert                        |
    | :------------- | :--------------------------- |
    | Abonnement   | **Wählen Sie Ihr Abonnement aus** |
    | Ressourcengruppe | **RG1**                      |
    | Name           | `app-vnet-nsg`            |
    | Region         | **USA, Osten**                  |

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

**Zuordnen des NSG zum App-vnet-Backend-Subnetz.**

NSGs können Subnetzen und/oder einzelnen Netzwerkschnittstellen zugeordnet werden, die mit virtuellen Computern von Azure verbunden sind. 

1. Wählen Sie **Zur Ressource gehen** oder navigieren Sie zur Ressource **app-vnet-nsg**.

1. Wählen Sie auf dem Blatt **Einstellungen** die Option **Subnetze** aus.

1. Wählen Sie **+ Zuordnen** aus.

1. Wählen Sie **app-vnet (RG1)** und dann das **Backend**-Subnetz. Wählen Sie **OK** aus.

### Erstellen von Regeln für Netzwerksicherheitsgruppen

Ein NSG verwendet [Sicherheitsregeln](https://learn.microsoft.com/azure/virtual-network/network-security-group-how-it-works), um ein- und ausgehenden Netzwerkverkehr zu filtern. 

1. Geben Sie in das Suchfeld am oberen Rand des Portals **Netzwerksicherheitsgruppen** ein. Wählen Sie in den Suchergebnissen Netzwerksicherheitsgruppen aus.

1. Wählen Sie in der Liste der Netzwerksicherheitsgruppen **app-vnet-nsg** aus.

1. Wählen Sie im Blatt **Einstellungen** die Option **Eingehende Sicherheitsregeln**.

1. Wählen Sie **+ Hinzufügen** und konfigurieren Sie eine eingehende Sicherheitsregel. 

    | Eigenschaft                               | Value                          |
    | :------------------------------------- | :----------------------------- |
    | Quelle                                 | **Alle**                        |
    | Quellportbereiche                     | **\***                         |
    | Ziel                            | **Anwendungssicherheitsgruppe** |
    | Ziel-Anwendungssicherheitsgruppe | **app-frontend-asg**            |
    | Dienst                                | **SSH**                        |
    | Aktion                                 | **Zulassen**                      |
    | Priorität                               | **100**                        |
    | Name                                   | **AllowSSH**                   |


### Erfahren Sie mehr in Onlineschulungen

+ [Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe über das Azure-Portal](https://learn.microsoft.com/training/modules/filter-network-traffic-network-security-group-using-azure-portal/): In diesem Modul konzentrieren Sie sich auf die Filterung des Netzwerkverkehrs mithilfe von Netzwerksicherheitsgruppen (NSGs) im Azure-Portal. Erfahren Sie, wie Sie NSGs erstellen, konfigurieren und anwenden, um die Netzwerksicherheit zu verbessern.
+ [Schützen und Isolieren des Zugriffs auf Azure-Ressourcen mithilfe von Netzwerksicherheitsgruppen und Dienstendpunkten.](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/) In diesem Modul erfahren Sie etwas über Netzwerksicherheitsgruppen und wie Sie die Netzwerkkonnektivität einschränken können. 

### Wichtige Erkenntnisse

Herzlichen Glückwunsch zum Abschluss der Übung. Hier sind die wichtigsten Erkenntnisse:

+ Mit Anwendungssicherheitsgruppen können Sie virtuelle Computer organisieren und Richtlinien für die Netzwerksicherheit auf der Grundlage der Anwendungen Ihres Unternehmens definieren.
+ Eine Azure-Netzwerksicherheitsgruppe wird zur Filterung des Netzwerkverkehrs zwischen Azure-Ressourcen in einem virtuellen Azure-Netzwerk verwendet.
+ Sie können jedem Subnetz eines virtuellen Netzwerks und jeder Netzwerkschnittstelle eines virtuellen Computers keine oder eine Netzwerksicherheitsgruppe zuordnen. 
+ Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln, die eingehenden Netzwerkverkehr zu oder ausgehenden Netzwerkverkehr von Azure-Ressourcen zulassen oder verweigern.
+ Sie verbinden virtuelle Computer mit einer Anwendungssicherheitsgruppe. Anschließend verwenden Sie die Anwendungssicherheitsgruppe als Quelle oder Ziel in den Regeln für Netzwerksicherheitsgruppen.



