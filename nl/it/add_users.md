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

# Gestione dell'accesso utente
{: #managing-user-access}

Dal dashboard dei membri, puoi controllare e gestire l'accesso alla tua organizzazione {{site.data.keyword.iot_full}}. Puoi aggiungere gli utenti aggiungendoli, invitandoli<!--, registering--> o importandoli. Puoi anche fornire diversi livelli di accesso ai tuoi utenti assegnando dei ruoli.
{:shortdesc}

## Aggiunta di utenti
{: #adding-new-users}

Dalla scheda **Members** del dashboard, puoi aggiungere singoli membri utilizzando la funzione di aggiunta. Puoi anche aggiungere membri contemporaneamente utilizzando la funzione di importazione.

Per impostazione predefinita, gli account dei membri non scadono. Quando aggiungi utenti alla tua organizzazione {{site.data.keyword.iot_short_notm}}, puoi facoltativamente impostare una data di scadenza per i relativi account.

### Aggiunta di membri alla tua organizzazione {{site.data.keyword.iot_short_notm}}

Per aggiungere un membro alla tua organizzazione, avrai bisogno dell'ID IBM del membro. Per aggiungere un membro, non hai bisogno della conferma da parte del membro.

Per aggiungere un solo membro alla tua organizzazione {{site.data.keyword.iot_short_notm}}:
1. Nel dashboard {{site.data.keyword.iot_short_notm}}, vai a **Members**.
2. Fai clic su **Add Members** e seleziona la scheda **Add**.
3. Immetti l'ID IBM del membro.
4. Seleziona un ruolo per il membro.
5. Facoltativo: imposta una data di scadenza per il membro.
6. Fai clic su **Add**.

Per aggiungere più membri contemporaneamente, devi caricare un file `.csv` che contiene l'ID IBM, il ruolo e facoltativamente la data di scadenza di ogni membro. Per informazioni, consulta [Messa a punto del tuo file CSV](#constructing-your-csv).
1. Nel dashboard {{site.data.keyword.iot_short_notm}}, vai a **Members**.
2. Fai clic su **Add Members** e seleziona la scheda **Import**.
3. Fai clic sui tuoi file o trascina il file `.csv` nella finestra **Upload CSV**.
4. Seleziona un ruolo predefinito da utilizzare se non viene riconosciuto un ruolo specificato nel file CSV.
5. Associa i numeri di colonna nel tuo file CSV all'ID IBM, al ruolo e (facoltativo) alle voci della data di scadenza corrispondenti.
6. Seleziona il separatore di colonna punto e virgola o virgola che corrisponde al separatore utilizzato nel tuo file `.csv`.
7. Fai clic su **Import** per importare gli ID IBM e creare i membri.

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

### Messa a punto del tuo file CSV
{: #constructing-your-csv}

Quando metti a punto un file CSV per importare i membri nella tua organizzazione, assicurati di includere le informazioni obbligatorie per il metodo che stai utilizzando. Utilizza le virgole o i punti e virgola per separare le colonne.  
**Importante:** quando carichi il file CSV, in **CSV settings**, assicurati di selezionare il separatore di colonna corretto.

File CSV di esempio con la delimitazione con virgola:  
```
user1@sample.com,PD_DEVELOPER_USER,2018-03-13
user2@sample.com,PD_OPERATOR_USER,2018-03-13
user3@sample.com,PD_ADMIN_USER,2018-03-13
```
Dove vengono utilizzate le seguenti voci di colonna:  
- Colonna 1: l'indirizzo email dell'utente.  
Se stai aggiungendo dei membri, assicurati di utilizzare l'indirizzo email corrispondente a un ID IBM valido. Se stai invitando i membri, puoi utilizzare qualsiasi indirizzo email valido.
- Colonna 2: il ruolo che deve essere assegnato all'utente.  
Immetti uno dei seguenti ruoli:
 - PD_ADMIN_USER
 - PD_OPERATOR_USER
 - PD_DEVELOPER_USER
 - PD_ANALYST_USER
 - PD_READER_USER  
 Per ulteriori informazioni sui ruoli utente, vedi [Ruoli utente, applicazione e gateway](roles_index.html#user_roles).
- Colonna 3: la data ISO 6801 (ad esempio, `2018-03-13`) che corrisponde alla data di scadenza dell'utente.

## Modifica degli utenti
{: #editing-users}

Gli utenti possono modificare il proprio ruolo, aggiungere, rimuovere o modificare una data di scadenza di accesso, oppure aggiungere o rimuovere l'accesso all'organizzazione.

1. Dal tuo dashboard {{site.data.keyword.iot_short_notm}}, fai clic su **Members** dalla barra di navigazione sulla sinistra.
2. Fai clic sull'icona **Edit** accanto all'utente che desideri modificare.
3. Apporta le modifiche desiderate all'utente.
4. Fai clic su **Save**.

## Blocco dell'accesso utenti
{: #blocking-users}

Gli utenti possono essere bloccati dall'accedere all'organizzazione {{site.data.keyword.iot_short_notm}}, mentre ancora mantengono l'appartenenza all'organizzazione.

1. Dal tuo dashboard {{site.data.keyword.iot_short_notm}}, fai clic su **Members** dalla barra di navigazione sulla sinistra.
2. Fai clic sull'attivazione accanto all'utente che desideri bloccare dall'accedere all'organizzazione {{site.data.keyword.iot_short_notm}}.

## Limitazione dell'accesso utente
{: #limiting-users}

Il controllo dell'accesso al livello della risorsa ti permette di limitare l'accesso ai dispositivi in un'organizzazione. Puoi utilizzare i gruppi di risorse per specificare i dispositivi che ogni utente o chiave API possono gestire. Per informazioni su come configurare il controllo dell'accesso al livello della risorsa, consulta [Configurazione del controllo dell'accesso al livello della risorsa](reference/rlac.html#configure_RLAC).

## Rimozione di utenti
{: #removing-users}

Gli utenti possono essere rimossi completamente dall'organizzazione {{site.data.keyword.iot_short_notm}}. La rimozione degli utenti non può essere annullata e gli utenti devono essere [aggiunti alla piattaforma](#adding-new-users) per ripristinare l'accesso.

1. Dal tuo dashboard {{site.data.keyword.iot_short_notm}}, fai clic su **Members** dalla barra di navigazione sulla sinistra.
2. Fai clic sull'icona **Delete** accanto all'utente che deve essere rimosso.
3. Fai clic su **Delete** nella finestra di dialogo di conferma.
