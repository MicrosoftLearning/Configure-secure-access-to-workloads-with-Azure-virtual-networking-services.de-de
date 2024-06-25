---
lab:
  title: 'Übung: Internes Aufzeichnen und Auflösen von Domänennamen'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Lab: Internes Aufzeichnen und Auflösen von Domänennamen.

## Szenario

Ihre Organisation benötigt, dass Workloads Domänennamen intern in virtuellen Netzwerken aufzeichnen und auflösen. Virtuelle Computer in virtuellen Netzwerken können den Domänennamen anstelle von IP-Adressen für die interne Kommunikation verwenden. In diesem Fall werden die Domänennamen mit einer privaten DNS-Zone über eine virtuelle Netzwerkverknüpfung aufgelöst.

### Architekturdiagramm

![Diagramm der Verbindung von Azure DNS mit einem virtuellen Netzwerk](../Media/task-5.png)

### Qualifikationsaufgaben

- Erstellen und Konfigurieren einer privaten DNS-Zone
- Erstellen und Konfigurieren von DNS-Einträgen.
- Konfigurieren von DNS-Einstellungen in einem virtuellen Netzwerk.

## Übungsanweisungen

### Erstellen einer privaten DNS-Zone

Privates Azure-DNS bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass Sie eine benutzerdefinierte DNS-Lösung hinzufügen müssen. Durch die Verwendung privater DNS-Zonen können Sie anstelle der momentan verfügbaren von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden.

1. Geben Sie in der Suchleiste des Portals **Private DNS-Zonen** in das Suchfeld ein, und wählen Sie „Private DNS-Zonen“ aus.

1. Wählen Sie **+ Erstellen** aus.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** von „Private DNS-Zone erstellen“ die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft       | Wert                        |
    | :------------- | :--------------------------- |
    | Abonnement   | **Wählen Sie Ihr Abonnement aus** |
    | Ressourcengruppe | **RG1**                      |
    | Name           | **contoso.com**              |
    | Region         | **USA, Osten**                  |

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

### Erstellen einer virtuellen Netzwerkverknüpfung zu Ihrer privaten DNS-Zone

1. Geben Sie in der Suchleiste des Portals **Private DNS-Zonen** in das Suchfeld ein, und wählen Sie „Private DNS-Zonen“ aus.

1. Wählen Sie **contoso.com** aus.

1. Wählen Sie **+ Virtuelle Netzwerkverknüpfung** aus.

1. Wählen Sie **+ Hinzufügen** aus.

1. Verwenden Sie auf der Registerkarte **Grundeinstellungen** die Informationen aus der folgenden Tabelle, um das virtuelle Netzwerk zu erstellen.

    | Eigenschaft                 | Wert             |
    | :----------------------- | :---------------- |
    | Linkname                | **app-vnet-link** |
    | Virtuelles Netzwerk          | **app-vnet**      |
    | Automatische Registrierung aktivieren | **Aktiviert**       |

1. Wählen Sie **OK** aus.

### Erstellen eines DNS-Ressourceneintragssatzes

1. Geben Sie in der Suchleiste des Portals **Private DNS-Zonen** in das Suchfeld ein, und wählen Sie „Private DNS-Zonen“ aus.

1. Wählen Sie **contoso.com** aus.

1. Wählen Sie **+ Datensatzgruppe** aus.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** von „Datensatzgruppe erstellen“ die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft   | Wert        |
    | :--------- | :----------- |
    | Name       | **backend**  |
    | type       | **A**        |
    | TTL        | **1**        |
    | IP-Adresse | **10.1.1.4** |

1. Wählen Sie **OK** aus.

1. Überprüfen Sie, ob **contoso.com** über einen Ressourceneintragssatz namens **backend** verfügt.
