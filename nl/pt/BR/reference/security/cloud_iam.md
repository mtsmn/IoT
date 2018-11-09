---

copyright:
  years: 2018
lastupdated: "2018-07-16"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Autenticação e autorização do Cloud IAM para o {{site.data.keyword.iot_short_notm}} (beta)
{: #cloud_iam}

As APIs do {{site.data.keyword.iot_full}} suportam a autenticação e a autorização de usuários usando o Identity and Access Management (IAM).

**Importante:** a Autenticação e a Autorização do Cloud IAM para o recurso {{site.data.keyword.iot_short_notm}} estão disponíveis somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

O Cloud IAM é construído no IBM Cloud e é usado para autenticar e autorizar usuários administrativos e desenvolvedores que precisam configurar e gerenciar seus serviços IBM. Os usuários que precisam de acesso à UI do Watson IoT Platform são autenticados com o IBM Cloud IAM. A origem da identidade para o Cloud IAM pode ser um usuário registrado do IBMid ou pode ser um serviço de diretório do cliente que suporta SAML.  

O {{site.data.keyword.iot_short_notm}} também suporta autenticar usuários por meio do App ID. O ID do app é usado para autenticar usuários que precisam de acesso a aplicativos hospedados no IBM Cloud. Os usuários do App ID geralmente não executam atividades administrativas ou de desenvolvimento em um serviço de nuvem. Para obter mais informações, veja [Autenticação do App ID para o Watson IoT Platform](app_id.html#app_id).

O IAM permite que os usuários obtenham um token OAuth do IAM e o usem para autenticar chamadas API. Por exemplo, um usuário com a CLI do {{site.data.keyword.containershort_notm}} instalada pode usar o comando a seguir:

` bx iam oauth-tokens `

O comando retorna o token do IAM do usuário com login efetuado no formato a seguir:

`IAM Token: Bearer <some token>`

Um usuário também pode usar o terminal do token do IAM para efetuar login e recuperar seu token do IAM, conforme mostrado no exemplo a seguir:
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

Este exemplo de API usa uma chave API do IAM para solicitar o token do IAM e retorna o `Bearer <token>`.

Em seguida, quando um usuário chama uma API do {{site.data.keyword.iot_short_notm}}, ele pode fornecer o cabeçalho de autorização `Authorization: Bearer <token>`  em suas chamadas de API.

## Gerando um token do IAM
{: #iam_generate}

É possível escolher como automatizar a criação do token do IAM, dependendo de como você autentica com o IBM Cloud e o tipo de ID do IBM Cloud usado.

Se você usar um ID não federado, será possível escolher uma das opções a seguir para criar um token do IAM:
 - **Nome do usuário e senha do IBM Cloud:** é possível criar um token e automatizar totalmente a criação de seu token de acesso do IAM.
 - **Gerar uma chave API do IBM Cloud:** use as chaves API do IBM Cloud que são dependentes da conta do IBM Cloud para a qual elas são geradas. Não é possível combinar a chave API do IBM Cloud com um ID de conta diferente no mesmo token do IAM. Para acessar os clusters que foram criados com uma conta diferente daquela em que a chave API do IBM Cloud se baseia, deve-se efetuar login na conta para gerar uma nova chave API.

Se você usar um ID federado, será possível escolher uma das opções a seguir para criar um token do IAM:
 - **Gerar uma chave API do IBM Cloud:** as chaves API do IBM Cloud são dependentes da conta do IBM Cloud para a qual elas são geradas. Não é possível combinar a chave API do IBM Cloud com um ID de conta diferente no mesmo token do IAM. Para acessar os clusters que foram criados com uma conta diferente daquela em que a chave API do IBM Cloud se baseia, deve-se efetuar login na conta para gerar uma nova chave API.
 - **Usar uma senha descartável:** se você autenticar com o IBM Cloud usando uma senha descartável, não será possível automatizar totalmente a criação de seu token do IAM porque a recuperação de sua senha descartável requer uma interação manual com seu navegador da web. Para automatizar totalmente a criação de seu token do IAM, deve-se criar uma chave API do IBM Cloud em seu lugar.

Quando você cria seu token de acesso, as informações do corpo que são incluídas em sua solicitação variam com base no método de autenticação do IBM Cloud usado. Substitua os valores, da seguinte forma:
- `<my_username>`  = Seu nome de usuário do IBM Cloud.
- `<my_password>` = Sua senha do IBM Cloud.
- `<my_api_key>`  = Sua chave do IBM Cloud API.
- `<my_passcode>`  = Sua senha descartável do IBM Cloud. Execute `bx login --sso` e siga as instruções em sua saída da CLI para recuperar sua senha única usando seu navegador da web.

Exemplo:
`POST https://iam.<region>.bluemix.net/oidc/token`

Para especificar uma região do IBM Cloud, revise as abreviações de região conforme elas são usadas nos terminais de API. Veja [Terminais de API da região do IBM Cloud [Ícone de link externo](../../icons/launch-glyph.svg)](https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions) para obter mais informações.

Parâmetro de entrada	 | Valores
---------------- | -----------
Header	| Content-Type:application/x-www-form-urlencoded<br>Autorização: Básico Yng6Yng=<br>Nota: é fornecido a você o Yng6Yng=, a autorização codificada pela URL para o nome do usuário bx e a senha bx.
Body for IBM Cloud user name and password	|	grant_type: senha<br>response_type: cloud_iam, uaa<br>username:  `<my_username>`<br>senha:  `<my_password>`<br>uaa_client_id: cf<br>uaa_client_Segredo:<br>Nota: inclua a chave uaa_client_secret sem nenhum valor especificado.
Body for IBM Cloud API keys	|	grant_type: urn:ibm:params: oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey:  `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_Segredo:<br>Nota: inclua a chave uaa_client_secret sem nenhum valor especificado.
Body for IBM Cloud one-time passcode	|	grant_type: urn:ibm:params: oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>senha:  `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_Segredo:<br>Nota: inclua a chave uaa_client_secret sem nenhum valor especificado.

Saída de API de exemplo:

```
{
"access_token": "<iam_token>",
"refresh_token": "<iam_refresh_token>",
"uaa_token": "<uaa_token>",
"uaa_refresh_token": "<uaa_refresh_token>",
"token_type": "Bearer",
"expires_in": 3600,
"expiration": 1493747503
}
```
É possível localizar o token IAM no campo **access_token** de sua saída da API.

O exemplo a seguir mostra como usar o token do IAM ao chamar APIs.

```
GET https://org.domain/api/v0002/bulk/devices
```

Parâmetros de Entrada  |	Valores
----------------- | -----------
Cabeçalhos	|	Content-Type: application / json<br>Autorização: bearer  `<iam_token>`
