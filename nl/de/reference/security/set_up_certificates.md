---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-13"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Zertifikate konfigurieren
{: #set_up_certificates}

Zertifikate werden für die Geräeteauthentifizierung oder zum Ersetzen des {{site.data.keyword.iot_full}}-Standardserverzertifikats für die MQTT-Nachrichtenübertragung verwendet. Geräten, die über keine gültigen und signierten Zertifikate verfügen, wird der Zugriff verweigert und eine Kommunikation mit dem Server ist nicht möglich.

Um Zertifikate und den Serverzugriff für Geräte zu konfigurieren, registriert der Systembediener die zugehörigen Zertifikate der Zertifizierungsstelle (CA-Zertifikate) und optional die Zertifikate des Messaging-Servers in {{site.data.keyword.iot_short_notm}}.

Informationen zur Verwendung der APIs für die Verwaltung von CA-Zertifikaten und Zertifikaten des Messaging-Servers finden Sie in der Dokumentation zu den [Authentifizierungs- und Autorisierungs-APIs für IBM Watson IoT Platform ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}.

## Zertifikate einer Zertifizierungsstelle
Zertifikate einer Zertifizierungsstelle (CA-Zertifikate) ermöglichen der Organisation das Erkennen vertrauenswürdiger Clientzertifikate für Geräte und damit den betreffenden Geräten das Herstellen einer Verbindung zu dem Server.

Wenn Sie ein CA-Zertifikat hinzufügen oder das Zertifikat des Messaging-Servers ersetzen, müssen alle Geräte mithilfe eines MQTT-Clients eine Verbindung herstellen, der SNI (Server Name Indication) unterstützt, damit der Server die entsprechenden Zertifizierungsstellen verwenden kann, um das Gerät zu authentifizieren.

Sie können die Ebene für die Verbindungssicherheit festlegen, indem Sie die Verbindungssicherheitsrichtlinie konfigurieren. Weitere Informationen hierzu finden Sie unter [Sicherheitsrichtlinien konfigurieren](set_up_policies.html).

## Clientzertifikate

Einzelclient- (Geräte- oder Gateway-) Zertifikate verbleiben auf den Geräten und werden nicht in die Plattform hochgeladen. Das zum Signieren aller Geräte- und Gateway-Zertifikate verwendete CA-Signaturzertifikat wird als einziges Zertifikat in die Plattform hochgeladen. Wenn Sie selbst signierte Messaging-Serverzertifikate verwenden, müssen Sie die Stamm- und Zwischenzertifikate hochladen, die zum Signieren des Clientzertifikats (cert.pem) verwendet werden.

Für das Einzelgeräte- oder Gateway-Zertifikat, das Sie mit dem CA-Zertifikat signieren, muss die Geräte-ID oder Gateway-ID entweder als allgemeiner Name (Common Name, CN) oder als 'SubjectAltName' im Zertifikat eingegeben werden.

Für Geräte ist das **CN**-Feldformat `CN=d:devtype:devid` und das **SubjectAltName**-Feldformat `SubjectAltName=email:d:*devtype:devid*`, wobei `email:d` konstant ist, `*devtype*` der Gerätetyp des Geräts und `*devid*` die ID des Geräts in der Plattform.

Für Gateways ist das **CN**-Feldformat `CN=g:typeId:deviceId` und das **SubjectAltName**-Feldformat SubjectAltName=email:g:*devtype:devid*

Hinweis: Schließen Sie die `Organisations-ID` nicht in die Felder **CN** oder **SubjectAltName** für Geräte- oder Gateway-Zertifikate ein. Die `Organisations-ID` sollte als Teil der SNI-Informationen angegeben werden, die bei der Verbindungsherstellung zum Messaging-Server durch die Clientimplementierung bereitgestellt werden.

Weitere Informationen zu Clientzertifikaten finden Sie in der [Anleitung zum Verbinden von Raspberry Pi mit IBM Watson IoT Platform mit clientseitigen Zertifikaten![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}.

## Messaging-Server-Zertifikate

Messaging-Server-Zertifikate akzeptieren die Standarddomäne 'internetofthings.ibmcloud.com'. Folgendes Format muss für den Zertifikats-CN oder 'SubjectAltName' befolgt werden:

- `orgId.messaging.internetofthings.ibmcloud.com` (IoTP-Domäne)

Das folgende Beispiel zeigt einen gültigen CN für das Serverzertifikat:

`mtxpd0.messaging.internetofthings.ibmcloud.com`

Weitere Informationen zu Messaging-Server-Zertifikaten finden Sie in der [Anleitung zum Verbinden von Raspberry Pi mit IBM Watson IoT Platform mit selbst signierten Serverzertifikaten ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}.

### Angepasste Domänen (Beta)
{: #custom_domains}

**Wichtig**: Die Funktion 'Angepasste Domänen' für Messaging-Server-Zertifikate steht nur als Teil des eingeschränkten Beta-Programms zur Verfügung. Zur Aktivierung der angepassten Domänen aktivieren Sie **Experimentelle Features** auf der Seite **Einstellungen**.

Als Teil der Beta-Funktion akzeptieren Messaging-Server-Zertifikate angepasste Domänen. Folgendes Format muss für den Zertifikats-CN oder 'SubjectAltName' befolgt werden:

- `orgId.messaging.<custom domain>`

Das Feld **CN** akzeptiert Platzhalterzeichen für angepasste Domänen, wie im folgenden Beispiel gezeigt:

- `CN=*.messaging.mywiotpcustomdomain.com`

**Wichtig**: Für angepasste Domänen ist ein externer DNS-Service erforderlich, um die angepasste Domäne in den {{site.data.keyword.iot_full}}-Messaging-Server aufzulösen. Dieser DNS-Service wird nicht von der Plattform bereitgestellt.

## Zertifikate der Zertifizierungsstelle für die Geräteauthentifizierung registrieren
{: #reg_ca_cert}

1. Melden Sie sich bei {{site.data.keyword.iot_short_notm}} an und navigieren Sie zu **Allgemeine Einstellungen**.
2. Klicken Sie im Abschnitt **Sicherheit** unter **CA-Zertifikate** auf **Zertifikat hinzufügen**.
3. Navigieren Sie zu der Zertifikatsdatei, die hochgeladen werden soll, oder legen Sie eine Datei mithilfe der Drag-und-Drop-Funktion im Fenster **Zertifikat hinzufügen** ab. In der Datei darf nur ein Zertifikat enthalten sein, welches gültige Datumsangaben aufweisen muss. Nur Zertifikate im Format .pem oder .der werden akzeptiert. Sie können eine Vorschau der Zertifikatsinformationen in der ausgewählten Datei anzeigen.
4. Geben Sie eine Beschreibung für die Zertifikatsdatei ein.
5. Stellen Sie sicher, dass die richtige Datei ausgewählt ist, und klicken Sie auf **Speichern**. Das ausgewählte Zertifikat wird in die Tabelle aufgenommen und ist standardmäßig aktiviert.

## Messaging-Serverzertifikate registrieren
{: #reg_msg_cert}

Mit der Plattform wird ein Standardserverzertifikat bereitgestellt. Sie können dieses Standardzertifikat verwenden oder ein Zertifikat von Ihrer Organisation hochladen. Wenn Sie noch kein Zertifikat haben, können Sie eine Anforderung für ein neues Zertifikat erstellen. Nach dem Erhalt des neuen Zertifikats muss es signiert werden und anschließend zurück auf die Plattform hochgeladen werden.

**Hinweis:** Die Dashboardseiten der Plattform stellen möglicherweise interne Verbindungen zum Messaging-Server her, um Geräteinformationen abzurufen. Beim Einrichten von selbst signierten Serverzertifikaten für eine Organisation müssen Dashoardbenutzer das Serverzertifikat in ihren Browsern als vertrauenswürdiges Zertifikat hinzufügen, um Verbindungsprobleme zu vermeiden, die entstehen können, weil die Browser den Messaging-Server standardmäßig nicht als vertrauenswürdigen Server erkennen.

## Messaging-Server-Zertifikat aus Ihrer Organisation hochladen
{: #upload_cert}
1. Klicken Sie unter **Allgemeine Einstellungen** im Abschnitt **Sicherheit** unter **Messaging-Serverzertifikate** auf **Zertifikat hinzufügen**.
2. Navigieren Sie zu der Zertifikatsdatei, die hochgeladen werden soll, oder legen Sie eine Datei mithilfe der Drag-und-Drop-Funktion im Fenster **Zertifikat hinzufügen** ab.
3. Navigieren Sie zu der Datei mit privatem Schlüssel, die hochgeladen werden soll, oder legen Sie eine Datei mithilfe der Drag-und-Drop-Funktion im Fenster **Zertifikat hinzufügen** ab.
4. Geben Sie die Kennphrase des privaten Schlüssels ein, wenn der private Schlüssel mit einer Kennphrase verschlüsselt wurde.
5. Stellen Sie sicher, dass die richtige Datei ausgewählt ist, und klicken Sie auf **Speichern**.

## Messaging-Server-Zertifikat aktivieren

Um das Standardzertifikat oder ein anderes Zertifikat zu aktivieren, das bereits hochgeladen wurde, wählen Sie das gewünschte Zertifikat in der Dropdown-Liste **Standard-Messaging-Server-Zertifikat** in der Tabelle unter **Messaging-Server-Zertifikate** aus. Das ausgewählte Zertifikat wird in der Tabelle als das aktive Zertifikat angezeigt.

## Neues Messaging-Server-Zertifikat anfordern
{: #request_cert}

Wenn Sie ein neues Messaging-Serverzertifikat verwenden möchten, können Sie eine Zertifikatssignieranforderung (Certificate Signing Request, CSR) generieren, um ein neues Zertifikat anzufordern.

1. Klicken Sie unter **Allgemeine Einstellungen** im Abschnitt **Sicherheit** unter **Messaging-Serverzertifikate** auf **CSR erstellen**.
2. Geben Sie die Details zum Anfordern einer CSR für Ihren Server ein und klicken Sie auf **Generieren**. Die Zertifikatssignieranforderung wird in der Tabelle angezeigt.
3. Laden Sie die Anforderung herunter und übergeben Sie sie zum Signieren an eine Zertifizierungsstelle.
4. Nach dem Abruf eines Zertifikats kehren Sie zum CSR-Eintrag in der Tabelle zurück und laden das neue Zertifikat hoch. Nachdem das Zertifikat hochgeladen wurde, wird die Zertifikatssignieranforderung in der Tabelle durch das hochgeladene Zertifikat ersetzt.
