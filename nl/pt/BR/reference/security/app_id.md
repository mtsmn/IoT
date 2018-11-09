---

copyright:
years: 2018
lastupdated: "2018-08-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Autenticação do App ID para o Watson IoT Platform (beta)
{: #app_id}

O App ID é usado para autenticar usuários que precisam de acesso a aplicativos hospedados no IBM Cloud. Esses usuários acessam o serviço por meio de aplicativos móveis ou da web fornecidos pelo serviço.
{: shortdesc}

**Importante:** a Autenticação e a Autorização do App ID para o recurso {{site.data.keyword.iot_short_notm}} estão disponíveis somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

O {{site.data.keyword.iot_short_notm}} também suporta autenticar usuários por meio do Cloud IAM. O Cloud IAM é construído no IBM Cloud e é usado para autenticar e autorizar usuários administrativos e desenvolvedores que precisam configurar e gerenciar seus serviços IBM. Para obter mais informações sobre o Cloud IAM, veja [Autenticação e autorização do Cloud IAM para o Watson IoT Platform](cloud_iam.html#cloud_iam).

Os usuários do App ID geralmente não executam atividades administrativas ou de desenvolvimento em um serviço de nuvem e não podem efetuar login no painel da web do {{site.data.keyword.iot_short_notm}}. Somente usuários do Cloud IAM podem efetuar login no painel.

As APIs do {{site.data.keyword.iot_short_notm}} suportam a autenticação de usuários por meio do serviço IBM Cloud App ID. É possível configurar sua organização do {{site.data.keyword.iot_short_notm}} para usar uma instância do serviço App ID e, em seguida, incluir seus usuários do App ID em sua organização. Seu aplicativo autentica esses usuários com o App ID que pode, então, ser usado para chamar as APIs do {{site.data.keyword.iot_short_notm}}. Para obter mais informações sobre o serviço IBM Cloud App ID, veja [Tutorial de introdução do IBM Cloud App ID ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}.

O suporte atual do App ID é para APIs de REST que não são do sistema de mensagens, incluindo APIs para configurar e gerenciar dispositivos, usuários e outros recursos do {{site.data.keyword.iot_short_notm}}. O App ID não pode ser usado para publicar e assinar mensagens.

Geralmente, você usa o App ID para autenticar APIs de REST que são chamadas em nome de um usuário não administrativo. Um desenvolvedor pode criar um aplicativo que usa um token do App ID para chamar uma API de REST em nome de um usuário registrado, como um técnico de manutenção, que não tem acesso administrativo. A vantagem de usar o App ID para autenticar usuários não administrativos em vez de usar um único token de autenticação é que a trilha de auditoria no {{site.data.keyword.iot_short_notm}} mostra o usuário que chamou as APIs de REST em vez de um token de autenticação compartilhada. O App ID também é útil para autenticar não administradores que precisam usar a interface da linha de comandos (CLI) do {{site.data.keyword.iot_short_notm}}.

**Importante:** a chamada de APIs de REST do {{site.data.keyword.iot_short_notm}} deve ser feita usando um aplicativo de usuário e não diretamente de um navegador ou apps móveis. O aplicativo de usuário pode fornecer um nível adicional de controle de acesso para determinar quais funções de dispositivo um usuário específico tem permissão para acessar, bem como validar as mensagens do MQTT que são enviadas para os dispositivos. A imagem a seguir mostra esse fluxo de chamada:

![Chamada da API de REST usando um aplicativo de usuário](images/app_id_user_app.PNG)

Os usuários do App ID devem ser registrados no {{site.data.keyword.iot_short_notm}} usando o mesmo processo que é usado pelos usuários do Cloud IAM e eles aparecem na interface do painel.

Como os usuários do App ID normalmente não são administradores, é importante considerar quais recursos e comandos eles devem ter permissão para acessar e, em seguida, definir as configurações de controle de acesso de nível de recursos do {{site.data.keyword.iot_short_notm}} apropriadamente. Na maioria dos casos, os usuários do App ID são designados a uma função customizada que é mais restritiva do que as funções padrão definidas de uma organização do {{site.data.keyword.iot_short_notm}}. Para obter mais informações sobre funções, veja [Funções de usuário, aplicativo e gateway](../../roles_index.html#user-application-and-gateway-roles).

Por padrão, os usuários do App ID podem acessar todos os dispositivos. Se um usuário do App ID conseguir gerenciar somente uma lista restrita de dispositivos, designe grupos de recursos aos usuários do App ID.

**Nota:** o {{site.data.keyword.iot_short_notm}} limita o número total de usuários registrados para 1.000.

## Configurando um serviço App ID
{: #set_up_app_id}

Antes de configurar um serviço App ID, deve-se incluir uma instância do serviço IBM Cloud App ID e configurar provedores de identidade. Se você ainda não tiver uma instância do App ID, será possível provisionar uma por meio do [Catálogo do IBM Cloud Services ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog){: new_window}.

Depois de configurar o serviço App ID, deve-se obter as credenciais de serviço para configurar o App ID como um provedor para a sua organização do {{site.data.keyword.iot_short_notm}}. É possível obter ou criar credenciais de serviço na UI do serviço App ID.

1. No painel do IBM Cloud, selecione seu serviço App ID e clique em **Credenciais de serviço**.
2. Clique em **Visualizar credenciais** para localizar o `tenantId`, `clientId` e `secret` nas credenciais. Esses valores são necessários para configurar o App ID em etapas posteriores. O emissor será o nome do host de `oauthServerUrl`, por exemplo, `appid-oauth.ng.bluemix.net`.

A imagem a seguir mostra um exemplo de como recuperar as credenciais:

![Recuperando credenciais do App ID](images/app_id_credentials.PNG "Exemplo de recuperação de credenciais do App")


## Configurando o ID do App no  {{site.data.keyword.iot_short_notm}}
{: #config_app_id}

Deve-se configurar o App ID antes de poder usar o token do App ID para autorização. Para criar a configuração do App ID, use a API `POST /api/v0002/authentication/providers`, em que `tenant_Id` e `issuer` são obtidos da etapa 2 em [Configurando um serviço App ID](#set_up_app_id):

Corpo da Solicitação

```
{
	"appIdConfigName": “TestAppIdConfigName", 	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”, 	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Resposta do Pedido: 200

```
{
	"appIdConfigName": “TestAppIdConfigName", 	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”, 	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## Incluindo Usuários no  {{site.data.keyword.iot_short_notm}}
{: #users_app_id}

Deve-se incluir usuários e conceder autorizações a eles com base em suas funções. Para criar usuários, use a API `POST /api/v0002/authorization/users` com os detalhes a seguir:

 - `issuer` é obtido da etapa 2 em [Configurando um serviço App ID](#set_up_app_id).
 - `uniqueSecurityName` é preenchido com os detalhes de `appIdConfigName`, `amr` e o e-mail do usuário que são configurados nos provedores de identidade do serviço App ID.
 - `appIdConfigName` é o nome usado em [Configurando o App ID](##config_app_id), por exemplo “TestAppIdConfigName”.
 - `amr` é a referência de método de autenticação, que é o provedor de identidade usado no serviço App ID. O App ID suporta os provedores de Identidade a seguir:

	 - Cloud Directory
	 - Federação SAML 2.0
	 - Facebook
	 - Google

	 Com base no provedor de identidade que é usado no serviço App ID, `amr` seria `cloud_directory`, `saml`, `facebook` ou `google`.

No exemplo a seguir, o Cloud Directory está sendo usado:

Corpo da Solicitação

```
{
    "uniqueSecurityName": "TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
    "PD_ADMIN_USER"
  ]
}
```

Resposta do Pedido: 200

```
{
    "uniqueSecurityName": “TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
        "PD_ADMIN_USER"
    ]
}
```

## Gerando um token de ID de APP
{: #token_app_id}

Depois de configurar o App ID e os usuários incluídos, seu aplicativo poderá começar a gerar tokens do App ID para seus usuários e usá-los para chamar APIs de plataforma. Para obter mais informações sobre a introdução ao App ID, veja o vídeo a seguir: [IBM Bluemix App ID ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}.

Um usuário pode usar o terminal do App ID para efetuar login e recuperar seu token do App ID postando os exemplos a seguir.

### Exemplo de HTTP de geração de um token do App ID

Para gerar o token do App ID, use a API a seguir autenticando para o IBM Cloud com as credenciais de serviço:

` POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token `

Em que `tenantId`, `issuer`, `clientId` e `secret` são obtidos na etapa 2 em [Configurando um serviço App ID](#set_up_app_id).

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

Corpo da Solicitação para grant_type = password:

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

Corpo da Solicitação para grant_type= authorization_code:

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

Resposta do Pedido:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…", "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ", "token_type": "Bearer", "expires_in": 3600
}
```

### Exemplo de Curl de geração de um token do App ID

O fragmento a seguir mostra um exemplo de CURL de geração de um token do App ID:

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

Em que `username` e `password` são definidos para o usuário no App ID.

Resposta do Pedido:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…", "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ", "token_type": "Bearer", "expires_in": 3600
}
```

### Exemplo do Swagger de geração de um token do App ID

Para usar o Swagger para gerar um token do App ID, use o [Exemplo do Swagger ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}.

Conclua todos os detalhes nos parâmetros do Swagger e clique em **Experimentar** no término do método `/oauth/v3/{tenantId}/token`.

A resposta do terminal do token contém os tokens de acesso e de ID, o token de atualização opcional e as informações de expiração.

## Usando o token do App ID
{: #use_app_id}

Após o token do App ID ser criado, seu aplicativo pode começar a usá-lo para chamar APIs de plataforma. O token do App ID é a propriedade `id_token` na resposta em que ela foi gerada em [Gerando um token do App ID](#token_app_id). Quando o token do App ID expira, seu aplicativo deve gerar um novo token para continuar a chamar as APIs de plataforma.

Os exemplos a seguir mostram como usar o token do App ID ao chamar APIs.

###	Exemplo de HTTP de uso de um token do App ID

` GET https://org.domain/api/v0002/bulk/devices `

Parâmetros de Entrada  |	Valores
----------------- | -----------
Cabeçalhos	|	Content-Type: application / json<br>Autorização: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc ...

### Exemplo de Curl de uso de um token do App ID

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
