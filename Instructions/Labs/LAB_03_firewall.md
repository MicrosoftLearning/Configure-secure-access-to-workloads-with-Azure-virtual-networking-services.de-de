---
lab:
  title: 'Übung 03: Erstellen und Konfigurieren der Azure Firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Übung 03: Erstellen und Konfigurieren der Azure Firewall

## Szenario

Ihr Unternehmen erfordert eine zentralisierte Netzwerksicherheit für das virtuelle Anwendungsnetzwerk. Mit zunehmender Nutzung der Anwendungen werden eine detailliertere Filterung auf Anwendungsebene und ein erweiterter Schutz vor Bedrohungen erforderlich sein. Außerdem ist zu erwarten, dass die Anwendung kontinuierliche Updates von Azure DevOps-Pipelines benötigt. Sie ermitteln diese Anforderungen.
+ Azure Firewall ist für zusätzliche Sicherheit im App-vnet erforderlich. 
+ Eine **Firewall-Richtlinie** sollte konfiguriert werden, um den Zugriff auf die Anwendung zu verwalten. 
+ Eine Firewall-Richtlinie **Anwendungsregel** ist erforderlich. Diese Regel ermöglicht der Anwendung den Zugriff auf Azure DevOps, damit der Anwendungscode aktualisiert werden kann. 
+ Eine Firewall-Richtlinie **Netzwerkregel** ist erforderlich. Diese Regel ermöglicht die DNS-Auflösung. 

### Qualifikationsaufgaben

+ Erstellen Sie eine Azure Firewall.
+ Erstellen und konfigurieren Sie eine Firewallrichtlinie.
+ Erstellen Sie eine Anwendungsregelsammlung.
+ Erstellen Sie eine Netzwerkregelsammlung.

## Architekturdiagramm

![Diagramm, das ein virtuelles Netzwerk mit einer Firewall und einer Routingtabelle zeigt](../Media/task-3.png)


  
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

**Hinweis**: Belassen Sie alle anderen Einstellungen unverändert.

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

1. Suchen Sie im Portal die Option `Firewall Policies`, und wählen Sie sie aus. 

1. Wählen Sie **fw-policy** aus.

### Hinzufügen einer Anwendungsregel

1. Wählen Sie im Blatt **Einstellungen** die Option **Anwendungsregeln** und dann **Regelsammlung hinzufügen** aus.

1. Konfigurieren Sie die Sammlung von Anwendungsregeln und wählen Sie dann **Hinzufügen**. 

    | Eigenschaft               | Wert                                     |
    | :--------------------- | :---------------------------------------- |
    | Name                   | `app-vnet-fw-rule-collection`         |
    | Regelsammlungstyp   | **Anwendung**                           |
    | Priorität               | `200`                                   |
    | Regelsammlungsaktion | **Zulassen**                                 |
    | Regelsammlungsgruppe  | **DefaultApplicationRuleCollectionGroup** |
    | Name             | `AllowAzurePipelines`                |
    | Quellentyp      | **IP-Adresse**                         |
    | Quelle           | `10.1.0.0/23`                       |
    | Protocol         | `https`                             |
    | Zieltyp | **FQDN**                                  |
    | Destination      | `dev.azure.com, azure.microsoft.com` |

**Hinweis**: Die Regel **AllowAzurePipelines** ermöglicht der Webanwendung den Zugriff auf Azure Pipelines. Die Regel ermöglicht der Webanwendung den Zugriff auf den Azure DevOps-Dienst und die Azure-Website.

### Hinzufügen einer Netzwerkregel

1. Wählen Sie im Blatt **Einstellungen** die Option **Netzwerkregeln** und dann **Eine Netzwerksammlung hinzufügen**.

1. Konfigurieren Sie die Netzwerkregel und wählen Sie dann **Hinzufügen**.  

    | Eigenschaft               | Wert                                 |
    | :--------------------- | :------------------------------------ |
    | Name                   | `app-vnet-fw-nrc-dns`               |
    | Regelsammlungstyp   | **Netzwerk**                           |
    | Priorität               | `200`                        |
    | Regelsammlungsaktion | **Zulassen**                             |
    | Regelsammlungsgruppe  | **DefaultNetworkRuleCollectionGroup** |
    | Regel                  | **AllowDns**         |
    | Quelle                | `10.1.0.0/23`      |
    | Protokoll              | **UDP**              |
    | Zielports     | `53`               |
    | Zieladressen | **1.1.1.1, 1.0.0.1** |

### Überprüfen des Status der Firewall- und Firewallrichtlinie

1. Suchen Sie im Portal nach **Firewall** und wählen Sie es aus. 

1. Sehen Sie sich die **App-vnet-firewall** an und stellen Sie sicher, dass der **Bereitstellungsstatus** **Erfolgreich** ist. Dies kann einige Minuten dauern. 

1. Suchen Sie im Portal nach und wählen Sie **Firewallrichtlinien**.

1. Sehen Sie sich die **fw-policy** an und stellen Sie sicher, dass der **Bereitstellungsstatus** **Erfolgreich** ist. Dies kann einige Minuten dauern.

### Erfahren Sie mehr in Onlineschulungen

+ [Einführung in Azure Firewall](https://learn.microsoft.com/training/modules/introduction-azure-firewall/). In diesem Modul erfahren Sie, wie Azure Firewall-Funktionen, -Regeln, -Bereitstellungsoptionen und -Verwaltung funktionieren.
+ [Einführung in Azure Firewall Manager](https://learn.microsoft.com/training/modules/intro-to-azure-firewall-manager/). In diesem Modul erfahren Sie, wie Azure Firewall Manager eine zentrale Verwaltung von Sicherheitsrichtlinien und Routen für cloudbasierte Sicherheitsperimeter bereitstellt.

### Wichtige Erkenntnisse

Herzlichen Glückwunsch zum Abschluss der Übung. Hier sind die wichtigsten Erkenntnisse:

+ Azure Firewall ist ein cloudbasierter Sicherheitsdienst, der Ihre virtuellen Azure-Netzwerkressourcen vor eingehenden und ausgehenden Bedrohungen schützt.
+ Eine Azure- Firewallrichtlinie ist eine Ressource, die eine oder mehrere Sammlungen von NAT-, Netzwerk- und Anwendungsregeln enthält.
+ Netzwerkregeln erlauben oder verweigern Datenverkehr auf der Grundlage von IP-Adressen, Ports und Protokollen.
+ Anwendungsregeln erlauben oder verweigern Datenverkehr auf der Grundlage von voll qualifizierten Domänennamen (FQDNs), URLs und HTTP/HTTPS-Protokollen.
