---
lab:
  title: 'Übung: Steuern des ein- und ausgehenden Netzwerkdatenverkehrs der Webanwendung'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Lab: Steuern des ein- und ausgehenden Netzwerkdatenverkehrs der Webanwendung

## Szenario

Ihre Organisation benötigt die Steuerung des ein- und ausgehenden Netzwerkdatenverkehrs der Webanwendung. Um die Sicherheit der Webanwendung weiter zu erhöhen, können Netzwerksicherheitsgruppen (NSG) und Anwendungssicherheitsgruppen (ASG) konfiguriert werden. NSG ist eine Sicherheitsebene, die ein- und ausgehenden Netzwerkdatenverkehr von Azure-Ressourcen filtert, während ASG die Gruppierung von Ressourcen zur gemeinsamen Verwaltung ermöglicht. Diese Sicherheitsgruppen bieten eine differenzierte Steuerung des ein- und ausgehenden Netzwerkdatenverkehrs der Webanwendungskomponenten.

### Architekturdiagramm

![Diagramm, das eine ASG und eine NSG zeigt, die mit einem virtuellen Netzwerk verbunden sind](../Media/task-2.png)

### Qualifikationsaufgaben

- Erstellen einer NSG.
- Erstellen von NSG-Regeln.
- Zuordnen einer NSG zu einem Subnetz.
- Erstellen und Verwenden von Anwendungssicherheitsgruppen in NSG-Regeln.

## Übungsanweisungen

### Erstellen von Anwendungssicherheitsgruppen

Mithilfe einer Anwendungssicherheitsgruppe (ASG) können Sie Server mit ähnlichen Funktionen gruppieren (z. B. Webserver).

1. Geben Sie im Suchfeld oben im Portal **Anwendungssicherheitsgruppe** ein. Wählen Sie in den Suchergebnissen Anwendungssicherheitsgruppen aus.

1. Wählen Sie **+ Erstellen** aus.

    Geben Sie auf der Registerkarte **Grundeinstellungen** von „Anwendungssicherheitsgruppe erstellen“ die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft | Wert    |
    |:---------|:---------|
    |Abonnement|**Wählen Sie Ihr Abonnement aus**|
    |Ressourcengruppe|**RG1**|
    |Name|**app-backend-asg**|
    |Region|**USA, Osten**|

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

[Erfahren Sie mehr über das Erstellen einer Anwendungssicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-application-security-groups).

>**Hinweis**: Sie erstellen die Anwendungssicherheitsgruppe in derselben Region, in der sich auch das vorhandene virtuelle Netzwerk befindet.

### Erstellen uns Zuordnen der Netzwerksicherheitsgruppe

Eine Netzwerksicherheitsgruppe (NSG) schützt den Netzwerkdatenverkehr in Ihrem virtuellen Netzwerk. NSGs enthalten eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit virtuellen Azure-Netzwerken (VNet) verbunden sind, zugelassen oder abgelehnt wird. NSGs können Subnetzen und/oder einzelnen Netzwerkschnittstellen zugeordnet werden, die mit virtuellen Computern (VM) verbunden sind.

1. Geben Sie im Suchfeld oben im Portal **Netzwerksicherheitsgruppen** ein. Wählen Sie in den Suchergebnissen **Netzwerksicherheitsgruppen** aus.

1. Wählen Sie **+ Erstellen** aus.

    Geben Sie auf der Registerkarte **Grundeinstellungen** von „Netzwerksicherheitsgruppe erstellen“ die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft | Wert    |
    |:---------|:---------|
    |Abonnement|**Wählen Sie Ihr Abonnement aus**|
    |Ressourcengruppe|**RG1**|
    |Name|**app-vnet-nsg**|
    |Region|**USA, Osten**|

    [Erfahren Sie mehr über das Erstellen einer Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-a-network-security-group).

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

In diesem Abschnitt ordnen Sie die Netzwerksicherheitsgruppe dem Subnetz des zuvor erstellten virtuellen Netzwerks zu.

1. Geben Sie im Suchfeld oben im Portal **Netzwerksicherheitsgruppen** ein. Wählen Sie in den Suchergebnissen Netzwerksicherheitsgruppen aus.

1. Wählen Sie in der Liste der Netzwerksicherheitsgruppen **app-vnet-nsg** aus.

