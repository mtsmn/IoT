---

copyright:
  years: 2017
lastupdated: "2017-06-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Einführende Anleitungen für {{site.data.keyword.iot_short_notm}}
{: #getting-started}

Die einführenden Anleitungen sind so konzipiert, dass Sie sie durch die Grundlagen der Entwicklung eines produktionsbereiten End-to-End-Prototypsystems mit {{site.data.keyword.iot_short_notm}} führen. Sie wurden für Entwickler geschrieben, die {{site.data.keyword.iot_short_notm}} erstmalig benutzen.
{:shortdesc}

Jede Anleitung stellt einen Schritt-für-Schritt-Prozess bereit, der Sie schnell zu einer bereitgestellten und ausführbaren Lösung führt, in der einzelne oder auch mehrere {{site.data.keyword.iot_short_notm}}-Funktionen gezeigt werden.

## Anleitungen verwenden  
{: #using-guides}
In jeder Anleitung erhalten Sie mindestens ein GitHub-Beispiel, das auf Basis der bewährten Verfahren für {{site.data.keyword.iot_short_notm}} codiert wurde. Diese Beispiele können skaliert, gesichert und mit Systemen integriert werden. Sie können sie im Allgemeinen direkt in Ihre devOps-Entwicklungs- und -Erstellungsprozesse einfügen.

Technisch interessierte Benutzer können auf die schnelle Lösung aufbauen und den Beispielcode an Ihre Umgebung anpassen oder durch eine Ergänzung der schnellen Lösung mit Hardwarebeispielen zur realitätsnäheren Simulation darstellen, welche Situationen in Ihrer Produktionsumgebung eintreten können. Die Anleitungen stellen zusätzliche Links zur taskrelevanten Dokumentation, zu API-Dokumenten, Clientbibliotheken (SDKs), Schulungsvideos und weiterem Schulungsmaterial bereit.

Sie können die Anleitungen in der angegebenen Reihenfolge durcharbeiten, wobei die erste Anleitung eine Basis bildet, auf der die nachfolgenden Anleitungen aufbauen. Wenn Sie die Anleitungen in einer anderen Reihenfolge aufrufen und einem eigenen Pfad durch die Anleitungsreihe folgen wollen, dann finden Sie in allen Anleitungen eine Liste der jeweiligen Voraussetzungen. Wenn Sie z. B. bereits eine {{site.data.keyword.iot_short_notm}}-Organisation konfiguriert haben, können Sie direkt mit den Anleitungen 2 und 3 beginnen und sich mit den Regeln, Aktionen und einer Beispielapp für die Überwachung vertraut machen. Die Voraussetzungen enthalten Informationen zum Format der Gerätedaten, die für diese Anleitungen erforderlich sind, sodass Sie die eigene Umgebung entsprechend einrichten können.
{: tip}

## Liste der Anleitungen
{: #list-of-guides}  

Die folgenden Anleitungen stehen zur Verfügung:

| Anleitung | Beschreibung |    
| ----- | ---- |   
| [1. Laufbandgerät verbinden](getting-started-iot-conveyor.html) | Beginnen Sie mit der Installation einer {{site.data.keyword.iot_short_notm}}-Organisation und stellen Sie dann eine Verbindung für eine GitHub-Simulationsanwendung für ein Laufband her. Wenn Sie einen praxisnäheren Ansatz vorziehen, dann können Sie auch einen eigenen Raspberry-Pi-basierten Laufbandsimulator installieren. </br> Konfigurieren Sie anschließend einige grundlegende {{site.data.keyword.iot_short_notm}}-Dashboardkarten, um die Gerätedaten zu visualisieren und zu überwachen. |   
| [2. Grundlegende Echtzeitregeln und -aktionen verwenden](getting-started-iot-rules.html) | Nachdem Sie mindestens ein Gerät mit der {{site.data.keyword.iot_short_notm}}-Organisation verbunden haben und Daten senden, können Sie Geschäftsregeln und Auslöseraktionen konfigurieren und z. B. den Anlagenoperator benachrichtigen, wenn die Geschwindigkeit seines Laufbands unter einen bestimmten Grenzwert fällt.  
| [3. Gerätedaten überwachen](getting-started-iot-monitoring.html) | Erweitern Sie die integrierten {{site.data.keyword.iot_short_notm}}-Dashboards zur Geräteüberwachung, indem Sie eine Node.js- oder widgetbibliotheksbasierte Überwachungsapp verbinden, um Laufbanddaten in Echtzeit anzuzeigen.  
| [4. Große Anzahl von Geräten simulieren](getting-started-iot-large-scale-simulation.html) | Sie können auf der einfachen Gerätesimulation aufbauen, indem Sie eine große Anzahl von Gerätesimulatoren zu Ihrer Umgebung hinzufügen, um die Analyse und die Überwachung der vorherigen Anleitungen in einer realistischeren Umgebung mit mehreren Geräten zu testen.</br>**Wichtig:** Für die Anwendung sind 512 MB Speicherplatz erforderlich, was das standardmäßig zugeordnete Speicherplatzvolumen und auch das Speicherplatzvolumen überschreitet, das für kostenlose Testkonten bereitgestellt wird. Für diese Testkonten muss zuerst ein Upgrade auf ein Subskriptionskonto oder ein nutzungsabhängiges Konto durchgeführt werden. |   
