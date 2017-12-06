---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung zum Datenmanagement mithilfe der Webschnittstelle (Beta)
{: #gs_web}

**Wichtig:** Die {{site.data.keyword.iot_full}}-Webschnittstellenfunktion für das Datenmanagement steht nur als Bestandteil eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} stellt in der Funktion für das Datenmanagement Online-Tools bereit, die zur Vereinfachung der Normalisierung und Umwandlung von Daten dienen. Zusätzlich zur [Watson IoT Platform Data Management API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}, die bereitgestellt wird, können Sie die Funktion zur Erstellung einer einfachen Schnittstelle oder die Funktion zur Erstellung erweiterter Schnittstellen verwenden, um Ressourcen zu konfigurieren und mit der Zuordnung Ihrer Gerätedaten zu beginnen. Die Onlinefunktionen zur Erstellung von Schnittstellen führen Sie durch die Schritte in der Benutzerschnittstelle.

Informationen zur Funktion für das Datenmanagement und ihrer Vorzüge finden Sie in der [Einführung zum Datenmanagement](../GA_information_management/ga_im_device_twin.html#device_twins).

Informationen zu den Ressourcen, die Sie konfigurieren müssen, finden Sie unter dem Thema [Erklärung des Datenmanagements](../GA_information_management/ga_im_definitions.html#definitions_resource).

Mit der Funktion zur Erstellung einer einfachen Schnittstelle können Sie eine physische Schnittstelle hinzufügen. Dadurch wird automatisch eine logische Schnittstelle erstellt und die beiden Schnittstellen werden einander zugeordnet. Mit der Funktion zur Erstellung erweiterter Schnittstellen verfügen Sie über einen höheren Grad an Kontrolle über die Konfiguration. Sie fügen eine physische Schnittstelle und mindestens eine logische Schnittstelle hinzu und erstellen anschließend Zuordnungen zwischen diesen Komponenten.

Die Schnittstellen und Zuordnungen werden im Entwurfsformat hinzugefügt. Nachdem Sie sie hinzugefügt und überprüft haben, müssen Sie sie aktivieren. Weitere Informationen zu Entwurfsressourcen und aktiven Ressourcen finden Sie in [Ressourcen erstellen, aktualisieren, aktivieren und inaktivieren](../GA_information_management/ga_im_definitions.html#draft_active_resources).



## Allgemeiner Workflow
{: #interface_flow}

Die folgenden Schritte sollen Sie beim Konfigurieren der Ressourcen unterstützen, die zur Verwendung der Funktion für das Datenmanagement erforderlich sind.

**Vorbemerkungen**
Bei der vorliegenden Prozedur wird davon ausgegangen, dass Sie über einen Gerätetyp mit mindestens einem verbundenen und registrierten Gerät verfügen. Weitere Informationen zu diesen Voraussetzungen und Anweisungen zur Vorgehensweise bei der Erstellung eines Gerätetyps und der Registrierung eines Geräts finden Sie in [Geräte zur Verwendung mit der Funktion für das Datenmanagement konfigurieren](im_config_devices.html).

1. Wählen Sie den Gerätetyp aus, für den Daten normalisiert und umgewandelt werden sollen.
2. Wählen Sie entweder die Funktion zur Erstellung einer einfachen Schnittstelle oder die Funktion zur Erstellung erweiterter Schnittstellen aus:

a. Einfacher Ablauf  
   i. [Erstellen Sie einen Entwurf einer physischen Schnittstelle](#create_physical_interface_simple).  
   
b. Erweiterter Ablauf  
   i. [Erstellen Sie einen Entwurf einer physischen Schnittstelle](#create_physical_interface_advanced).  
   ii. [Erstellen Sie mindestens einen Entwurf einer logischen Schnittstelle](#create_logical_interface). (Dieser Schritt wird automatisch ausgeführt, wenn Sie den einfachen Ablauf verwenden.)  
   iii. [Erstellen Sie eine Zuordnung zwischen der physischen Schnittstelle und den logischen Schnittstellen](#create_interface_mappings).  
     
     
3. [Legen Sie die Benachrichtigungsvorgaben fest](#set_notifications).
4. [Aktivieren Sie die Konfiguration](#validate_activate).

*Hinweis:* Sie können die Funktion für das Datenmanagement auch über die APIs konfigurieren. Detaillierte Informationen zum Konfigurieren der Funktion für das Datenmanagement zur Normalisierung und Umwandlung Ihrer Gerätedaten mithilfe von APIs finden Sie in [Einführung zum Datenmanagement]{ga_im_example.html#im_example}. [Schrittweise Anleitung: Detailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle](../GA_information_management/ga_im_index_scenario.html#scenario) führt Sie durch die Schritte zur Erstellung einer logischen Schnittstelle eines Gerätetyps für heterogene Thermometergeräte.

Details zur API finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Entwurf einer physischen Schnittstelle erstellen (Einfacher Ablauf)
{: #create_physical_interface_simple}

1. Wählen Sie im Hauptnavigationsmenü die Option **Geräte** aus.
2. Wählen Sie **Gerätetypen** und dann den Gerätetyp aus, für den eine Schnittstelle erstellt werden soll. Sie können auch einen [neuen Gerätetyp erstellen].
3. Zeigen Sie die Informationen zum Gerätetyp an und aktualisieren Sie sie (sofern erforderlich). Speichern Sie alle vorgenommenen Änderungen.
4. Klicken Sie auf **Schnittstelle**.
5. Klicken Sie auf **Einfacher Ablauf**, um die Funktion zur Erstellung einer einfachen Schnittstelle zu öffnen.
6. Klicken Sie auf **Schnittstelle erstellen**.
7. Klicken Sie auf **Eigenschaft hinzufügen**, um mit dem Hinzufügen von Ereignissen und Eigenschaften zur physischen Schnittstelle zu beginnen.  
 a. Wählen Sie im Fenster **Eigenschaften zur Schnittstelle hinzufügen** die Ereignisse aus, die zur Schnittstelle hinzugefügt werden sollen. Das System ist für aktive Ereignisse der angeschlossenen Geräte des ausgewählten Gerätetyps empfangsbereit. Sie können die Geräte-ID des zuletzt im Cache zwischengespeicherten Ereignisses oder aber ein anderes Ereignis in der Liste auswählen.  
  b. Wählen Sie die Eigenschaften des Ereignisses aus und klicken Sie dann auf **Hinzufügen**.  
  c. Fügen Sie bei Bedarf weitere Eigenschaften hinzu.
8. Klicken Sie auf **Fertig**. Die physische Schnittstelle wird erstellt. Wenn Sie weitere Details hinzufügen möchten, dann können Sie für die Schnittstelle ein Upgrade durchführen, indem Sie den Link **Funktion zur Erstellung erweiterter Schnittstellen verwenden** benutzen.


## Entwurf einer physischen Schnittstelle erstellen (Erweiterter Ablauf)
{: #create_physical_interface_advanced}

1. Wählen Sie im Hauptnavigationsmenü die Option **Geräte** aus.
2. Wählen Sie **Gerätetypen** und dann den Gerätetyp aus, für den eine Schnittstelle erstellt werden soll. Sie können auch einen [neuen Gerätetyp erstellen].
2. Zeigen Sie die Informationen zum Gerätetyp an und klicken Sie auf **Schnittstelle**.
4. Klicken Sie auf **Erweiterter Ablauf**, um die Funktion zur Erstellung erweiterter Schnittstellen zu öffnen.
5. Klicken Sie auf **Schnittstelle erstellen**.
7. Geben Sie auf der Registerkarte **Identität** der Seite **Physische Schnittstelle erstellen** den Namen und eine Beschreibung für die physische Schnittstelle ein.
7. Optional: Aktivieren Sie den Edge Analytics-Switch, wenn Sie Edge-fähige Geräte für die Schnittstelle einsetzen.
8. Klicken Sie auf **Weiter**.
9. Definieren Sie die physische Schnittstelle, indem Sie Ereignistypen und Nutzdaten hinzufügen:
 a. Klicken Sie auf **Ereignistyp erstellen**. 
 b. Wählen Sie das zuletzt im Cache zwischengespeicherte Ereignis oder ein Ereignis in der Liste aus oder fügen Sie manuell ein Ereignis hinzu.
10. Klicken Sie auf **Fertig**. Die physische Schnittstelle wird im Entwurfsformat erstellt. Sie können nun mit dem Hinzufügen einzelner oder mehrerer logischer Schnittstellen fortfahren.

## Entwurf einer logischen Schnittstelle erstellen
{: #create_logical_interface}

1. Klicken Sie, nachdem Sie den Entwurf einer physischen Schnittstelle erstellt haben, auf **Logische Schnittstelle hinzufügen**.
2. Geben Sie auf der Registerkarte **Identität** der Seite **Logische Schnittstelle erstellen** den Namen und eine Beschreibung für die logische Schnittstelle ein.
3. Klicken Sie auf **Weiter**.
4. Klicken Sie auf **Eigenschaft hinzufügen**, um mit dem Definieren der Eigenschaften der logischen Schnittstelle zu beginnen.
5. Wählen Sie im Fenster **Eigenschaft erstellen** die Eigenschaften der physischen Schnittstelle aus, die zur logischen Schnittstelle hinzugefügt werden sollen, und speichern Sie die Änderungen.
6. Fügen Sie weitere logische Schnittstellen bei Bedarf hinzu und klicken Sie dann auf **Weiter**.
7. {: #set_notifications}Konfigurieren Sie nun die Benachrichtigungsvorgaben für die Schnittstelle. Wählen Sie eine der folgenden Optionen aus, um festzulegen, wann die Benachrichtigungen zum Gerätestatus gesendet werden sollen:
 - Keine Ereignisbenachrichtigungen - Das System sendet keine Benachrichtigungen. Sie können die REST-API verwenden, um die Informationen zu den Änderungen am Gerätestatus abzurufen.
 - Bei Statusänderung - Das System sendet eine Benachrichtigung, wenn der Gerätestatus sich ändert.
 - Bei jedem Ereignis - Das System sendet jedes Mal eine Benachrichtigung, wenn die Plattform ein Ereignis für das Gerät verarbeitet. Dies gilt auch dann, wenn das Ereignis nicht zu einer Statusänderung führt.
8. Klicken Sie auf **Fertig**. Die Schnittstellen werden im Entwurfsformat erstellt.
9. {: #validate_activate} Klicken Sie auf **Bereitstellen**, um die Konfiguration zu überprüfen.
10. Klicken Sie auf **Bereitstellen**, um die Konfiguration zu aktivieren. Als Konfigurationsstatus wird 'Bereitgestellt' festgelegt. 

