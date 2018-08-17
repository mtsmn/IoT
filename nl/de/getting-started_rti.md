---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Lernprogramm 'Einführung'
{: #getting-started-with-iotp}

In diesem Lernprogramm mit einer Einführung in {{site.data.keyword.iot_full}} wird ein IoT-Gerät mit {{site.data.keyword.iot_short_notm}} verbunden und eine Analyse zur Durchsuchung von Echtzeitdaten eingerichtet.
{:shortdesc}

<div id="prerequisites"></div>

## Vorbereitende Schritte
{: #prereqs}

Sie benötigen ein [IBM Cloud-Konto ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/registration/){: new_window},
eine Instanz des {{site.data.keyword.iot_short_notm}}-Service und ein Gerät, das die nachfolgend aufgeführten Anforderungen erfüllt:

*	Ihr Gerät muss in der Lage sein, unter Verwendung der HTTP- oder MQTT-Protokolle zu kommunizieren.

* Die Gerätenachrichten müssen den Anforderungen an die {{site.data.keyword.iot_short_notm}}-Nachrichtennutzdaten entsprechen.

Weitere Informationen finden Sie in [Entwickeln von Geräten in Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Schritt 1: Gerät registrieren

Sie können Geräte einzeln über das [{{site.data.keyword.iot_short_notm}}-Dashboard ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://internetofthings.ibmcloud.com){: new_window} hinzufügen oder die [Watson IoT Platform-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} verwenden, um mehrere Geräte hinzuzufügen.

Gehen Sie wie folgt vor, um ein Gerät über das {{site.data.keyword.iot_short_notm}}-Dashboard hinzuzufügen:

1. Klicken Sie in der {{site.data.keyword.Bluemix_notm}}-Konsole auf der Detailseite für den {{site.data.keyword.iot_short_notm}}-Service auf **Starten**.

    Die {{site.data.keyword.iot_short_notm}}-Webkonsole wird in einer neuen Browserregisterkarte unter der folgenden URL geöffnet:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Dabei ist *Organisations-ID* die ID [Ihrer {{site.data.keyword.iot_short_notm}}-Organisation](iotplatform_overview.html#organizations){: new_window}.

2. Wählen Sie im Dashboard 'Überblick' im Menüteilfenster die Option **Geräte** aus und klicken Sie anschließend auf **Gerät hinzufügen**.

3. Erstellen Sie einen Gerätetyp für das Gerät, das Sie hinzufügen.

    Jedem Gerät, das mit {{site.data.keyword.iot_short_notm}} verbunden ist, muss ein Gerätetyp zugeordnet sein. Gerätetypen sind Gruppen von Geräten, denen allgemeine Merkmale gemeinsam sind.

    1. Klicken Sie auf **Gerätetyp erstellen**.
    2. Geben Sie einen Gerätenamen ein, z. B. `my_device_type`, sowie eine Beschreibung für den Gerätetyp.
    
        > **Hinweis:** Der Name des Gerätetyps darf maximal 36 Zeichen umfassen und nur alphanumerische Zeichen (a-z, A-Z, 0-9) sowie folgende Zeichen enthalten: `_`, `.` und `-`.

    3. Optional: Geben Sie Attribute und Metadaten zu dem Gerätetyp ein.

        Sie können Attribute und Metadaten später hinzufügen und bearbeiten.
        {: tip}

4. Klicken Sie auf **Weiter**, um mit dem Hinzufügen Ihres Geräts mit dem ausgewählten Gerätetyp zu beginnen.

5. Geben Sie eine Geräte-ID ein, z. B. `my_first_device`.

    Die Geräte-ID wird zum Ermitteln des Geräts im {{site.data.keyword.iot_short_notm}}-Dashboard verwendet und ist auch ein erforderlicher Parameter für das Herstellen einer Verbindung zwischen Ihrem Gerät und {{site.data.keyword.iot_short_notm}}.

    > **Hinweis:** Der Name des Gerätetyps darf maximal 36 Zeichen umfassen und nur alphanumerische Zeichen (a-z, A-Z, 0-9) sowie folgende Zeichen enthalten: `_`, `.` und `-`.

    Bei netzverbundenen Geräten kann die Geräte-ID beispielsweise die MAC-Adresse des Geräts ohne trennende Doppelpunkte sein.

5. Klicken Sie auf **Weiter**, um den Prozess abzuschließen.

6. Geben Sie ein Authentifizierungstoken an oder akzeptieren Sie ein automatisch generiertes Token. Wenn Sie ein eigenes Token erstellen möchten, achten Sie darauf, dass es zwischen 8 und 36 Zeichen lang ist und nur aus alphanumerischen Zeichen und den folgenden Zeichen besteht: `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)` und `-`.

    Das Token darf keine Folgen aus wiederholten Zeichen, Wörterbuchwörter, Benutzernamen oder andere vordefinierte Folgen enthalten.

7. Überprüfen Sie die Übersichtsinformationen auf ihre Richtigkeit und klicken Sie anschließend auf **Hinzufügen**, um die Verbindung hinzuzufügen.

8. Kopieren Sie auf der Seite mit den Geräteinformationen die folgenden Einzelangaben und speichern Sie sie:

    * Organisations-ID
    * Gerätetyp
    * Geräte-ID
    * Authentifizierungsmethode
    * Authentifizierungstoken

    Sie benötigen die Organisations-ID, den Gerätetyp, die Geräte-ID und das Authentifizierungstoken, um Ihr Gerät für die Verbindung mit {{site.data.keyword.iot_short_notm}} zu konfigurieren.

## Schritt 2: Gerät mit {{site.data.keyword.iot_short_notm}} verbinden

1. Richten Sie Ihr Gerät für MQTT-Messaging ein und verwenden Sie zur Authentifizierung die Organisations-ID, den Gerätetyp, die Geräte-ID und das Authentifizierungstoken.

2. Senden Sie mithilfe des MQTT-Protokolls Gerätenachrichten an Ihre {{site.data.keyword.iot_short_notm}}-Organisation.

    Folgende Informationen sind zum Verbinden Ihrer Geräte erforderlich:

    * URL: *Organisations-ID*.messaging.internetofthings.ibmcloud.com

      Dabei ist *Organisations-ID* die ID Ihrer {{site.data.keyword.iot_short_notm}}-Organisation.

    * Port:
      * 1883
      * 8883 (verschlüsselt)
      * 443 (Websockets)
    * Geräte-ID: d:_org_id:device_type:device_id_
    * Benutzername: use-token-auth
    * Kennwort: _Authentication token_
    * Format des Ereignisthemas: iot-2/evt/_event_id/fmt/format_string_

      Hierbei gibt _event_id_ den Ereignisnamen an, der in {{site.data.keyword.iot_short_notm}} angezeigt wird, und _format_string_ das Format des Ereignisses, z. B. JSON.

    * Nachrichtenformat: JSON

  Weitere Informationen finden Sie in [MQTT-Konnektivität für Geräte](/docs/services/IoT/devices/mqtt.html).

## Schritt 3: Boards und Karten erstellen, um Gerätedaten zu verfolgen

Mithilfe von Boards und Karten können Sie Grafiken anzeigen, die Dataset-Werte von einem oder mehreren Geräten darstellen, um eine schnelle Übersicht und einen Einblick in die Gerätedaten zu erhalten.

1. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard auf **Neues Board erstellen**.

2. Geben Sie einen Namen und eine Beschreibung für das Board ein.

3. Klicken Sie auf **Weiter** und anschließend auf **Erstellen**.

4. Klicken Sie auf das soeben erstellte Board und anschließend auf **Neue Karte hinzufügen**. Wählen Sie "Geräte" als Kartentyp aus. Die folgende Tabelle beschreibt die Visualisierungsoptionen, aus denen Sie auswählen können.

| Typ | Angezeigte Daten |
|---------------|---------------|
| Generische Visualisierung | Der Wert mindestens eines Datasets. Zum Anzeigen von bis zu drei Datenpunkten in einer kleinen Tabelle wählen Sie die große Widgetgröße aus. |
| Kurvendiagramm (Line Chart) | Mindestens ein Dataset in einem Echtzeitdiagramm, in dem geblättert werden kann. Verwenden Sie das Menü 'Einstellungen', um den Datenbereich und die Aufbewahrungsdauer, die Darstellung und die Funktionsweise sowie weitere Einstellungen für die Diagramme festzulegen. |
| Balkendiagramm | Datasetwerte in beschrifteten Balken. Mit dem Menü 'Einstellungen' können Sie zwischen der horizontalen oder vertikalen Richtung hin- und herschalten. |
| Ringdiagramm | Mindestens ein Dataset in einer kreisförmigen Darstellung. |
| Wert | Der unaufbereitete Wert mindestens eines Datasets. |
| Messanzeige | Als Messanzeige angezeigter Wert eines Datasets. Mit dem Menü 'Einstellungen' können Sie für die Messanzeige optional Schwellenwerte für den unteren, mittleren und oberen Datenbereich festlegen. |
| Geräteeigenschaften | Bestimmte Eigenschaften für mindestens ein Gerät. |
| Alle Geräteeigenschaften | Alle Eigenschaften für mindestens ein Gerät. |
| Geräteliste | Eine Liste für die Überwachung mehrerer Geräte. Eine Liste kann als Datenquelle für andere Karten verwendet werden. |
| Geräteinformationen | Basisinformationen für ein einzelnes Gerät. |
| Gerätezuordnung | Position von Geräten in einer Geräteliste. |

{: caption="Tabelle 1. Visualisierungsoptionen" caption-side="top"}

## Schritt 4: Schemas für Gerätetypen erstellen

Nun müssen Sie ein Gerätetypschema erstellen und Geräteeigenschaften zuordnen, um anschließend Regeln zu erstellen, die anhand der Datenpunkte aus Ihren zugeordneten Geräteeigenschaften ausgelöst werden.

1. Wechseln Sie im {{site.data.keyword.iot_short_notm}}-Dashboard zu **Geräte > Schemas verwalten** und klicken Sie auf **Schema hinzufügen**.

2. Wählen Sie einen Gerätetyp aus, der diesem Nachrichtenschema zugeordnet werden soll. Für einen Gerätetyp kann nur ein einziges Schema festgelegt werden.

3. Klicken Sie auf **Eigenschaft hinzufügen** und fügen Sie eine oder mehrere Eigenschaften hinzu.

    Sie können Eigenschaften aus einem verbundenen Gerät auswählen, virtuelle Eigenschaften erstellen, die vorhandene Eigenschaften ändern oder kombinieren, oder Sie können Eigenschaften manuell hinzufügen.

    Die verfügbaren Eigenschaften sind in den Nutzdaten der Nachrichten definiert, die von einem Gerät gesendet werden.
    {: tip}

    * Um eine Eigenschaft manuell hinzuzufügen, wählen Sie die Registerkarte **Manuell** aus und definieren Sie die folgenden Eigenschaftsdetails:
        * Name
        * Datentyp
        * Eigenschaft
        * Dateineinheit (optional)
        * Dezimalstellen (optional)

    * Um eine virtuelle Eigenschaft zu erstellen, wählen Sie die Registerkarte **Virtuelle Eigenschaft** aus und definieren Sie die folgenden Eigenschaftsdetails:
        * Name
        * Datentyp
        * Eigenschaft
        * Berechnung
        * Dateineinheit (optional)
        * Dezimalstellen

    * Um Eigenschaften von einem verbundenen Gerät auszuwählen, wählen Sie die Registerkarte **Von verbundenen Geräten** aus und wählen Sie eine mehrere Eigenschaften aus, die dem Schema hinzugefügt werden sollen. Die erstellten Eigenschaften werden hinzugefügt und als Beschreibung wird der Name der Eigenschaft ausgewählt.

4. Klicken Sie auf **Fertigstellen**, um die Eigenschaften zu erstellen.

## Schritt 5: Regeln und Aktionen erstellen

Regeln sind bedingungsbasierte Entscheidungspunkte, die gerätebezogene Echtzeitdaten und vordefinierte Schwellenwerte oder andere Eigenschaftsdaten abgleichen, um ein Alert auszulösen, wenn eine Bedingung erfüllt wird. Zusätzlich zu einem im {{site.data.keyword.iot_short_notm}}-Dashboard angezeigten Alert können Sie mindestens eine Aktion hinzufügen, mit der bei Auslösen einer Regel Geschäftslogik ausgeführt wird.

1. Wechseln Sie im {{site.data.keyword.iot_short_notm}}-Dashboard zu **Regeln** und klicken Sie auf **Cloud-Regel erstellen**.

2. Geben Sie einen Namen und eine Beschreibung für die Regel ein, wählen Sie einen Gerätetyp aus, auf den die Regel zutrifft, und klicken Sie auf **Weiter**.

3. Fügen Sie zum Einrichten der Regellogik mindestens eine IF-Bedingung hinzu, die als Auslöser für die Regel verwendet wird.

    Sie können Bedingungen in parallelen Reihen hinzufügen, um sie als OR-Bedingungen anzuwenden, oder Sie können Bedingungen in sequenziellen Spalten hinzufügen, um sie als AND-Bedingungen anzuwenden. Siehe dazu die folgenden Beispiele:

      * Eine einfache Regel kann einen Alert auslösen, wenn ein Parameterwert größer ist als ein angegebener Wert: Bedingung = `temp_cpu>80`
      * Eine komplexere Regel kann zu einem Auslösen führen, wenn eine Übereinstimmung mit einer Kombination aus Schwellenwerten auftritt: Bedingung = `temp_cpu>60 AND cpu_load>90`

    Um eine Bedingung auszulösen, die zwei Eigenschaften vergleicht, oder um mindestens zwei Eigenschaftsbedingungen auszulösen, die sequenziell durch AND verbunden sind, schließen Sie die auslösenden Datenpunkte in dieselbe Gerätenachricht ein. Wenn die Daten in mehr als einer Nachricht empfangen werden, werden die Bedingung oder die sequenziellen Bedingungen nicht ausgelöst.
    {: tip}

4. Konfigurieren Sie für Ihre Regel Anforderungen für bedingte Auslöser.

    Sie können Anforderungen für bedingte Trigger verwenden, um die Anzahl der Alerts, die für eine Regel über einen Zeitraum ausgelöst werden, zu steuern. Das bedingte Auslösen gilt für jede Bedingung in der Regel. Wenn beispielsweise für eine Regel mithilfe von OR fünf parallele Bedingungen festgelegt wurden, wird für den Zähler des bedingten Auslösers jede Bedingung, die als 'true' erkannt wird, gezählt.

      1. Klicken Sie im Regeleditor auf den Standardlink **Jedes Mal auslösen, wenn Bedingungen erfüllt sind**, um das Dialogfeld 'Anforderung für Häufigkeit festlegen' zu öffnen.
      2. Wählen Sie den bedingten Auslöser, den Sie in Ihrer Regel verwenden wollen, aus und konfigurieren Sie ihn.

        * Jedes Mal auslösen, wenn Bedingungen erfüllt sind
        * Auslösen, wenn Bedingungen N-mal in M _Zeiteinheit_ erfüllt sind

5. Erstellen Sie mindestens eine Aktion, die auftritt, wenn die Regelbedingungen erfüllt sind, oder wählen Sie eine Aktion aus.

    Zum Beispiel kann eine Aktion darin bestehen, eine E-Mail zu senden oder einen Webhook zu posten.

6. Optional: Wählen Sie für die Regel eine Alertpriorität aus.

    Die Priorität wird zum Klassifizieren der Alerts verwendet, die in **Boards > Regelbasierte Analyse** angezeigt werden. Die Standardpriorität ist 'Niedrig'.

7. Klicken Sie auf **Speichern**, um ohne Aktivierung zu speichern, oder auf **Aktivieren**, um Ihre Regel zu speichern und zu aktivieren.

    Wenn Sie die Regel aktivieren, wird zum Board **Regelbasierte Analyse** ein Alert hinzugefügt, wenn die Bedingungen erfüllt sind und eine beliebige Regelaktion ausgeführt wird.

## Nächste Schritte

Erweitern Sie die Datenanalysefunktionen, indem Sie Ihre eigenen Apps erstellen und verbinden, mit denen Echtzeit- und archivierte Gerätedaten verarbeitet werden können.

  * Rufen Sie die [Clientbibliotheken](/docs/services/IoT/iot_platform_client_lib.html) auf, um auf Tools zuzugreifen, die dem Erstellen von Code für die Integration und Verbindung Ihrer Geräte und Apps dienen.

  * Durchsuchen Sie die [Anleitungen für Watson IoT Platform![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} nach weiteren Lernprogrammen zur Integration erweiterter Analysefunktionen in Ihre verbundenen Geräte und Apps.
