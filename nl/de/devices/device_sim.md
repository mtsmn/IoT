---

copyright:
  years: 2017
lastupdated: "2017-09-28"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Gerätedaten simulieren (Beta)
{: #sim_device_data}

Mithilfe des {{site.data.keyword.iot_full}}-Gerätesimulators können Sie simulierte Ereignisse für Geräte einrichten. Sie können die Daten für simulierte Ereignisse verwenden, um mit allen Funktionen versehene {{site.data.keyword.iot_short_notm}}-Komponenten zu untersuchen, zu testen und vorzuführen.
{: shortdesc}

Sie können vorhandene Gerätetypen und Geräte verwenden. Der Simulator ermöglicht Ihnen das Generieren neuer Geräte für vorhandene Typen. Sie können die Ereignisdetails für jedes Gerät konfigurieren oder eine Standardkonfiguration einrichten, die auf alle Geräte angewendet wird. Sie können eine Konfiguration für simulierte Ereignisse exportieren, sodass sie wiederverwendet oder gemeinsam genutzt werden kann, um andere Simulationen einzurichten.

**Wichtig:** Die {{site.data.keyword.iot_full}}-Funktion für den Gerätesimulator steht nur als Bestandteil eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Gehen Sie wie folgt vor, um die Gerätedaten zu simulieren: 

1. Melden Sie sich bei {{site.data.keyword.iot_short_notm}} an.
2. Wählen Sie im Hauptnavigationsbereich **Einstellungen** aus.
3. Aktivieren Sie im Abschnitt **Experimentelle Features** den Gerätesimulator.
4. Wählen Sie im Hauptnavigationsbereich **Geräte** aus. In einer Nachricht, die in der rechten unteren Ecke angezeigt wird, ist angegeben, dass momentan keine Simulationen ausgeführt werden.
5. Klicken Sie auf die Nachricht "0 Simulationen werden ausgeführt".
6. Klicken Sie im Popup-Fenster **Simulationen** auf **Erste Simulation hinzufügen**.
7. Wählen Sie den Gerätetyp aus, für den Daten simuliert werden sollen.
8. Wählen Sie eine der folgenden Optionen aus:
  - Wählen Sie die Anzahl aus, wenn Sie einzelne oder mehrere neue Geräte hinzufügen wollen, und klicken Sie dann auf **Neues Gerät**. Die neuen Geräte werden aufgelistet.
  - Klicken Sie zum Hinzufügen eines bereits vorhandenen Geräts auf **Registriertes Gerät verwenden** und wählen Sie dann in der Liste ein Gerät aus.
  - Klicken Sie zum Importieren einer bereits vorhandenen Simulation auf **Importieren/Exportieren**, wählen Sie die Registerkarte **Importieren** aus und importieren Sie eine JSON-Datei oder kopieren Sie eine zuvor exportierte Konfiguration aus der Zwischenablage.
9. Klicken Sie auf ![Symbol 'Einstellungen'](images/settings_icon.png) und konfigurieren Sie die Simulationsdetails für einen Gerätetyp:
   1. Wählen Sie den Gerätetyp in der Liste aus und klicken Sie dann auf **+ Ereignistyp**, um einen Ereignistyp zum Gerätetyp hinzuzufügen.
   2. Wählen Sie einen Ereignistyp in der Liste aus und klicken Sie dann auf **Zeitplan**, um die Häufigkeit des Ereignisses festzulegen.
   3. Bearbeiten Sie die Ereignistypdetails im JSON-Format und speichern Sie den aktualisierten Ereignistyp.
   
   <p> Sie können die folgenden beiden speziellen Variablen verwenden, wenn Sie Ereignisse konfigurieren:  
        - `range`: Diese Variable wird durch eine nach dem Zufallsprinzip bestimmte Zahl ersetzt, die zwischen den beiden angegebenen Werten liegt. Beispiel: `{ "random": range(300, 100) }`  
        - `$counter`: Diese Variable wird durch die Anzahl der Geräte ersetzt, die im Simulator für den aktuellen Typ hinzugefügt werden. Beispiel: `{"total": $counter}`</p>
   {: tip}
   
   4. Wählen Sie aus, ob diese Einstellungen als Standardwerte für alle Geräte dieses Gerätetyps verwendet werden sollen oder ob Sie angepasste Werte für die einzelnen Geräte benutzen wollen. 
   5. Wiederholen Sie die Konfigurationsschritte für jedes Gerät, auf das angepasste Einstellungen angewendet werden sollen. Die Nachricht in der rechte unteren Ecke der Anzeige zeigt die Anzahl der aktiven Simulationen an.
10. **Optional:** Nachdem Sie Simulationen für Geräte konfiguriert haben, müssen Sie die Simulationsdetails exportieren, damit sie wiederverwendet oder gemeinsam genutzt werden können:
    1. Klicken Sie im Popup-Fenster **Simulationen** auf **Importieren/Exportieren**.
    2. Wählen Sie die Registerkarte **Exportieren** aus.
    3. Kopieren Sie die Simulationsdetails in die Zwischenablage oder laden Sie sie in einer JSON-Datei herunter.
11. Suchen Sie auf der Seite **Geräte** eines der simulierten Geräte und wählen Sie die Registerkarte **Kürzliche Ereignisse** aus, um zu überprüfen, ob die simulierten Ereignisse ordnungsgemäß funktionieren.
