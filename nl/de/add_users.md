---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-14"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Benutzerzugriff verwalten
{: #managing-user-access}

Über das Mitgliederdashboard können Sie den Zugriff auf Ihre {{site.data.keyword.iot_full}}-Organisation steuern und verwalten. Sie können Benutzer hinzufügen, indem Sie sie hinzufügen<!--, registering--> oder importieren. Durch die Zuweisung von Rollen können Sie Ihren Benutzern unterschiedliche Zugriffsebenen zuordnen.
{:shortdesc}

## Benutzer hinzufügen
{: #adding-new-users}

Auf der Registerkarte **Mitglieder** im Dashboard können Sie einzelne Mitglieder mithilfe der Funktion 'Hinzufügen' hinzufügen. Darüber hinaus ist es möglich, Mitglieder gleichzeitig mithilfe der Funktion 'Importieren' hinzuzufügen.

Mitgliederkonten haben standardmäßig kein Ablaufdatum. Wenn Sie Ihrer {{site.data.keyword.iot_short_notm}}-Organisation Mitglieder hinzufügen, können Sie für deren Konto optional ein Ablaufdatum festlegen.

### Mitglieder zur {{site.data.keyword.iot_short_notm}}-Organisation hinzufügen

Zum Hinzufügen von Mitgliedern zu Ihrer Organisation benötigen Sie die IBM ID des Mitglieds. Eine Bestätigung des Mitglieds benötigen Sie zum Hinzufügen des Mitglieds nicht.

Gehen Sie wie folgt vor, um Ihrer {{site.data.keyword.iot_short_notm}}-Organisation ein einzelnes Mitglied hinzuzufügen:
1. Wechseln Sie im {{site.data.keyword.iot_short_notm}}-Dashboard zu **Mitglieder**.
2. Klicken Sie auf **Mitglieder hinzufügen** und wählen Sie die Registerkarte **Hinzufügen** aus.
3. Geben Sie die IBM ID des Mitglieds ein.
4. Wählen Sie eine Rolle für das Mitglied aus.
5. Optional: Legen Sie ein Ablaufdatum für das Mitglied fest.
6. Klicken Sie auf **Hinzufügen**.

Zum gleichzeitigen Hinzufügen mehrerer Mitglieder müssen Sie eine CSV-Datei (`.csv`) hochladen, die für jedes Mitglied die IBM ID, die Rolle und das optionale Ablaufdatum enthält. Informationen finden Sie in [CSV-Datei erstellen](#constructing-your-csv).
1. Wechseln Sie im {{site.data.keyword.iot_short_notm}}-Dashboard zu **Mitglieder**.
2. Klicken Sie auf **Mitglieder hinzufügen** und wählen Sie die Registerkarte **Importieren** aus.
3. Durchsuchen Sie Ihre Dateien oder ziehen Sie die CSV-Datei (`.csv`) in das Fenster **CSV-Upload**.
4. Wählen Sie eine Standardrolle aus, die verwendet werden soll, wenn eine in der CSV-Datei angegebene Rolle nicht erkannt wird.
5. Ordnen Sie die Spaltennummern in Ihrer CSV-Datei der entsprechenden Einträgen für IBMid, Rolle und (optional) Ablaufdatum zu.
6. Wählen Sie das passende Komma- oder Semikolonspaltentrennzeichen aus, das mit dem Trennzeichen in Ihrer CSV-Datei (`.csv`) übereinstimmt.
7. Klicken Sie auf **Importieren**, um die IBMids zu importieren, und erstellen Sie die Mitglieder.

<!--
### Inviting members to your {{site.data.keyword.iot_short_notm}} organization

When you invite a user to become a member of your {{site.data.keyword.iot_short_notm}} organization, the user receives an email that contains an invitation link. Invitation links expire 48 hours after they are sent. If an invitation link is not used within 48 hours, the user must be invited again to receive a new invitation link.

**Important:** The invite feature requires a configured mail service. For more information, see the Email section of the [External service integrations](reference/extensions/index.html#email) topic.

To invite a member to your {{site.data.keyword.iot_short_notm}} organization:
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select the **Invite** tab.
3. Enter the email address of the member.
4. Select a role for this member.
5. Optional: Set an expiry date for the member.
6. Click **Invite Member**.

To invite multiple members simultaneously, you must upload a `.csv` file that contains the email address, role and the optional expiry date of each member. For information, see [Constructing your CSV file](#constructing-your-csv).
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select the **Import** tab.
3. Browse your files or drag the `.csv` file into the **Upload CSV** window.
4. Select a default role to use if a role specified in the CSV file is not recognized.
5. Map the column numbers in your CSV file to the corresponding email address, role, and (optional) expiry date entries.
6. Select the appropriate comma or semicolon column separator to match the separator used in your `.csv` file.
7. Click **Import** to send out the invitations. -->

<!-- ### Registering a member with your {{site.data.keyword.iot_short_notm}} organization

If your organization is using {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.ssoshort}}, you can add individual members to your organization by registering them, which does not require an IBMid.

To register a member with your {{site.data.keyword.iot_short_notm}} organization:
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select **Invite**.
3. Enter the email address of the member.
4. Select a role for this member.
5. Enter the subject, realm name, and issuer.
   **Important:** Ensure that the `Subject`, `Realm Name`, and `Issuer` fields comply with the OpenID Connect recommendations and standards. For more information, see the [OpenID Connect ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://openid.net/connect/){: new_window} website.
6. Optional: Set an expiry date for the member.
7. Click **Register Member**.

To register multiple members simultaneously, you must upload a CSV (`.csv`) file that contains the email address, role, subject, realm name, issuer, and the optional expiry date of each member.
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Access**.
2. Click **Add Member** and select **Import**.
3. Click **Bulk Register**.
4. Select a default role and ensure that the column numbers on your CSV file match the column numbers in the CSV settings.
5. Ensure the column separator in your CSV file matches the column separator in the CSV settings.
6. Click **Browse your files** or drag the CSV file into the **Upload CSV** window. -->

### CSV-Datei erstellen
{: #constructing-your-csv}

Beim Erstellen einer CSV-Datei für den Import von Mitgliedern in Ihre Organisation müssen Sie sicherstellen, dass Sie für die von Ihnen verwendete Methode die erforderlichen Informationen einschließen. Verwenden Sie zum Trennen von Spalten entweder Kommas oder Semikola.  
**Wichtig:** Stellen Sie beim Hochladen der CSV-Datei sicher, dass Sie unter **CSV-Einstellungen** das richtige Spaltentrennzeichen auswählen.

CSV-Beispieldatei mit Kommatrennzeichen:  
```
user1@sample.com,PD_DEVELOPER_USER,2018-03-13
user2@sample.com,PD_OPERATOR_USER,2018-03-13
user3@sample.com,PD_ADMIN_USER,2018-03-13
```
Wo die folgenden Spalteneinträge verwendet werden:  
- Spalte 1: Die E-Mail-Adresse des Benutzers.  
Wenn Sie Mitglieder hinzufügen, stellen Sie sicher, dass die verwendeten E-Mail-Adressen gültigen IBMids entsprechen. Wenn Sie Mitglieder einladen, können Sie alle gültigen E-Mail-Adressen verwenden.
- Spalte 2: Die Rolle, die dem Benutzer zugewiesen wird.  
Geben Sie eine der folgenden Rollen ein:
 - PD_ADMIN_USER
 - PD_OPERATOR_USER
 - PD_DEVELOPER_USER
 - PD_ANALYST_USER
 - PD_READER_USER  
 Weitere Informationen zu Benutzerrollen finden Sie in [Rollen für Benutzer, Anwendungen und Gateways](roles_index.html#user_roles).
- Spalte 3: Datum im ISO 6801-Format (z. B. `2018-03-13`), das dem Benutzerablaufdatum entspricht.

## Benutzer bearbeiten
{: #editing-users}

Beim Bearbeiten eines Benutzer können Sie die Benutzerrolle ändern, ein Ablaufdatum hinzufügen, entfernen oder ändern sowie Zugriff auf eine Organisation hinzufügen oder entfernen.

1. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard links in der Navigationsleiste auf **Mitglieder**.
2. Klicken Sie auf das Symbol **Bearbeiten** neben dem Benutzer, den Sie bearbeiten möchten.
3. Nehmen Sie die gewünschten Änderungen für den Benutzer vor.
4. Klicken Sie auf **Speichern**.

## Benutzerzugriff sperren
{: #blocking-users}

Sie können den Zugriff von Benutzern auf die {{site.data.keyword.iot_short_notm}}-Organisation sperren, ohne die Zugehörigkeit zu der Organisation zu ändern.

1. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard links in der Navigationsleiste auf **Mitglieder**.
2. Klicken Sie auf den Schalter neben dem Benutzer, für den der Zugriff auf die {{site.data.keyword.iot_short_notm}}-Organisation gesperrt werden soll.

## Benutzerzugriff einschränken
{: #limiting-users}

Die Zugriffssteuerung auf Ressourcenebene ermöglicht Ihnen die Einschränkung des Zugriffs auf Geräte innerhalb einer Organisation. Sie können Ressourcengruppen verwenden, um die Geräte anzugeben, die von den einzelnen Benutzern oder API-Schlüsseln verwaltet werden können. Weitere Informationen zur Vorgehensweise bei der Konfiguration der Zugriffssteuerung auf Ressourcenebene finden Sie in [Zugriffssteuerung auf Ressourcenebene konfigurieren](reference/rlac.html#configure_RLAC).

## Benutzer entfernen
{: #removing-users}

Sie können Benutzer vollständig aus der {{site.data.keyword.iot_short_notm}}-Organisation entfernen. Das Entfernen von Benutzern kann nicht rückgängig gemacht werden, d. h. die betreffenden Benutzer müssen erneut [zur Plattform hinzugefügt werden](#adding-new-users), um den Zugriff wiederherzustellen.

1. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard links in der Navigationsleiste auf **Mitglieder**.
2. Klicken Sie auf das Symbol **Löschen** neben dem Benutzer, den Sie entfernen möchten.
3. Klicken Sie im Bestätigungsdialog auf **Löschen**.
