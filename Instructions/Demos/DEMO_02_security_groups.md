---
demo:
  title: 'Demonstration: Erstellen und Konfigurieren von Netzwerksicherheitsgruppen'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstration – Erstellen und Konfigurieren von Netzwerksicherheitsgruppen


In dieser Demonstration werden wir uns mit Sicherheitsgruppen befassen. 

**Hinweis:** Eine **[interaktive Labsimulation für virtuelle Netzwerke](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013?azure-portal=true)** ist verfügbar, mit der Sie ein ähnliches Lab durchgehen können, wenn Sie keine Live-Demonstration durchführen können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und der vorgeschlagenen Demo vor, aber die dargestellten Kernkonzepte und Ideen sind identisch. 

[Einschränken des Zugriffs auf PaaS-Ressourcen – Tutorial – Azure-Portal](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

### Erstellen einer Netzwerksicherheitsgruppe

1. Öffnen Sie das Azure-Portal.

1. Navigieren Sie zum Eintrag  **Netzwerksicherheitsgruppen**, und wählen Sie ihn aus.

1. [Unterstützende Folie] Erstellen Sie eine NSG, und erläutern Sie dabei die Einstellungen. 
 
1. Warten Sie, bis die neue NSG bereitgestellt wurde.

**Erkunden von Eingangs- und Ausgangsregeln**

1. Wählen Sie Ihre neue NSG aus.

1. [Unterstützende Folie] Besprechen Sie, wie die NSG Subnetzen oder Netzwerkschnittstellen zugeordnet werden kann.

1. Erörtern Sie die zweckmäßigen Eingangs- und Ausgangsregeln.  

1. Überprüfen Sie die Standardeingangs- und -ausgangsregeln. 

1. Erstellen Sie eine neue Regel, und erläutern Sie die Einstellungen. Erläutern Sie insbesondere die Dienstauswahl (z. B. HTTPS) und die Prioritätseinstellungen. 
 

### Erstellen der ASG
 
1. [Unterstützende Folie] Suchen Sie den Eintrag  **Anwendungssicherheitsgruppen**, und wählen Sie ihn aus.

1. Erstellen Sie eine ASG, und erläutern Sie dabei die Einstellungen. 
 
1. Warten Sie, bis die neue ASG bereitgestellt wurde.

1. Besprechen Sie, wie NSG-Regeln zur ASG zugeordnet werden können.


### Zuordnen der NSGs 
1.  Navigieren Sie zur NSG, die Sie erstellt haben.
1.  Wählen Sie „Subnetze“ im Abschnitt „Einstellungen“ aus.
1.  Wählen Sie auf der Seite „Subnetze“ die Option „+ Zuordnen“ aus.
1.  Wählen Sie unter „Subnetz zuordnen“ Ihr virtuelles Netzwerk aus.


>**Hinweis**: Die Kursteilnehmer sollten jetzt in der Lage sein, LAB_02 abzuschließen.

