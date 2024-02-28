---
demo:
  title: 'Demonstration: Erstellen und Konfigurieren von Azure DNS'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstration – Erstellen und Konfigurieren von Azure DNS

In dieser Demonstration erkunden Sie Azure DNS.

[Tutorial: Hosten Ihrer Domäne und Unterdomäne – Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Erstellen einer privaten DNS-Zone**

1. Öffnen Sie das Azure-Portal.

1. Suchen Sie nach dem Dienst  **DNS-Zonen** .

1. Erstellen Sie eine **DNS-Zone**, und erläutern Sie den Zweck der Zone. Als Namen können Sie contoso.internal.com verwenden.

1.  Warten Sie, bis die DNS-Zone erstellt wurde. Möglicherweise müssen Sie die Seite  **aktualisieren** .

**Hinzufügen eines DNS-Ressourceneintragssatzes**


[Tutorial: Erstellen eines Aliaseintrags, um auf einen Zonenressourceneintrag zu verweisen](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Nachdem Ihre Zone erstellt wurde, wählen Sie  **+ Datensatzgruppe** aus.

1. Verwenden Sie die Dropdownliste  **Typ** , um die verschiedenen Arten von Einträgen anzuzeigen. Überprüfen Sie, wie die verschiedenen Datensatztypen verwendet werden. Beachten Sie, wie sich die Datensatzinformationen ändern, wenn Sie verschiedene Datensatztypen auswählen.

1. Erstellen Sie als Beispiel einen **A**-Datensatz. 

**Verknüpfen von VNet für die automatische Registrierung**

1.  Nachdem die DNS-Zone bereitgestellt wurde, überprüfen Sie die Übersichtsseite mit den Kursteilnehmern.
1.  Verknüpfen Sie die private DNS-Zone mit einem virtuellen Netzwerk, indem Sie eine Verknüpfung mit einem virtuellen Netzwerk erstellen.
1.  Wählen Sie im linken Bereich die Option „Verknüpfungen virtueller Netzwerke“ aus.
1.  Wählen Sie „Hinzufügen“ aus.
1.  Geben Sie „myLink“ für den Linknamen ein.
1.  Wählen Sie unter „Virtuelles Netzwerk“ die Option „myAzureVNet“ aus.
1.  Aktivieren Sie das Kontrollkästchen „Automatische Registrierung aktivieren“.
1.  Wählen Sie „OK“ aus.

>**Hinweis**: Die Kursteilnehmer sollten jetzt in der Lage sein, LAB_05 abzuschließen.