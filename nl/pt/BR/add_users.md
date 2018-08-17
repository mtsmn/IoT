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

# Gerenciando o acesso de usuário
{: #managing-user-access}

No painel de membros, é possível controlar e gerenciar o acesso à sua organização do {{site.data.keyword.iot_full}}. É possível incluir usuários incluindo, convidando<!--, registering--> ou importando-os. Também é possível fornecer diferentes níveis de acesso a seus usuários, designando funções.
{:shortdesc}

## Adicionando usuários
{: #adding-new-users}

Na guia **Membros** do painel, é possível incluir membros individuais usando a função Incluir. Também é possível incluir membros simultaneamente
usando a função Importar.

Por padrão, contas dos membros não expiram. Ao incluir usuários à sua organização do {{site.data.keyword.iot_short_notm}}, é possível configurar opcionalmente uma data de validade para suas contas.

### Incluindo membros em sua organização do {{site.data.keyword.iot_short_notm}}

Para incluir um membro em sua organização, você precisará do IBMid do membro. Para incluir um membro, você não precisa de confirmação do membro.

Para incluir um único membro em sua organização do {{site.data.keyword.iot_short_notm}}:
1. No painel do {{site.data.keyword.iot_short_notm}}, acesse **Membros**.
2. Clique em **Incluir membros** e selecione a guia **Incluir**.
3. Insira o IBMid do membro.
4. Selecione uma função para o membro.
5. Opcional: configure uma data de validade para o membro.
6. Clique em **Incluir**.

Para incluir vários membros simultaneamente, deve-se fazer upload de um arquivo `.csv` que contém o IBMid, a função e a data de expiração opcional de cada membro. Para obter informações, veja [Construindo seu arquivo CSV](#constructing-your-csv).
1. No painel do {{site.data.keyword.iot_short_notm}}, acesse **Membros**.
2. Clique em **Incluir membros** e selecione a guia **Importar**.
3. Procure seus arquivos ou arraste o arquivo `.csv` para a janela **Fazer upload do CSV**.
4. Selecione uma função padrão para usar se uma função especificada no arquivo CSV não for reconhecida.
5. Mapeie os números de coluna no arquivo CSV para as entradas de IBMid, de função e de data de validade (opcional) correspondentes.
6. Selecione o separador de colunas de vírgula ou ponto-e-vírgula apropriado para corresponder ao separador usado no arquivo `.csv`.
7. Clique em **Importar** para importar os IBMids e criar os membros.

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

### Construindo seu arquivo CSV
{: #constructing-your-csv}

Ao construir um arquivo CSV para importar membros para sua organização, assegure que sejam incluídas as informações necessárias para o método que você está usando. Use vírgulas ou pontos e vírgulas para separar colunas.  
**Importante:** ao fazer upload do arquivo CSV, em **Configurações de CSV**, assegure-se de selecionar o separador de colunas correto.

Arquivo de CSV de amostra com delimitação por vírgula:  
```
user1@sample.com,PD_DEVELOPER_USER,2018-03-13
user2@sample.com,PD_OPERATOR_USER,2018-03-13
user3@sample.com,PD_ADMIN_USER,2018-03-13
```
Em que as entradas de coluna a seguir são usadas:  
- Coluna 1: o endereço de e-mail do usuário.  
Se você estiver incluindo membros, certifique-se de usar o endereço de e-mail correspondente a um IBMid válido. Se estiver convidando membros, será possível usar qualquer endereço de e-mail válido.
- Coluna 2: a função a ser designada ao usuário.  
Insira uma das funções a seguir:
 - PD_ADMIN_USER
 - PD_OPERATOR_USER
 - PD_DEVELOPER_USER
 - PD_ANALYST_USER
 - PD_READER_USER  
 Para obter mais informações sobre as funções de usuário, veja [Funções de usuário, aplicativo e gateway](roles_index.html#user_roles).
- Coluna 3: a data ISO 6801 (por exemplo, `2018-03-13`) que corresponde
à data de expiração do usuário.

## Editando usuários
{: #editing-users}

Os usuários podem ser editados para mudar sua função, incluir, remover ou mudar uma data de expiração de acesso ou incluir ou remover o acesso à organização.

1. No painel do {{site.data.keyword.iot_short_notm}}, clique em **Membros** na barra de navegação à esquerda.
2. Clique no ícone **Editar** próximo ao usuário que você deseja editar.
3. Faça as mudanças desejadas no usuário.
4. Clique em **Salvar**.

## Bloqueando o acesso de usuário
{: #blocking-users}

Os usuários podem ser bloqueados contra o acesso à organização do {{site.data.keyword.iot_short_notm}}, enquanto ainda mantêm a associação à organização.

1. No painel do {{site.data.keyword.iot_short_notm}}, clique em **Membros** na barra de navegação à esquerda.
2. Clique na alternância próxima ao usuário que você deseja bloquear contra o acesso à organização do {{site.data.keyword.iot_short_notm}}.

## Limitando o acesso de usuário
{: #limiting-users}

O controle de acesso no nível de recursos permite limitar o acesso a dispositivos
dentro de uma organização. É possível usar grupos de recursos para especificar os
dispositivos que cada usuário ou chave API pode gerenciar. Para obter informações sobre
como configurar o controle de acesso no nível de recursos, consulte
[Configurando o controle de acesso no
nível recursos](reference/rlac.html#configure_RLAC).

## Removendo usuários
{: #removing-users}

Os usuários podem ser removidos completamente da organização do {{site.data.keyword.iot_short_notm}}. A remoção dos usuários não pode ser desfeita e os usuários devem ser [incluídos na plataforma](#adding-new-users) para restaurar o acesso.

1. No painel do {{site.data.keyword.iot_short_notm}}, clique em **Membros** na barra de navegação à esquerda.
2. Clique no ícone **Excluir** próximo ao usuário a ser removido.
3. Clique em **Excluir** no diálogo de confirmação.
