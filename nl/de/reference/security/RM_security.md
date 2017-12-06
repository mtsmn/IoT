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

# Risiko- und Sicherheitsmanagement
{: #RM_security}

Sie können die Sicherheit verbessern, um das Erstellen, Durchsetzen und Melden der Verbindungssicherheit von Geräten zu ermöglichen. Dank dieser erhöhten Sicherheit werden neben den von {{site.data.keyword.iot_short_notm}} verwendeten Benutzer-IDs und Tokens Zertifikate und die TLS-Authentifizierung (TLS, Transport Layer Security) verwendet, um festzulegen, wie und wo Geräte eine Verbindung zu der Plattform herstellen.

## Zertifikate
{: #certificates}

Sind Zertifikate aktiviert, wird während der Kommunikation zwischen Geräten und dem Server allen Geräten, die in ihren Sicherheitseinstellungen nicht über gültige Zertifikate verfügen, der Zugriff verweigert, selbst wenn sie über gültige Benutzer-IDs und Kennwörter verfügen.

Um Zertifikate und den Serverzugriff für Geräte zu konfigurieren, registriert der Systembediener die zugehörigen Zertifikate der Zertifizierungsstelle (CA-Zertifikate) und optional die Zertifikate des Messaging-Servers in der Watson IoT Platform.
Um Clientzertifikate und den Serverzugriff für Geräte zu konfigurieren, importiert der Systembediener die zugehörigen Zertifikate der Zertifizierungsstelle (CA-Zertifikate) und des Messaging-Servers in {{site.data.keyword.iot_short_notm}}. Der Sicherheitsanalyst konfiguriert dann die Standardsicherheitsrichtlinien für Verbindungen zwischen Geräten und der Plattform. Der Analyst kann verschiedene Richtlinien für unterschiedliche Gerätetypen hinzufügen.

Informationen zum Konfigurieren von Zertifikaten finden Sie in [Zertifikate konfigurieren](set_up_certificates.html).

## Sicherheitseinstellungen
{: #connect_policy}

Die Sicherheitseinstellungen, einschließlich der Verwendung von Verbindungsrichtlinieneinstellungen, QA-Zertifikaten und Messaging-Server-Zertifikaten, erzwingen die Art der Verbindung zwischen Geräten und der Plattform. Sie können Standardverbindungsrichtlinien für alle Gerätetypen festlegen und angepasste Einstellungen für bestimmte Gerätetypen erstellen. Die Richtlinie kann so eingerichtet werden, dass unverschlüsselte Verbindungen möglich sind, nur TLS-Verbindungen zulässig sind oder Geräte sich mit clientseitigen Zertifikaten authentifizieren müssen.

Informationen zum Konfigurieren von Sicherheitsrichtlinien für Verbindungen finden Sie in [Sicherheitsrichtlinien konfigurieren](set_up_policies.html).

Mit Richtlinien für Blacklists und Whitelists können die Positionen gesteuert werden, von denen aus Geräte eine Verbindung zum Konto der Organisation herstellen dürfen. Eine Blacklist gibt alle IP-Adressen, CIDRs oder Länder an, für die der Serverzugriff verweigert wird. Eine Whitelist erteilt explizit Zugriff auf bestimmte IP-Adressen.

Informationen zum Konfigurieren von Richtlinien für Blacklists und Whitelists finden Sie in [Blacklists und Whitelists konfigurieren](set_up_policies.html#config_black_white).

## Organisationspläne und Sicherheitsrichtlinien
Die erweiterten Sicherheitsrichtlinien ermöglichen Organisationen, mithilfe von Verbindungsrichtlinien sowie Blacklist- und Whitelist-Richtlinien festzulegen, wie Geräte verbunden und bei der Plattform authentifiziert werden. Welche Optionen für Sicherheitsrichtlinien in einer Organisation verfügbar sind, hängt vom Plantyp der Organisation ab:

**Standardplan:**
- Systembediener können Verbindungsrichtlinien mit den folgenden Optionen konfigurieren:
    - TLS Optional
    - TLS mit Tokenauthentifizierung
    - TLS mit Token- und Clientzertifikatsauthentifizierung

**Advanced Security Plan (ASP) oder Lite-Plan:**
- Systembediener können Verbindungsrichtlinien mit den folgenden Optionen konfigurieren:
    - TLS Optional
    - TLS mit Tokenauthentifizierung
    - TLS mit Clientzertifikatsauthentifizierung
    - TLS mit Token- und Clientzertifikatsauthentifizierung
    - TLS mit Clientzertifikat oder Token
- Systembediener können Blacklists oder Whitelists konfigurieren.

## Dashboard und Berichte für das Risiko- und Sicherheitsmanagement
{: #dashboard}

Der Systembediener und der Sicherheitsanalyst können über das Dashboard für Risiko- und Sicherheitsmanagement den allgemeinen Sicherheitsstatus anzeigen. Karten auf dem Dashboard können einen umfassenden Überblick über die Konformität und über den Verbindungsstatus von Geräten liefern.

Folgende Dashboard-Karten stehen für die Analyse von Risiken und Konformität zur Verfügung:
 - **Verbindungssicherheit-Konformität** - Zeigt den Grad der Konformität von Geräten an, die mit dem Server verbunden sind.
 - **Blacklist-/Whitlelist-Konformität** - Zeigt den Grad der Konformität von Geräten anhand der jeweils aktiven Blacklist- oder Whitelist-Richtlinie an.
 - **Richtlinienkonformität insgesamt** (Beta) - Zeigt den Konformitätsgrad insgesamt anhand aller geltenden Richtlinien an.
 - **Richtlinienverstöße** (Beta) - Zeigt die Verstöße insgesamt für die einzelnen Richtlinien an.

**Wichtig**: Die Karten **Richtlinienkonformität insgesamt** und **Richtlinienverstöße** stehen nur als Bestandteil des eingeschränkten Beta-Programms zur Verfügung. Zur Aktivierung dieser Dashboard-Karten aktivieren Sie **Experimentelle Features** auf der Seite **Einstellungen**.

### Richtlinienberichte mit Drilldown-Funktion (Beta)
{: #drill_down}

**Wichtig**: Die Drilldown-Berichtsfunktion für Risikomanagement-Richtlinien steht nur als Teil des eingeschränkten Beta-Programms zur Verfügung. Zur Aktivierung von Drilldown-Berichten aktivieren Sie **Experimentelle Features** auf der Seite **Einstellungen**.

Als Bestandteil der Beta-Funktion kann der Systembediener in jedem Bericht einen Drilldown durchführen, um die spezifischen Details zu den Geräten anzuzeigen, die mit den Richtlinien konform sind bzw. gegen diese verstoßen.

Zugriff auf die Drilldown-Berichte haben Sie in den Dashboard-Karten auf der Seite **Richtlinien**, wo alle Sicherheitsrichtlinie aufgelistet sind, und über die Detailseiten für die einzelnen Richtlinien. In den Berichten ist ein Drilldown auf drei Detaillierungsebenen möglich:
1. Auf der ersten Ebene sind alle Richtlinien aufgelistet, die im ausgewählten Richtlinientyp enthalten sind, z. B. die Standardverbindungsrichtlinie und alle angepassten Richtlinien.
2. Auf der zweiten Ebene sind die Geräte aufgeführt, die gegen die Richtlinie verstoßen, sowie der jeweilige Grund für den Verstoß.
3. Auf der dritten Ebene sind die spezifischen Details zu jedem Gerät aufgeführt, das gegen die Richtlinie verstößt.
