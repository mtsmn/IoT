---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} Edge konfigurieren (Vorschau)
{: #edge_configure}

**Wichtig:** Die {{site.data.keyword.iot_full}} Edge-Komponente steht nur im Rahmen eines eingeschränkten Vorschauprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Bevor Sie mit dem Empfang von Daten von Geräten beginnen können, die mit Ihrem Edge-Knoten verbunden sind, müssen Sie das Gateway mit {{site.data.keyword.iot_short_notm}} verbinden. Zum Herstellen einer Verbindung zwischen einem Gateway und {{site.data.keyword.iot_short_notm}} gehört die Erstellung eines Gateway-Gerätetyps und die Registrierung des Gateways in {{site.data.keyword.iot_short_notm}}. Sie können die Registrierungsinformationen anschließend verwenden, um das Gateway-Gerät mit {{site.data.keyword.iot_short_notm}} zu verbinden.


## Allgemeiner Konfigurationsablauf

1. [Erstellen Sie eine Organisation](#edge_org) in der Testumgebung für die Vorschau von {{site.data.keyword.iot_short_notm}} Edge.
2. [Aktivieren Sie die Vorschau von {{site.data.keyword.iot_short_notm}}](#edge_enable).
3. Konfigurieren Sie Ihre Organisation für die Verwendung von Edge-Gateway-Knoten mit {{site.data.keyword.iot_short_notm}}, indem Sie [einen Gateway-Typ erstellen, für den Edge aktiviert ist](#edge_gw_type), [Gateway-Geräte hinzufügen](#edge_gw) und die Edge-Services auswählen, die auf den Edge-Knoten ausgeführt werden sollen. Bei Bedarf können Sie [angepasste Services erstellen](WIoTP_edge_dev.html#create_service).
4. [Konfigurieren Sie die Edge-Services](#config_service).
5. [Installieren Sie Edge auf Ihren Geräten](#edge_install_device).
6. [Verwalten und überwachen Sie die Edge-Services](#monitor_service).

## Organisation erstellen
{: #edge_org}

Gehen Sie wie folgt vor, um eine Organisation in der Testumgebung für die Vorschau von {{site.data.keyword.iot_short_notm}} Edge zu erstellen:
1. Melden Sie sich für ein {{site.data.keyword.Bluemix_notm}}-Konto an und erstellen Sie eine Instanz des {{site.data.keyword.iot_short_notm}}-Service in Ihrer {{site.data.keyword.Bluemix_notm}}-Organisation. Sie können eine {{site.data.keyword.iot_short_notm}}-Instanz direkt über die [{{site.data.keyword.iot_short_notm}}-Seite im IBM Cloud-Servicekatalog ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window} erstellen.  
2. Geben Sie die Informationen zur Bereitstellung der IBM Cloud-Organisation an. **Wichtig:** Sie müssen Ihre IBM Cloud-Organisation am Standort **Vereinigte Staaten (Süden)** bereitstellen, um die {{site.data.keyword.iot_short_notm}} Edge-Vorschau zu verwenden.
3. Klicken Sie auf **Erstellen**.
4. Klicken Sie in der {{site.data.keyword.iot_short_notm}}-Instanz, die geöffnet wird, auf **Starten**.
Eine neue {{site.data.keyword.iot_short_notm}}-Organisation wird erstellt. Die Organisations-ID wird am Anfang der URL und im Dashboard unter Ihrem Benutzernamen angezeigt. Sie können jetzt {{site.data.keyword.iot_short_notm}} Edge für die Organisation aktivieren.

## Vorschau von {{site.data.keyword.iot_short_notm}} Edge aktivieren
{: #edge_enable}

Die Vorschau von {{site.data.keyword.iot_short_notm}} Edge ist standardmäßig inaktiviert. So aktivieren Sie {{site.data.keyword.iot_short_notm}} Edge:

1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard für Ihre Organisation die Option **Einstellungen** aus.
2. Aktivieren Sie im Abschnitt **Experimentelle Features** die experimentellen Features, einschließlich der Vorschau von {{site.data.keyword.iot_short_notm}} Edge.
3. Aktualisieren Sie den Browser, um das neue Feature für **Edge-Services** und den Direktaufruf im Navigationsmenü des Dashboards anzuzeigen. **Hinweis:** Wenn das Symbol für **Edge-Services** nicht im Navigationsmenü des Dashboards angezeigt wird, stellen Sie sicher, dass Sie **Vereinigte Staaten (Süden)** als Ihren Standort ausgewählt haben.

Das Flag wird dem lokalen Browserspeicher hinzugefügt.

## Gateway-Typ mit aktivierter Edge-Funktionalität erstellen
{: #edge_gw_type}

Jedem Gateway, das mit {{site.data.keyword.iot_short_notm}} verbunden ist, muss ein Gateway-Typ zugeordnet sein. Gateway-Typen sind Gruppen von Geräten, denen allgemeine Merkmale gemeinsam sind.

1. Wählen Sie im Hauptnavigationsmenü des {{site.data.keyword.iot_short_notm}}-Dashboards die Option **Geräte** aus.
2. Klicken Sie auf der Registerkarte **Gerätetypen** auf **Gerätetyp hinzufügen**.
3. Wählen Sie auf der Registerkarte **Identität** der Seite **Typ hinzufügen** den Typ **Gateway** aus und geben Sie einen Namen für den Gateway-Typ, z. B. `mein_gateway-typ`, und eine Beschreibung für den Gateway-Typ ein.
**Wichtig:** Der Name des Gateways-Typs darf maximal 36 Zeichen umfassen und nur folgende Zeichen enthalten:
<ul>
 <li>Alphanumerische Zeichen (a-z, A-Z, 0-9)</li>
 <li>Bindestriche (-)</li>
 <li>Unterstreichungszeichen (&lowbar;)</li>
 <li>Punkte (.)</li>
 </ul>
3. Optional: Geben Sie Attribute und Metadaten für den Gateway-Typ ein.
4. Aktivieren Sie die Schaltfläche für die **Edge-Funktionen**.
5. Wählen Sie den Architekturtyp für den Gateway-Typ aus. Der Architekturtyp muss mit der Architektur Ihres Geräts übereinstimmen. Beispiel: Ein Odroid-Gerät wird unter der ARM64-Architektur ausgeführt.
6. Klicken Sie auf **Weiter**.
7. Optional: Geben Sie auf der Registerkarte **Geräteinformationen** zusätzliche Details und Metadaten zum Gerätetyp des Gateways ein.
8. Wählen Sie die Edge-Services aus, die zu Ihren Edge-Geräten hinzugefügt werden sollen. Der Core IoT-Service wird zu allen Geräten hinzugefügt; Sie können jedoch weitere Services aus dem Katalog hinzufügen, indem Sie die folgenden Schritte ausführen:
 1. Klicken Sie auf **Services hinzufügen**.
 2. Wählen Sie im Fenster **Edge-Services hinzufügen** die entsprechende Version der Services aus, die hinzugefügt werden sollen, und klicken Sie anschließend auf **Fertig**.
9. Klicken Sie im Fenster **Gateway-Typ hinzufügen** auf **Fertig**.
Sie können mit dem [Hinzufügen von Gateway-Geräten zu diesem Gateway-Typ](#edge_gw) fortfahren.

## Gateway-Geräte hinzufügen
{: #edge_gw}

1. Wählen Sie im Hauptnavigationsmenü des {{site.data.keyword.iot_short_notm}}-Dashboards die Option **Geräte** aus.
2. Klicken Sie auf **Gerät hinzufügen**.
3. Wählen Sie auf der Registerkarte **Identität** den Gerätetyp des Gateways aus, dem Sie Geräte hinzufügen, und geben Sie eine eindeutige ID für das Gateway-Gerät ein, das Sie hinzufügen.
Die Geräte-ID wird zum Ermitteln des Gateway-Geräts im {{site.data.keyword.iot_short_notm}}-Dashboard verwendet und ist auch ein erforderlicher Parameter für das Herstellen einer Verbindung zwischen Ihrem Gateway-Gerät und {{site.data.keyword.iot_short_notm}}.
**Wichtig:** Die Geräte-ID darf maximal 36 Zeichen umfassen und nur folgende Zeichen enthalten:
 <ul>
 <li>Alphanumerische Zeichen (a-z, A-Z, 0-9)</li>
 <li>Bindestriche (-)</li>
 <li>Unterstreichungszeichen (&lowbar;)</li>
 <li>Punkte (.)</li>
 </ul>
 **Tipp:** Bei netzverbundenen Geräten kann die Geräte-ID beispielsweise die MAC-Adresse des Geräts ohne trennende Doppelpunkte sein.
4. Klicken Sie auf **Weiter**.
5. Optional: Geben Sie auf der Registerkarte **Geräteinformationen** zusätzliche Details und Metadaten zum Gateway-Gerät ein.
6. Klicken Sie auf **Weiter**.
7. Führen Sie auf der Registerkarte **Sicherheit** der Seite **Gerät hinzufügen** eine automatische Generierung eines Authentifizierungstokens für dieses Gerät durch.Wenn Sie auswählen, ein eigenes Token zu erstellen, müssen Sie sicherstellen, dass es eine Länge von 8-36 Zeichen hat, aus einer Mischung von Groß- und Kleinbuchstaben, Zahlen, und Bindestrichen, Unterstreichungszeichen oder Punkten besteht. Das Token darf keine Folgen aus wiederholten Zeichen, Wörterbuchwörter, Benutzernamen oder andere vordefinierte Folgen enthalten.
9. Klicken Sie auf **Weiter**.
10. Klicken Sie auf **Fertig**.
11. Kopieren Sie auf der Seite mit den Geräteinformationen folgende Geräteinformationen und speichern Sie sie:
 - Organisations-ID, wie beispielsweise `tubo8x`
 - Gerätetyp, wie beispielsweise `my_gateway_type`
 - Geräte-ID. **Tipp:** Bei netzverbundenen Geräten kann dies beispielsweise die MAC-Adresse ohne trennende Doppelpunkte sein.
 - Authentifizierungsmethode, wie beispielsweise `token`
 - Authentifizierungstoken, z. B. `PtBVriRqIg4uh)_-Kl`
  Verwenden Sie diese Informationen, um das Gateway mit {{site.data.keyword.iot_short_notm}} zu verbinden und mit dem Empfang von Daten von Geräten zu beginnen, die mit dem Gateway verbunden sind.

## Edge-Services konfigurieren
{: #config_service}

Nachdem Sie einen neuen Gerätetyp mit aktivierter Edge-Funktionalität erstellt haben, können Sie verschiedene Services auswählen, die in Ihrem Edge-Knoten installiert werden sollen.

1. Wählen Sie im Hauptnavigationsmenü des {{site.data.keyword.iot_short_notm}}-Dashboards die Option **Geräte** aus.
2. Klicken Sie auf **Gerätetypen**.
3. Wählen Sie den Edge-Gerätetyp aus, den Sie konfigurieren möchten.
4. Wählen Sie auf der Registerkarte **Edge-Services** das Stiftsymbol aus, um den Datensatz zu bearbeiten.
6. Klicken Sie auf **Edge-Services hinzufügen**, um eine Liste der Services anzuzeigen, die für diese Organisation hochgeladen wurden.
7. Wählen Sie im Fenster **Edge-Services hinzufügen** die entsprechende Version der Services aus, die hinzugefügt werden sollen, und klicken Sie anschließend auf **Fertig**.
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## Edge-Services verwalten und überwachen
{: #monitor_service}

Nach der Installation von Edge-Knoten können Sie mit {{site.data.keyword.iot_short_notm}} den Status von Services überwachen, die auf Edge-Knoten ausgeführt werden.

1.	Wählen Sie im Hauptnavigationsmenü des {{site.data.keyword.iot_short_notm}}-Dashboards die Option **Geräte** aus.
2.	Wählen Sie das Gerät aus, das Ihrem Edge-Knoten entspricht.
3.	Klicken Sie auf **Edge-Services**, um den Status der Services anzuzeigen, die auf dem Edge-Knoten installiert sind.

## Weitere Informationen
{: #more_info}

Informationen zum Herstellen einer Verbindung zu {{site.data.keyword.iot_short_notm}} finden Sie in [MQTT-Konnektivität für Gateways](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt).

Weitere ausführliche Informationen zu {{site.data.keyword.iot_short_notm}} finden Sie in [Einführung in Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp).

Weitere Informationen zu {{site.data.keyword.iot_short_notm}} Edge finden Sie in den folgenden Abschnitten:
- [{{site.data.keyword.iot_short_notm}} Edge - Übersicht](WIoTP_edge.html#edge_overview)
- [{{site.data.keyword.iot_short_notm}} Edge auf Edge-Geräten installieren](WIoTP_edge_install.html#edge_install_device)
- [Für {{site.data.keyword.iot_short_notm}} Edge entwickeln](WIoTP_edge_dev.html#edge_dev)