1. Wählen Sie **Subnetze** im Abschnitt „Einstellungen“ von **app-vnet-nsg** aus.

1. Wählen Sie auf der Seite „Subnetze“ die Option **+ Zuordnen** aus.

1. Wählen Sie unter **Subnetz zuordnen** die Option **app-vnet (RG1)** für „Virtuelles Netzwerk“ aus, und wählen Sie **Back-End** für „Subnetz“ und dann „OK“ aus.

    [Erfahren Sie mehr über das Zuordnen einer Netzwerksicherheitsgruppe zu einem Subnetz](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#associate-a-network-security-group-to-a-subnet).

### Erstellen von Regeln für Netzwerksicherheitsgruppen
Eine Netzwerksicherheitsgruppe (NSG) schützt den Netzwerkdatenverkehr in Ihrem virtuellen Netzwerk.

1. Geben Sie im Suchfeld oben im Portal **Netzwerksicherheitsgruppen** ein. Wählen Sie in den Suchergebnissen Netzwerksicherheitsgruppen aus.

1. Wählen Sie in der Liste der Netzwerksicherheitsgruppen **app-vnet-nsg** aus.

1. Wählen Sie im Abschnitt „Einstellungen“ von **app-vnet-nsg** die Option **Eingangssicherheitsregeln** aus.

1. Wählen Sie **+ Hinzufügen** aus.

1. Geben Sie auf der Seite **Eingangssicherheitsregel hinzufügen** die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft | Wert    |
    |:---------|:---------|
    |Quelle|**Alle**|
    |Quellportbereiche|**\***|
    |Ziel|**Anwendungssicherheitsgruppe**|
    |Ziel-Anwendungssicherheitsgruppe|**app-backend-asg**|    
    |Dienst|**SSH**|
    |Aktion|**Zulassen**|
    |Priorität|**100**|
    |Name|**AllowSSH**|

    [Erfahren Sie mehr über das Erstellen einer Netzwerksicherheitsgruppen-Regel](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-a-network-security-group).

### Bereitstellen einer ARM-Vorlage mit Cloud Shell zum Erstellen der für diese Übung erforderlichen VMs

1. Öffnen Sie **Azure Cloud Shell** im Azure-Portal, indem Sie rechts oben im Azure-Portal das entsprechende Symbol auswählen.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus.

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **Für Sie wurde kein Speicher bereitgestellt** angezeigt wird, wählen Sie das in diesem Lab verwendete Abonnement aus, und wählen Sie dann **Speicher erstellen** aus.

1. Stellen Sie die folgenden ARM-Vorlagen mit Cloud Shell bereit, um die für diese Übung erforderlichen VMs zu erstellen:

>**Hinweis**: Sie können den Text im folgenden Abschnitt auswählen und ihn in die Cloud Shell kopieren/einfügen.

   ```powershell
   $RGName = "RG1"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
   ```
  
1. Um zu überprüfen, ob die virtuellen Computer **VM1** und **VM2** ausgeführt werden, navigieren Sie zur Ressourcengruppe **RG1**, und wählen Sie **VM1** aus.

1. Überprüfen Sie, ob der Status des virtuellen Computers **Ausgeführt** lautet.

1. Wiederholen Sie den vorherigen Schritt für **VM2**.

### Zuordnen der Anwendungssicherheitsgruppe zur Netzwerkschnittstelle der VM
Als Sie die VMs erstellt haben, hat Azure eine Netzwerkschnittstelle für jede VM erstellt und an die VM angefügt.

Fügen Sie die Netzwerkschnittstelle von VM2 die Anwendungssicherheitsgruppe hinzu, die Sie zuvor erstellt haben.

1. Navigieren Sie im Azure-Portal zu Ihrer Ressourcengruppe **RG1**, und wählen Sie **VM2** aus.

1. Navigieren Sie zur Registerkarte „Netzwerk“ der VM, und wählen Sie **+ Anwendungssicherheitsgruppen hinzufügen** im Abschnitt **Anwendungssicherheitsgruppen** aus. 

1. Wählen Sie **app-backend-asg** in der Liste der Anwendungssicherheitsgruppen aus.

1. Wählen Sie **Hinzufügen** aus.

  [Erfahren Sie mehr über das Hinzufügen einer NIC zu einer Anwendungssicherheitsgruppe](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface?tabs=azure-portal#add-or-remove-from-application-security-groups).

