---
lab:
  title: 'Übung: Schützen der Webanwendung vor böswilligem Datenverkehr und Blockieren des nicht autorisierten Zugriffs'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Lab: Schützen der Webanwendung vor böswilligem Datenverkehr und Blockieren des nicht autorisierten Zugriffs.

## Szenario

Ihre Organisation möchte die Webanwendung vor böswilligem Datenverkehr schützen und nicht autorisierten Zugriff blockieren.

Zusätzlich zu NSG und ASG kann eine Firewall konfiguriert werden, um der Webanwendung eine zusätzliche Sicherheitsebene hinzuzufügen. Eine Firewall schützt die Webanwendung vor böswilligem Datenverkehr und blockiert den nicht autorisierten Zugriff mit von Ihnen konfigurierten Richtlinien.

Die Azure Firewallrichtlinie ist eine Ressource der obersten Ebene, die Sicherheits- und Betriebseinstellungen für Azure Firewall enthält. Mit ihr können Sie eine Regelhierarchie definieren und Compliance erzwingen. In dieser Aufgabe konfigurieren Sie Anwendungsregeln und Netzwerkregeln für die Firewall mithilfe der Firewallrichtlinie. Sie können Azure Firewallrichtlinien verwenden, um Regelsätze zu verwalten, die die Azure Firewall zum Filtern des Datenverkehrs verwendet.

### Architekturdiagramm

![Diagramm, das ein virtuelles Netzwerk mit einer Firewall und einer Routingtabelle zeigt](../Media/task-3.png)

### Qualifikationsaufgaben

- Erstellen Sie eine Azure Firewall.
- Erstellen und konfigurieren Sie eine Firewallrichtlinie.
- Erstellen Sie eine Anwendungsregelsammlung.
- Erstellen Sie eine Netzwerkregelsammlung.
  
## Übungsanweisungen

### Erstellen eines Azure Firewall-Subnetzes in unserem vorhandenen virtuellen Netzwerk

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff **Virtuelle Netzwerke** ein. Wählen Sie in den Suchergebnissen **Virtuelle Netzwerke** aus.

1. Wählen Sie **app-vnet** aus.

1. Wählen Sie **Subnetze** aus.

1. Wählen Sie **+ Subnetz** aus.

1. Geben Sie die folgenden Informationen ein, und wählen Sie **Speichern** aus.

    | Eigenschaft      | Wert                   |
    | :------------ | :---------------------- |
    | Name          | **AzureFirewallSubnet** |
    | Adressbereich | **10.1.63.0/24**        |

    > **Hinweis**: Belassen Sie alle anderen Einstellungen unverändert.

### Erstellen einer Azure Firewall-Instanz

1. Geben Sie in das Suchfeld am oberen Rand des Portals **Firewall** ein. Wählen Sie **Firewall** in den Suchergebnissen aus.

1. Wählen Sie **+ Erstellen** aus.

1. Erstellen Sie eine Firewall mithilfe der Werte in der folgenden Tabelle. Verwenden Sie für jede Eigenschaft, die nicht angegeben ist, den Standardwert.
    >**Hinweis**: Die Bereitstellung der Azure Firewall kann einige Minuten dauern.

    | Eigenschaft                 | Wert                                             |
    | :----------------------- | :------------------------------------------------ |
    | Ressourcengruppe           | **RG1**                                           |
    | Name                     | **app-vnet-firewall**                             |
    | Firewall-SKU             | **Standard**                                      |
    | Firewallverwaltung      | **Firewallrichtlinie zum Verwalten dieser Firewall verwenden** |
    | Firewallrichtlinie          | Wählen Sie **Neue hinzufügen** aus.                                |
    | Richtlinienname              | **fw-policy**                                     |
    | Region                   | **USA, Osten**                                       |
    | Richtlinientarif              | **Standard**                                      |
    | Virtuelles Netzwerk auswählen | **Vorhandenes verwenden**                                  |
    | Virtuelles Netzwerk          | **app-vnet** (RG1)                                |
    | Öffentliche IP-Adresse        | Neu hinzufügen: **fwpip**                                |

    [Erfahren Sie mehr über das Erstellen einer Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal).

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

### Aktualisieren der Firewallrichtlinie

