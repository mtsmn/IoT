---

copyright:
years: 2016, 2018
lastupdated: "2018-05-14"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung zum Datenmanagement mithilfe der Webschnittstelle
{: #gs_web}

{{site.data.keyword.iot_full}} stellt in der Funktion für das Datenmanagement Online-Tools bereit, die zur Vereinfachung der Normalisierung und Umwandlung von Daten dienen.

## Übersicht
{: #web_overview}

Zusätzlich zur [Watson IoT Platform Data Management API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}, die bereitgestellt wird, können Sie die Funktion zur Erstellung einer einfachen Schnittstelle oder die Funktion zur Erstellung erweiterter Schnittstellen verwenden, um Ressourcen zu konfigurieren und mit der Zuordnung Ihrer Gerätedaten zu beginnen. Die Onlinefunktionen zur Erstellung von Schnittstellen führen Sie durch die Schritte in der Benutzerschnittstelle.

Eine physische Schnittstelle wird verwendet, um die Schnittstelle zwischen einem physischen Gerät und Watson IoT Platform zu modellieren. Ereignistypen können der physische Schnittstelle zugeordnet werden. Eine logische Schnittstelle wird verwendet, um die normalisierte Ansicht für den Gerätestatus in Watson IoT Platform zu definieren, und wird für gewöhnlich von Anwendungen genutzt. Eine logische Schnittstelle muss einem Schema der logischen Schnittstelle zugeordnet werden. Der Status wird als Antwort auf eingehende Geräteereignisse aktualisiert.

Weitere Informationen zur Funktion für das Datenmanagement und ihrer Vorzüge finden Sie in der [Einführung zum Datenmanagement](../GA_information_management/ga_im_device_twin.html#device_twins).

Informationen zu den Ressourcen, die Sie konfigurieren müssen, finden Sie unter dem Thema [Erklärung des Datenmanagements](../GA_information_management/ga_im_definitions.html#definitions_resource).

Mit der Funktion zur Erstellung einer einfachen Schnittstelle können Sie eine physische Schnittstelle hinzufügen. Dadurch wird automatisch eine logische Schnittstelle erstellt und die beiden Schnittstellen werden einander zugeordnet. Mit der Funktion zur Erstellung erweiterter Schnittstellen verfügen Sie über einen höheren Grad an Kontrolle über die Konfiguration. Sie fügen eine physische Schnittstelle und mindestens eine logische Schnittstelle hinzu und erstellen anschließend Zuordnungen zwischen diesen Komponenten.

Die Schnittstellen und Zuordnungen werden im Entwurfsformat hinzugefügt. Nachdem Sie sie hinzugefügt und überprüft haben, müssen Sie sie aktivieren. Weitere Informationen zu Entwurfsressourcen und aktiven Ressourcen finden Sie in [Ressourcen erstellen, aktualisieren, aktivieren und inaktivieren](../GA_information_management/ga_im_definitions.html#draft_active_resources).

Die Schnittstellenbibliotheken enthalten physische und logische Vorlagenschnittstellen, die Sie wiederverwenden können.

## Allgemeiner Workflow
{: #interface_flow}

Die folgenden Schritte sollen Sie beim Konfigurieren der Ressourcen unterstützen, die zur Verwendung der Funktion für das Datenmanagement erforderlich sind.

### Vorbereitende Schritte

Bei dieser Prozedur wird vorausgesetzt, dass Sie über einen Gerätetyp mit mindestens einem registrierten und verbundenen Gerät verfügen. Weitere Informationen zu diesen Voraussetzungen und zu den Anweisungen zum Erstellen eines Gerätetyps und zum Registrieren eines Geräts finden Sie unter [Geräte verbinden](../iotplatform_task.html#iotplatform_task).

### Schritte

1. Wählen Sie entweder die Funktion zur Erstellung einer einfachen Schnittstelle oder die Funktion zur Erstellung erweiterter Schnittstellen aus:
  - **Funktion zur Erstellung einer einfachen Schnittstelle - Ablauf**
    1. [Erstellen Sie einen Entwurf einer physischen Schnittstelle](#create_physical_interface_simple).
  - **Funktion zur Erstellung einer erweiterten Schnittstelle - Ablauf**
    1. [Erstellen Sie einen Entwurf einer physischen Schnittstelle](#create_physical_interface_advanced).
    2. [Erstellen Sie mindestens einen Entwurf einer logischen Schnittstelle](#create_logical_interface). (Dieser Schritt wird automatisch ausgeführt, wenn Sie den einfachen Ablauf verwenden.)
    3. [Erstellen Sie eine Zuordnung zwischen der physischen Schnittstelle und den logischen Schnittstellen](#create_interface_mappings).
2. [Legen Sie die Benachrichtigungsvorgaben fest](#set_notifications).
3. [Aktivieren Sie die Konfiguration](#validate_activate).

Sie können die Funktion für das Datenmanagement auch über die APIs konfigurieren. Detaillierte Informationen zum Konfigurieren der Funktion für das Datenmanagement zur Normalisierung und Umwandlung Ihrer Gerätedaten mithilfe von APIs finden Sie in [Einführung zum Datenmanagement](ga_im_example.html#im_example). [Schrittweise Anleitung: Detailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle](../GA_information_management/ga_im_index_scenario.html#scenario) führt Sie durch die Schritte zur Erstellung einer logischen Schnittstelle eines Gerätetyps für heterogene Thermometergeräte. Details zur API finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.
{: tip}


## Entwurf einer physischen Schnittstelle erstellen (Einfacher Ablauf)
{: #create_physical_interface_simple}

1. Klicken Sie im Hauptnavigationsmenü auf **Geräte**.
2. Klicken Sie auf **Gerätetypen** und wählen Sie den Gerätetyp aus, für den Sie eine Schnittstelle erstellen möchten. Sie können auch einen neuen Gerätetyp erstellen. Weitere Informationen hierzu finden Sie unter [Geräte verbinden](../iotplatform_task.html#iotplatform_task).
3. Zeigen Sie die Informationen zum Gerätetyp an und klicken Sie auf **Schnittstelle**.
4. Klicken Sie auf **Einfacher Ablauf**, um die Funktion zur Erstellung einer einfachen Schnittstelle zu öffnen.
5. Klicken Sie auf **Schnittstelle erstellen**.
6. Klicken Sie auf **Eigenschaft hinzufügen**, um mit dem Hinzufügen von Ereignissen und Eigenschaften zu der physischen Schnittstelle zu beginnen.
   a. Wählen Sie im Fenster **Eigenschaften zur Schnittstelle hinzufügen** die Ereignisse aus, die zur Schnittstelle hinzugefügt werden sollen. Das System ist für aktive Ereignisse der angeschlossenen Geräte des ausgewählten Gerätetyps empfangsbereit. Sie können die Geräte-ID des zuletzt im Cache zwischengespeicherten Ereignisses oder aber ein anderes Ereignis in der Liste auswählen.
   b. Wählen Sie die Eigenschaften des Ereignisses aus und klicken Sie dann auf **Hinzufügen**.
   c. Fügen Sie bei Bedarf weitere Eigenschaften hinzu.
7. Klicken Sie auf **Fertig**. Die physische Schnittstelle wird erstellt. Wenn Sie weitere Details hinzufügen möchten, können Sie [ein Upgrade auf eine erweiterte Schnittstelle durchführen](#create_physical_interface_advanced), indem Sie den Link **Funktion zur Erstellung erweiterter Schnittstellen verwenden** benutzen.


## Entwurf einer physischen Schnittstelle erstellen (Erweiterter Ablauf)
{: #create_physical_interface_advanced}

1. Klicken Sie im Hauptnavigationsmenü auf **Geräte**.
2. Klicken Sie auf **Gerätetypen** und wählen Sie den Gerätetyp aus, für den Sie eine Schnittstelle erstellen möchten. Sie können auch einen neuen Gerätetyp erstellen. Weitere Informationen hierzu finden Sie unter [Geräte verbinden](../iotplatform_task.html#iotplatform_task).
2. Zeigen Sie die Informationen zum Gerätetyp an und klicken Sie auf **Schnittstelle**.
3. Klicken Sie auf **Erweiterter Ablauf**, um die Funktion zur Erstellung erweiterter Schnittstellen zu öffnen.
4. Wählen Sie eine der folgenden Optionen aus:
 - Wenn Sie eine neue physische Schnittstelle erstellen möchten, klicken Sie auf **Schnittstelle erstellen**.
 - Wenn Sie eine Schnittstellenvorlage aus der Bibliothek verwenden möchten, klicken Sie auf **Aus Bibliothek hinzufügen**, wählen Sie die Schnittstelle aus und wechseln Sie dann zu [Entwurf einer logischen Schnittstelle erstellen](#create_logic_interface).
5. Geben Sie auf der Registerkarte **Identität** der Seite **Physische Schnittstelle erstellen** den Namen und eine Beschreibung für die physische Schnittstelle ein.
6. Optional: Aktivieren Sie den Edge-Switch, wenn Sie Edge-fähige Geräte für die Schnittstelle einsetzen. Weitere Informationen zu Edge finden Sie unter [Edge Analytics](../edge_analytics.html#edge_analytics).
7. Klicken Sie auf **Weiter**.
8. Definieren Sie die physische Schnittstelle, indem Sie Ereignistypen und Nutzdaten hinzufügen und ändern:
   a. Wählen Sie einen Ereignistyp aus der Liste aus, um einen vorhandenen Ereignistyp zu ändern, oder klicken Sie auf **Ereignistyp erstellen**.
   b. Wählen Sie das zuletzt im Cache zwischengespeicherte Ereignis oder ein Ereignis in der Liste aus oder fügen Sie manuell ein Ereignis hinzu und klicken Sie dann auf **Hinzufügen**.
9. Klicken Sie auf **Fertig**. Die physische Schnittstelle wird im Entwurfsformat erstellt. Sie können nun mit dem Hinzufügen einzelner oder mehrerer logischer Schnittstellen fortfahren.

## Entwurf logischer Schnittstellen erstellen (Erweiterter Ablauf)
{: #create_logical_interface}

1. Nachdem Sie den Entwurf der physischen Schnittstelle erstellt haben, wählen Sie eine der folgenden Optionen aus:
 - Klicken Sie auf **Logische Schnittstelle hinzufügen**, um eine neue logische Schnittstelle zu erstellen.
 - Klicken Sie auf **Aus Bibliothek hinzufügen**, um eine Schnittstellenvorlage aus der Schnittstellenbibliothek zu verwenden. Wählen Sie die Schnittstelle aus und fahren Sie mit Schritt 7 fort.
2. Geben Sie auf der Registerkarte **Identität** der Seite **Logische Schnittstelle erstellen** den Namen und eine Beschreibung für die logische Schnittstelle ein.
3. Klicken Sie auf **Weiter**.
4. Klicken Sie auf **Eigenschaft hinzufügen**, um mit dem Definieren der Eigenschaften der logischen Schnittstelle zu beginnen.
5. Wählen Sie im Fenster **Eigenschaft erstellen** die Eigenschaften der physischen Schnittstelle aus, die zur logischen Schnittstelle hinzugefügt werden sollen, und speichern Sie die Änderungen.
6. Klicken Sie auf **Objekt hinzufügen**.
7. Fügen Sie weitere logische Schnittstellen bei Bedarf hinzu und klicken Sie dann auf **Weiter**.
8. {: #set_notifications}Konfigurieren Sie nun die Benachrichtigungsvorgaben für die Schnittstelle. Wählen Sie eine der folgenden Optionen aus, um festzulegen, wann die Benachrichtigungen zum Gerätestatus gesendet werden sollen:
 - Keine Ereignisbenachrichtigungen - Das System sendet keine Benachrichtigungen. Sie können die REST-API verwenden, um die Informationen zu den Änderungen am Gerätestatus abzurufen.
 - Bei Statusänderung - Das System sendet eine Benachrichtigung, wenn der Gerätestatus sich ändert.
 - Bei jedem Ereignis - Das System sendet jedes Mal eine Benachrichtigung, wenn die Plattform ein Ereignis für das Gerät verarbeitet. Dies gilt auch dann, wenn das Ereignis nicht zu einer Statusänderung führt.
8. Klicken Sie auf **Fertig**. Die Schnittstellen werden erstellt und ![Symbol für Entwurfsstatus](images/draft_icon.png) gibt den Entwurfsstatus an. Sie können fortfahren, um die Schnittstellen zu ändern oder zu aktivieren.

## Konfiguration aktivieren
{: #validate_activate}

Nachdem Sie den Entwurf der physischen und logischen Schnittstellen erstellt haben, müssen Sie die Konfiguration aktivieren.

- Klicken Sie auf **Aktivieren**, um die Konfiguration zu aktivieren. Der Konfigurationsstatus wird auf 'Aktiviert' gesetzt.
![Aktive Bereitstellung](images/active_deployment.png) Wenn Sie Änderungen an den Schnittstellen vornehmen, werden die Schnittstellen wieder zu Entwürfen und Sie müssen sie erneut aktivieren.
