---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung zum Datenmanagement mithilfe von REST-APIs
{: #im_example}

Die folgenden Schritte sollen Sie beim Konfigurieren der Ressourcen unterstützen, die für die Verwendung der Gerätezwillings- und Assetzwillingsfunktion der Datenmanagementkomponente von {{site.data.keyword.iot_full}} erforderlich sind.

Details zur API finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Allgemeiner Workflow
{: #workflow}

Die folgenden Schritte sollen Sie beim Konfigurieren der erforderlichen Ressourcen unterstützen, die zum Zuordnen Ihrer Geräte- oder 'Ding'-Daten mithilfe der Funktion für Geräte- oder Assetzwillinge erforderlich sind.

**Tipp:** Nähere Informationen zu den einzelnen Schritten finden Sie in den Beispielszenarios. Über die entsprechenden Links können Sie auch direkt zu einem bestimmten Schritt in dem Beispielszenario gelangen. Die [schrittweise Anleitung 1](ga_im_index_scenario.html#scenario) führt Sie durch die Schritte zum Erstellen einer logischen Schnittstelle für Gerätetypen für heterogene Thermometergeräte. Die [schrittweise Anleitung 2](../information_management/im_index_scenario_thing.html#scenario) erweitert das Szenario indem beschrieben wird, wie eine logische Schnittstelle erstellt wird, über die die Anwendung Daten von zwei unterschiedlichen Klimagerätetypen verarbeiten kann, die in einem 'Ding namens 'Raumtyp' zusammengeführt sind.

Je nachdem, welchem Typ (Gerät oder 'Ding') eine von Ihnen erstellte logische Schnittstelle zugeordnet ist, ist der Prozess zum Erstellen und Konsumieren von logischen Schnittstellen ein wenig anders.

### Vorbereitende Schritte
Um eine logische Schnittstelle zu erstellen, die einem Geräte- oder 'Ding'-Typ zugeordnet ist, muss [mindestens ein Gerät für {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) registriert sein und Ereignisse mit Statuseigenschaften senden.  


### Schritte

1. 	Definieren Sie die eingehenden Statuseigenschaften.  
Legen Sie zuerst die eingehenden Statuseigenschaften fest, die von Ihrer logischen Schnittstelle für Anwendungen zur Verfügung gestellt werden sollen.  
Führen Sie je nach der logischen Schnittstelle, die Sie erstellen, eine der folgenden zwei Schritte aus:
<dl>
<dt>Gerätetyp: Erstellen Sie eine physische Schnittstelle.</dt>
<dd>
<ol>
<li>[Erstellen Sie eine Ereignisschemadatei](ga_im_index_scenario.html#step1). Bei der Ereignisschemadatei handelt es sich um eine lokale .JSON-Datei, in der die Struktur und das Format eines eingehenden Ereignisses definiert ist.
<li>[Erstellen Sie eine Ereignisschemaressource für Ihren Ereignistyp](ga_im_index_scenario.html#step2). Das Ereignisschema ist ein programmgesteuertes Konstrukt, das von {{site.data.keyword.iot_short_notm}} verwendet wird.
<li>[Erstellen Sie einen Ereignistyp, der das Ereignisschema referenziert](ga_im_index_scenario.html#step3). Der Ereignistyp wird von {{site.data.keyword.iot_short_notm}} verwendet, um eine oder mehrere Ereignisschemaressourcen zu einer physischen Schnittstelle zuzuordnen.
<li>[Erstellen Sie eine physische Schnittstelle](ga_im_index_scenario.html#step7).
<li>[Fügen Sie den Ereignistyp zu der physischen Schnittstelle hinzu](ga_im_index_scenario.html#step8).
<li>[Fügen Sie die physische Schnittstelle zu Ihrem Gerätetyp hinzu](ga_im_index_scenario.html#step9).
</ol>
</dd>
<dt>'Ding'-Typ: Definieren Sie einen 'Ding'-Typ.</dt>
<dd>
<ol>
<li>[Erstellen Sie eine Schemadatei für den 'Ding'-Typ](../information_management/im_index_scenario_thing.html#crt_composition_file).  
Eine Schemadatei eines 'Ding'-Typs ist eine lokale JSON-Datei, die die Zusammensetzung des 'Ding'-Typs definiert, indem sie auf vorhandene logische Schnittstellen verweist.
<li>[Erstellen Sie eine Schemaressource für den 'Ding'-Typ](../information_management/im_index_scenario_thing.html#crt_composition_resource).  
Laden Sie die lokale JSON-Datei auf {{site.data.keyword.iot_short_notm}} hoch.
<li>[Erstellen Sie einen 'Ding'-Typ](../information_management/im_index_scenario_thing.html#crt_thing_type). Ein 'Ding'-Typ dient demselben Zweck wie ein Gerätetyp, indem er eine Klasse von Dingen darstellt.
</ol>
</dd>
</dl>
4. 	Erstellen Sie die logische Schnittstelle.
 1. 	Erstellen Sie eine Schemadatei für die logische Schnittstelle für den [Gerätetyp](ga_im_index_scenario.html#step4) oder ['Ding'-Typ](../information_management/im_index_scenario_thing.html#crt_ai_schema_file).  
Eine Schemadatei einer logischen Schnittstelle ist eine lokale JSON-Datei, die den Gerätestatus definiert, der für Ihre Anwendungen bereitgestellt wird.
 2. 	Erstellen Sie eine Schemaressource für die logische Schnittstelle für den [Gerätetyp](ga_im_index_scenario.html#step5) oder ['Ding'-Typ](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource).
 3.	Erstellen Sie eine logische Schnittstelle für den [Gerätetyp](ga_im_index_scenario.html#step6) oder ['Ding'-Typ](../information_management/im_index_scenario_thing.html#crt_thing_ai).
 4.	Fügen Sie die logische Schnittstelle zum [Gerätetyp](ga_im_index_scenario.html#step10) oder ['Ding'-Typ](../information_management/im_index_scenario_thing.html#add_thing_ai) hinzu.
5. 	Definieren Sie die Zuordnungen für den [Gerätetyp](ga_im_index_scenario.html#step11) oder ['Ding'-Typ](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings).   
Zuordnungen werden zum Zuordnen von eingehenden Eigenschaften zu den Eigenschaften in der logischen Schnittstelle verwendet.  
6. 	Validieren und aktivieren Sie die Konfiguration, die dem Entwurf des [Gerätetyps](ga_im_index_scenario.html#step15) oder ['Ding'-Typs](../information_management/im_index_scenario_thing.html#activate) zugeordnet ist.
7. 	Rufen Sie den [Geräte](ga_im_index_scenario.html#step13)- oder ['Ding'](../information_management/im_index_scenario_thing.html##verify_Thing_state)-Status ab.  
Stellen Sie sicher, dass die Subskriptionen die aktualisierten Gerätedaten anzeigen oder dass die aktualisierten Gerätedaten mithilfe eines REST-Aufrufs oder durch Subskribieren einer Topic-Zeichenfolge zurückgegeben werden.  