1. Geben Sie in das Suchfeld am oberen Rand des Portals **Firewallrichtlinie** ein. Wählen Sie in den Suchergebnissen **Firewallrichtlinien** aus.

1. Wählen Sie **fw-policy** aus.

1. Wählen Sie **Anwendungsregeln** aus.

1. Wählen Sie **+ Regelsammlung hinzufügen** aus.

1. Verwenden Sie die Werte in der folgenden Tabelle. Verwenden Sie für jede Eigenschaft, die nicht angegeben ist, den Standardwert.

    | Eigenschaft               | Wert                                     |
    | :--------------------- | :---------------------------------------- |
    | Name                   | **app-vnet-fw-rule-collection**           |
    | Regelsammlungstyp   | **Anwendung**                           |
    | Priorität               | **200**                                   |
    | Regelsammlungsaktion | **Zulassen**                                 |
    | Regelsammlungsgruppe  | **DefaultApplicationRuleCollectionGroup** |

    1. Verwenden Sie unter **Regeln** die Werte aus der folgenden Tabelle.

        | Eigenschaft         | Wert                                  |
        | :--------------- | :------------------------------------- |
        | Name             | **AllowAzurePipelines**                |
        | Quellentyp      | **IP-Adresse**                         |
        | Quelle           | **10.1.0.0/23**                        |
        | Protokoll         | **https**                              |
        | Zieltyp | FQDN                                   |
        | Ziel      | **dev.azure.com, azure.microsoft.com** |

        und wählen Sie **Hinzufügen** aus.

> **Hinweis**: Die Regel **AllowAzurePipelines** ermöglicht der Webanwendung den Zugriff auf Azure Pipelines. Die Regel ermöglicht der Webanwendung den Zugriff auf den Azure DevOps-Dienst und die Azure-Website.

1. Erstellen Sie eine **Netzwerkregelsammlung**, die eine einzelne IP-Adressregel enthält, indem Sie die Werte in der folgenden Tabelle verwenden. Verwenden Sie für jede Eigenschaft, die nicht angegeben ist, den Standardwert.

1. Wählen Sie **Netzwerkregeln** aus.

1. Wählen Sie unter **+ Netzwerkregelsammlung** aus.

1. Verwenden Sie die Werte in der folgenden Tabelle. Verwenden Sie für jede Eigenschaft, die nicht angegeben ist, den Standardwert.

    | Eigenschaft               | Wert                                 |
    | :--------------------- | :------------------------------------ |
    | Name                   | **app-vnet-fw-nrc-dns**               |
    | Regelsammlungstyp   | **Netzwerk**                           |
    | Priorität               | **200**                               |
    | Regelsammlungsaktion | **Zulassen**                             |
    | Regelsammlungsgruppe  | **DefaultNetworkRuleCollectionGroup** |

    1. Verwenden Sie unter **Regeln** die Werte aus der folgenden Tabelle.

        | Eigenschaft              | Wert                |
        | :-------------------- | :------------------- |
        | Regel                  | **AllowDns**         |
        | Quelle                | **10.1.0.0/23**      |
        | Protokoll              | **UDP**              |
        | Zielports     | **53**               |
        | Zieladressen | **1.1.1.1, 1.0.0.1** |

        und wählen Sie **Hinzufügen** aus.

    Erfahren Sie mehr über das [Erstellen einer Anwendungsregel](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal#configure-an-application-rule) und [Erstellen einer Netzwerkregel](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule).

1. Um zu überprüfen, ob der Bereitstellungsstatus der Azure Firewall und der Firewallrichtlinie **erfolgreich** war, führen Sie Folgendes aus.

1. Geben Sie in das Suchfeld am oberen Rand des Portals **Firewall** ein. Wählen Sie **Firewall** in den Suchergebnissen aus.

1. Wählen Sie **app-vnet-firewall** aus.

1- Überprüfen Sie, ob der **Bereitstellungsstatus** **Erfolgreich** lautet.

1- Geben Sie in das Suchfeld am oberen Rand des Portals **Firewall** ein. Wählen Sie in den Suchergebnissen **Firewallrichtlinien** aus.

1. Wählen Sie **fw-policy** aus.

1- Überprüfen Sie, ob der **Bereitstellungsstatus** **Erfolgreich** lautet.
