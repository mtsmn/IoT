---

copyright:
  years: 2016, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node-RED-Gerätesimulator erstellen und verbinden
Verwenden Sie Node-RED, um einen Gerätesimulator zu erstellen und Daten zum simulierten Gerät an Ihre {{site.data.keyword.iot_full}}-Organisation zu senden.  
{:shortdesc}

## Übersicht
Node-RED ist ein Tool, mit dem Hardware-Geräte, APIs und Online-Services auf eine neue und interessante Weise miteinander verbunden werden können. Weitere Informationen finden Sie auf der Website von [Node-RED ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://nodered.org/){: new_window}.  

Sie können Ihre Node-RED-Instanz in Ihrer eigenen Umgebung ausführen oder Sie können sie als {{site.data.keyword.Bluemix_notm}}-Anwendung verwenden. Die folgenden Schritte enthalten die Anweisungen für {{site.data.keyword.Bluemix_notm}}.

Erstellen und verbinden Sie den Node-RED-Gerätesimulator, um MQTT-Gerätenachrichten an {{site.data.keyword.iot_short_notm}} zu senden. Im folgenden Beispiel simuliert der Gerätesimulator das Senden von Daten für einen Frachtcontainer an einen MQTT-Broker, z. B. {{site.data.keyword.iot_short_notm}}.

## Schritte

1. Erstellen Sie den Node-RED-Gerätesimulator, indem Sie die folgenden Schritte ausführen:   
    1. Melden Sie sich unter 'https://console.ng.bluemix.net' bei {{site.data.keyword.Bluemix_notm}} an.
    2. Wählen Sie die Registerkarte **Katalog** aus.
    3. Suchen Sie im Servicekatalog den Abschnitt mit den Boilerplates und klicken Sie auf die Option für **Watson IoT Platform Starter**. **Tipp:** Klicken Sie [hier ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window}, um direkt zur Seite für Watson IoT Platform Starter zu gelangen.
    4. Wählen Sie auf der Seite für Watson IoT Platform Starter den Bereich aus, in dem Sie Node-RED bereitstellen möchten, überprüfen Sie die Auswahl für 'App erstellen' und klicken Sie dann auf **Erstellen**, um Node-RED zu Ihrer Bluemix-Organisation hinzuzufügen. Beispiel:
    <ul>
     <li> Bereich: dev
     <li> App-Name: myDevice
     <li> Hostname: myDevice  
    </ul>  
Übernehmen Sie bei den anderen Optionen die Standardwerte. Nach Bereitstellung der Anwendung wird die Seite angezeigt, auf der Sie mit der Node-RED-Codierung beginnen können.
**Hinweis:** Der Staging-Prozess benötigt einige Minuten.  

2. Klicken Sie auf den Link 'Routen', um Node-RED zu öffnen, oder besuchen Sie die URL der App. Beispiel: `http://myDevice.mybluemix.net`  
3. Führen Sie die folgenden Schritte aus, um den **Node-RED-Editor zu sichern**:
    1. Klicken Sie auf **Weiter**.
    2. Fügen Sie einen Benutzernamen und ein Kennwort hinzu.  
    **Hinweis:** Beim Kennwort müssen die Regeln zur erforderlichen Komplexität beachtet werden. Andernfalls bleibt die Schaltfläche **Weiter** abgeblendet.  
    3. Optional. Aktivieren Sie die Option, mit der allen Benutzern die Anzeige des Editors erlaubt, jedoch die Durchführung von Änderungen untersagt wird. Alternativ hierzu können Sie auch die Option auswählen, mit der allen Benutzern der Zugriff auf den Editor und die Durchführung von Änderungen erlaubt wird. Die Auswahl dieser Option wird jedoch nicht empfohlen.
    4. Klicken Sie auf **Fertigstellen**, um die Konfiguration abzuschließen.
4. Klicken Sie auf **Zu Node-RED-Ablaufeditor wechseln**.
5. Geben Sie den Benutzernamen und das Kennwort ein, die Sie zuvor erstellt haben.  
Der Gerätesimulatorablauf wird nun für den Ablaufeditor verfügbar gemacht. Der Ablauf simuliert ein **Thermostat**, das seine Position sowie die gemessenen Daten zur Temperatur und zur Feuchtigkeit an {{site.data.keyword.iot_short_notm}} sendet.  
6. Vergewissern Sie sich, dass das Gerät verbunden ist, indem Sie überprüfen, ob für den Knoten **An IBM IoT Platform senden** eine Verbindungsnachricht für den Punkt angezeigt wird. Die Überprüfung gilt auch für den Knoten **IBM IoT App In** für den Unterablauf **Temperaturüberwachung**.  
7. Senden und empfangen Sie MQTT-Gerätenachrichten, indem Sie die folgenden Schritte ausführen:  
    1. Klicken Sie auf den Einfügeknoten **Daten senden**, um das Senden von Daten an {{site.data.keyword.iot_short_notm}} auszulösen.
       **Hinweis:** Sie können den Debugknoten **Nutzdaten der Debugausgabe** aktivieren, um anzuzeigen, welche Daten gesendet werden, und um den Funktionsknoten **Gerätenutzdaten** zu überprüfen und den Code anzuzeigen, mit dem die Nutzdaten erstellt werden. 
    2. Nachdem Sie auf **Daten senden** geklickt haben, werden die MQTT-Nachrichten an {{site.data.keyword.iot_short_notm}} gesendet und vom Knoten **IBM IoT App In** empfangen. Der Unterablauf **Temperaturüberwachung** legt fest, ob die Temperatur sich innerhalb der definierten Grenzwerte befindet, und zeigt eine Nachricht auf der Debugregisterkarte an. 
       **Hinweis:** Klicken Sie auf den Switchknoten **temp thresh**, um die Grenzwerte zu überprüfen und zu ändern.
    3. Überprüfen Sie die Debugregisterkarte. Das System zeigt eine Nachricht an, z. B. **Temperatur (17) innerhalb der Grenzwerte**.
    
Ihr IoT-Beispielgerät ist nun mit {{site.data.keyword.iot_short_notm}} verbunden und Sie können Gerätedaten anzeigen.
