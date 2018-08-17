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

# Gestión de acceso de usuario
{: #managing-user-access}

Desde el panel de control de miembros, puede controlar y gestionar el acceso a la organización de {{site.data.keyword.iot_full}}. Puede añadir usuarios añadiéndolos, invitándolos<!--, registering--> o importándolos. También puede dar distintos niveles de acceso a los usuarios asignando roles.
{:shortdesc}

## Adición de usuarios
{: #adding-new-users}

En el separador **Miembros** del panel de control, puede añadir miembros mediante la función Añadir. También puede añadir miembros simultáneamente mediante la función Importar.

De forma predeterminada, las cuentas de los miembros no caducan. Al añadir usuarios a la organización de {{site.data.keyword.iot_short_notm}}, puede, si lo desea, establecer una fecha de caducidad para su cuenta.

### Adición de miembros a la organización de {{site.data.keyword.iot_short_notm}}

Para añadir un miembro a su organización, necesitará el IBMid del miembro. Para añadir un miembro, no necesita que este se lo confirme.

Para añadir un solo miembro a la organización de {{site.data.keyword.iot_short_notm}}:
1. En el panel de control de {{site.data.keyword.iot_short_notm}}, vaya a **Miembros**.
2. Pulse **Añadir miembros** y seleccione el separador **Añadir**.
3. Especifique el IBMid del miembro.
4. Seleccione un rol para el miembro.
5. Opcional: Establezca una fecha de caducidad para el miembro.
6. Pulse **Añadir**.

Para añadir varios miembros simultáneamente, debe cargar un archivo `.csv` que contiene el IBMid, el rol y la fecha de caducidad opcional de cada miembro. Para obtener información, consulte [Construir el archivo CSV](#constructing-your-csv).
1. En el panel de control de {{site.data.keyword.iot_short_notm}}, vaya a **Miembros**.
2. Pulse **Añadir miembros** y seleccione el separador **Importar**.
3. Examine los archivos o arrastre el archivo `.csv` a la ventana **Cargar CSV**.
4. Seleccione un rol predeterminado para utilizar si no se reconoce un rol especificado en el archivo CSV.
5. Correlacione los números de la columna de su archivo CSV con el IBMid correspondiente, rol y (opcional) entradas de fecha de caducidad.
6. Seleccione el separador de columna de coma o punto y coma adecuado para que coincida con el separador utilizado en el archivo `.csv`.
7. Pulse **Importar** para importar los IBMid y crear los miembros.

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

### Construir el archivo CSV
{: #constructing-your-csv}

Cuando se construye un archivo CSV para importar miembros a su organización, asegúrese de incluir la información necesaria para el método que está utilizando. Utilice comas o puntos y comas para separar columnas.  
**Importante:** al cargar el archivo CSV, en **Valores CSV**, asegúrese de que selecciona el separador de columna correcto.

Archivo CSV de ejemplo con delimitación por comas:  
```
user1@sample.com,PD_DEVELOPER_USER,2018-03-13
user2@sample.com,PD_OPERATOR_USER,2018-03-13
user3@sample.com,PD_ADMIN_USER,2018-03-13
```
Donde se utilizan las siguientes entradas de columna:  
- Columna 1: la dirección de correo electrónico del usuario.  
Si está añadiendo miembros, asegúrese de utilizar la dirección de correo electrónico correspondiente a un IBMid válido. Si está invitando a miembros, puede utilizar cualquier dirección de correo electrónico válida.
- Columna 2: el rol que se asignará al usuario.  
Especifique uno de los roles siguientes:
 - PD_ADMIN_USER
 - PD_OPERATOR_USER
 - PD_DEVELOPER_USER
 - PD_ANALYST_USER
 - PD_READER_USER  
 Para obtener más información sobre los roles de usuario, consulte [Roles de usuario, aplicación y pasarela](roles_index.html#user_roles).
- Columna 3: la fecha ISO 6801 (por ejemplo, `2018-03-13`) correspondiente a la fecha de caducidad del usuario.

## Edición de usuarios
{: #editing-users}

Los usuarios se pueden editar para modificar su rol, añadir, eliminar o cambiar una fecha de caducidad de acceso o añadir o eliminar el acceso a la organización.

1. Desde el panel de control de {{site.data.keyword.iot_short_notm}}, pulse **Miembros** en la barra de navegación en la izquierda.
2. Pulse el icono **Editar**, situado junto al usuario que desea editar.
3. Realice los cambios que desee en el usuario.
4. Pulse **Guardar**.

## Bloqueo del de acceso de usuarios
{: #blocking-users}

Se puede bloquear el acceso de los usuarios a la organización {{site.data.keyword.iot_short_notm}} mientras siguen manteniendo la pertenencia a la organización.

1. Desde el panel de control de {{site.data.keyword.iot_short_notm}}, pulse **Miembros** en la barra de navegación en la izquierda.
2. Pulse el conmutar situado junto al usuario cuyo acceso a la organización {{site.data.keyword.iot_short_notm}} desea bloquear.

## Limitación del acceso de usuarios
{: #limiting-users}

El control de acceso a nivel de recurso le permite limitar el acceso a dispositivos en una organización. Puede utilizar grupos de recursos para especificar los dispositivos que puede gestionar cada usuario o clave de API. Para obtener información acerca de cómo configurar el control de acceso a nivel de recurso, consulte [Configuración del control de acceso a nivel de recurso](reference/rlac.html#configure_RLAC).

## Eliminación de usuarios
{: #removing-users}

Los usuarios se pueden eliminar por completo de la organización {{site.data.keyword.iot_short_notm}}. La eliminación de usuarios no se puede deshacer y los usuarios se deben [añadir a la plataforma](#adding-new-users) para restaurar el acceso.

1. Desde el panel de control de {{site.data.keyword.iot_short_notm}}, pulse **Miembros** en la barra de navegación en la izquierda.
2. Pulse el icono **Suprimir**, situado junto al usuario que se va a eliminar.
3. Pulse **Suprimir** en el diálogo de confirmación.
