---
demo:
  title: 'Demonstration: Erstellen und Konfigurieren des Peerings virtueller Netzwerke'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstration – Erstellen und Konfigurieren des Peerings virtueller Netzwerke


In dieser Demo erstellen Sie virtuelle Netzwerke.

**Hinweis:** Sie können die vorgeschlagenen Werte für die Einstellungen oder eigene benutzerdefinierte Werte verwenden.

**Hinweis:** Eine **[interaktive Labsimulation für virtuelle Netzwerke](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204?azure-portal=true)** und **[virtuelles Netzwerk-Peering](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209?azure-portal=true)** ist verfügbar, mit der Sie ein ähnliches Lab durchgehen können, wenn Sie keine Live-Demonstration durchführen können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und der vorgeschlagenen Demo vor, aber die dargestellten Kernkonzepte und Ideen sind identisch. 


[Schnellstart: Erstellen eines virtuellen Netzwerks – Azure-Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

### Erstellen eines virtuellen Netzwerks im Portal


   
1.  [Unterstützende Folie] Bevor Sie mit der Demonstration beginnen, wiederholen wir, was virtuelle Netzwerke sind und welche wichtigen Konzepte für virtuelle Azure-Netzwerke gelten. Verwenden Sie diese Folie, um die Funktionen von virtuellen Azure-Netzwerken hervorzuheben. Sowie Konzepte und bewährte Methoden für Azure Virtual Network. Wie Sie das Erstellen eines virtuellen Netzwerks veranschaulichen, können Sie die grundlegenden Konzepte von Adressraum, Subnetzen, Regionen und Abonnements erläutern. Sie könnten diese Folien auch am Ende besprechen und direkt mit der Demonstration beginnen.
   
2.  Melden Sie sich beim Azure-Portal an, und suchen Sie nach  **Virtuelle Netzwerke**.
   
3.  Erstellen Sie ein virtuelles Netzwerk, und erläutern Sie die grundlegenden Einstellungen. Stellen Sie sicher, dass mindestens ein Subnetz erstellt wird. 
   
4.  Erläutern Sie, dass Azure-Portal eine benutzerfreundliche Benutzeroberfläche bietet. Erforderliche Elemente sind durch ein rotes Sternchen (*) gekennzeichnet.
   
5.  [Unterstützende Folie] Wählen Sie die Registerkarte „Sicherheit“ aus. Verwenden Sie diese Folie, um die Sicherheitsdienste kurz hervorzuheben. Diese Themen werden später im Kurs ausführlicher behandelt. Erfahren Sie mehr über Dienste, die in einem virtuellen Netzwerk bereitgestellt werden können. 
   
6.  [Unterstützende Folie] Wählen Sie die Registerkarte „IP-Adressen“ aus. Verwenden Sie diese Folie, um Folgendes zu wiederholen: Planen virtueller Netzwerke und Subnetze. Fügen Sie ein Subnetz hinzu, um den Kursteilnehmern zu zeigen, wie Subnetze konfiguriert werden. 
7.  Wählen Sie „Überprüfen“ aus, um Sie sicherzustellen, dass keine Überprüfungsfehler vorhanden sind.
8.  Wählen Sie „Erstellen“ aus, und warten Sie, bis die Ressource bereitgestellt wird. Weisen Sie auf die Benachrichtigungen hin. 
9.  Zeigen Sie, wie zur Ressource gewechselt wird.
10. Wiederholen Sie den Vorgang zum Erstellen eines anderen virtuellen Netzwerks, damit Sie VNet-Peering demonstrieren können.

## Konfigurieren des VNet-Peerings

**Hinweis:** Für diese Demo benötigen Sie zwei virtuelle Netzwerke.

[Herstellen von Verbindungen zwischen virtuellen Netzwerken mittels VNet-Peering – Tutorial](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Konfigurieren des VNet-Peerings für das erste virtuelle Netzwerk**

1. Wählen Sie im  **Azure-Portal** das erste virtuelle Netzwerk aus. Überprüfen Sie den Wert des Peerings. 

1. Wählen Sie unter  **Einstellungen** die Option  **Peerings** und **+ Neues Peering hinzufügen** aus.

1. Konfigurieren Sie das Peering des zweiten virtuellen Netzwerks. Verwenden Sie die Informationssymbole, um die verschiedenen Einstellungen zu überprüfen. 

1. Wenn das Peering abgeschlossen ist, überprüfen Sie den **Peeringstatus**. 

**Konfigurieren des VNet-Peerings für das zweite virtuelle Netzwerk**

1. Wählen Sie im  **Azure-Portal** das zweite virtuelle Netzwerk aus.

1. Wählen Sie unter  **Einstellungen** die Option  **Peerings** aus.

1. Beachten Sie, dass automatisch ein Peering erstellt wurde. Beachten Sie, dass der  **Peeringstatus**  **Verbunden** lautet.


>**Hinweis**: Die Kursteilnehmer sollten jetzt in der Lage sein, LAB_01 abzuschließen.

