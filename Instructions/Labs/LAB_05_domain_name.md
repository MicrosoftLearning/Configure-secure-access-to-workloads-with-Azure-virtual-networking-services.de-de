---
lab:
  title: 'Übung 05: Erstellen von DNS-Zonen und Konfigurieren der DNS-Einstellungen'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Übung 05: Erstellen von DNS-Zonen und Konfigurieren der DNS-Einstellungen

## Szenario

Ihr Unternehmen erfordert, dass Workloads Domänennamen anstelle von IP-Adressen für die interne Kommunikation verwenden.  Das Unternehmen möchte keine eigene DNS-Lösung hinzufügen. Sie ermitteln diese Anforderungen.
+ Eine **private DNS-Zone** ist für contoso.com erforderlich.
+ Der DNS wird einen **virtuellen Netzwerk Link** zu app-vnet verknüpfen. 
+ Für das Backend-Subnetz ist ein neuer **DNS-Eintrag** erforderlich. 

## Qualifikationsaufgaben

+ Erstellen und Konfigurieren einer privaten DNS-Zone
+ Erstellen und Konfigurieren von DNS-Einträgen.
+ Konfigurieren von DNS-Einstellungen in einem virtuellen Netzwerk.
  
## Architekturdiagramm

![Diagramm der Verbindung von Azure DNS mit einem virtuellen Netzwerk](../Media/task-5.png)



## Übungsanweisungen

**Hinweis:** Für diese Übung ist es erforderlich, dass die virtuellen Netzwerke und Subnetze von Lab 01 installiert sind. Eine [Vorlage](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Allfiles/Labs/All-Labs/create-vnet-subnets-template.json) wird zur Verfügung gestellt, wenn Sie diese Ressourcen bereitstellen müssen.

### Erstellen einer privaten DNS-Zone

[Azure Private DNS](https://learn.microsoft.com/azure/dns/private-dns-overview) bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass eine benutzerdefinierte DNS-Auflösung hinzugefügt werden muss. Mithilfe von privaten DNS-Zonen können Sie Ihre eigenen benutzerdefinierten Domänennamen anstelle der von Azure bereitgestellten Namen verwenden.

1. Suchen Sie im Azure-Portal nach `Private dns zones` und wählen Sie es aus.

1. Wählen Sie **+ Erstellen** und konfigurieren Sie die DNS-Zone. 

    | Eigenschaft       | Wert                        |
    | :------------- | :--------------------------- |
    | Abonnement   | **Wählen Sie Ihr Abonnement aus** |
    | Ressourcengruppe | **RG1**                      |
    | Name           | `private.contoso.com`              |
    | Region         | **USA, Osten**                  |

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

1. Warten Sie, bis die DNS-Zone bereitgestellt ist, und wählen Sie dann **Zur Ressource gehen**. 

### Erstellen einer virtuellen Netzwerkverknüpfung zu Ihrer privaten DNS-Zone

Um DNS-Einträge in einer privaten DNS-Zone aufzulösen, müssen die Ressourcen mit der privaten Zone verknüpft sein. Eine [virtuelle Netzwerkverbindung](https://learn.microsoft.com/azure/dns/private-dns-virtual-network-links) ordnet das virtuelle Netzwerk der privaten Zone zu.

1. Arbeiten Sie im Portal weiter an der DNS-Zone **private.contoso.com**. 

1. Wählen Sie im Blatt **DNS-Verwaltung** die Option **+ Virtuelle Netzwerk-Links**.

1. Wählen Sie **+ Hinzufügen"** aus und konfigurieren Sie den virtuellen Netzwerk-Link. 

    | Eigenschaft                 | Wert             |
    | :----------------------- | :---------------- |
    | Linkname                | `app-vnet-link` |
    | Virtuelles Netzwerk          | **app-vnet**      |
    | Automatische Registrierung aktivieren | **Aktiviert**       |

1. Wählen Sie **Erstellen** und warten Sie, bis die Bereitstellung abgeschlossen ist. Wenn nötig, **Aktualisieren** Sie die Seite. 

### Erstellen eines DNS-Ressourceneintragssatzes

[DNS-Datensätze](https://learn.microsoft.com/en-us/azure/dns/dns-zones-records#dns-records) stellen Informationen über die DNS-Zone bereit. 

1. Arbeiten Sie im Portal weiter an der DNS-Zone **private.contoso.com**. 

1. Wählen Sie unter **DNS-Verwaltung** die Option **Eintragsgruppen** aus.

1. Beachten Sie, dass für jeden virtuellen Computer automatisch zwei A-Einträge erstellt worden sind. 

1. Wählen Sie **+ Hinzufügen** und konfigurieren Sie eine Eintragsgruppe. Wählen Sie abschließend **Hinzufügen** aus. 
   
    | Eigenschaft   | Wert        |
    | :--------- | :----------- |
    | Name       | `backend`    |
    | type       | **A**        |
    | TTL        | **1**        |
    | IP-Adresse | **10.1.1.5** |

**Hinweis:** Diese Datensatzgruppe impliziert, dass ein virtueller Computer in app-vnet mit der privaten IP-Adresse 10.1.1.5 vorhanden ist.

### Erfahren Sie mehr in Onlineschulungen

+ [Einführung in Azure DNS](https://learn.microsoft.com/training/modules/intro-to-azure-dns/) In diesem Modul wird erläutert, was Azure DNS ist, wie es funktioniert und wann Sie Azure DNS als Lösung verwenden sollten, um die Anforderungen Ihrer Organisation zu erfüllen.
+ [Hosten Ihrer Domäne in Azure DNS.](https://learn.microsoft.com/training/modules/host-domain-azure-dns/) In diesem Modul erfahren Sie, wie Sie eine DNS-Zone und einen DNS-Datensatz erstellen.

### Wichtige Erkenntnisse

Herzlichen Glückwunsch zum Abschluss der Übung. Hier sind die wichtigsten Erkenntnisse:

+ Azure DNS ist ein Clouddienst, mit dem Sie DNS-Domänen (Domain Name System) hosten und verwalten können, die auch als DNS-Zonen bezeichnet werden. 
+ Öffentliche Azure DNS-Zonen hosten Daten zu Domänennamenszonen für Einträge, die von jedem Host im Internet aufgelöst werden sollen.
+ Azure Private DNS-Zonen ermöglichen Ihnen die Konfiguration eines privaten DNS-Zonen-Namensraums für private Azure-Ressourcen.
+ Eine DNS-Zone ist eine Sammlung von DNS-Einträgen. DNS-Einträge liefern Informationen über die Domäne.
