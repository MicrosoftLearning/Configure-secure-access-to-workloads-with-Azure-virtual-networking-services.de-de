---
demo:
  title: 'Demonstration: Erstellen und Konfigurieren einer Azure Firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstration – Erstellen und Konfigurieren einer Azure Firewall

**Hinweis:** Die Bereitstellung der Azure Firewall kann einige Minuten dauern.

In dieser Demonstration erkunden Sie Azure Firewall.
Überprüfen und erstellen Sie eine Azure Firewall und eine Firewallrichtlinie.
1.  [Unterstützende Folie] Bevor wir mit der Demonstration beginnen, wiederholen wir, was Azure Firewall ist.
2.  Öffnen Sie das Azure-Portal.
3.  Erstellen Sie eine Azure Firewall.
4.  ⓘ Erläutern Sie die verfügbaren Konfigurationsoptionen auf der Registerkarte „Grundeinstellungen“, während Sie sie ausfüllen. 
5.  Übernehmen Sie die anderen Standardwerte, und wählen Sie „Überprüfen + erstellen“ aus.
6.  Wechseln Sie nach Abschluss der Bereitstellung zur Firewallressource, und überprüfen Sie die Übersichtsseite. 


### Konfigurieren einer Anwendungsregel 

1. [Unterstützende Folie] Richtlinienregeln für Azure Firewall

Hierbei handelt es sich um die Anwendungsregel, die ausgehenden Zugriff auf [www.google.com]\(www.google.com) zulässt.
1.  Navigieren Sie zu der von Ihnen erstellten Firewallrichtlinie.
2.  Wählen Sie „Anwendungsregeln“ aus.
3.  Wählen Sie „Regelsammlung hinzufügen“ aus.
4.  Geben Sie unter „Name“ den Wert „App-Coll01“ ein.
5.  Geben Sie unter „Priorität“ den Wert 200 ein.
6.  Wählen Sie unter „Regelsammlungsaktion“ die Option „Zulassen“ aus.
7.  Geben Sie unter „Regeln“ für „Name“ den Wert „Allow-Google“ ein.
8.  Wählen Sie unter „Quelltyp“ die Option „IP-Adresse“ aus.
9.  Geben Sie unter „Quelle“ den Wert 10.0.2.0/24 ein.
10. Geben Sie unter „Protokoll:Port“ den Eintrag „http, https“ ein.
11. Wählen Sie unter „Zieltyp“ die Option „FQDN“ aus.
12. Geben Sie für „Ziel“ den Wert „www.google.com“ ein.
13. Wählen Sie „Hinzufügen“ aus.

Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. Weitere Informationen finden Sie unter Infrastruktur-FQDNs.

### Konfigurieren einer Netzwerkregel
Hierbei handelt es sich um die Netzwerkregel, die ausgehenden Zugriff auf zwei IP-Adressen am Port 53 (DNS) zulässt.
1.  Wählen Sie „Netzwerkregeln“ aus.
2.  Wählen Sie „Regelsammlung hinzufügen“ aus.
3.  Geben Sie unter „Name“ den Wert „Net-Coll01“ ein.
4.  Geben Sie unter „Priorität“ den Wert 200 ein.
5.  Wählen Sie unter „Regelsammlungsaktion“ die Option „Zulassen“ aus.
6.  Wählen Sie für „Regelsammlungsgruppe“ die Option „DefaultNetworkRuleCollectionGroup“ aus.
7.  Geben Sie unter „Regeln“ für „Name“ den Wert „Allow-DNS“ ein.
8.  Wählen Sie unter „Quelltyp“ die Option „IP-Adresse“ aus.
9.  Geben Sie unter „Quelle“ den Wert 10.0.2.0/24 ein.
10. Wählen Sie für „Protokoll“ die Option „UDP“ aus.
11. Geben Sie unter „Zielports“ den Wert 53 ein.
12. Wählen Sie unter „Zieltyp“ die Option „IP-Adresse“ aus.
13. Geben Sie unter „Ziel“ den Wert 209.244.0.3, 209.244.0.4 ein.
Dies sind öffentliche DNS-Server, die von CenturyLink betrieben werden.
14. Wählen Sie „Hinzufügen“ aus.

