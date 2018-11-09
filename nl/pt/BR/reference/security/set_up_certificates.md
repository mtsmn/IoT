---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Configurando Certificados
{: #set_up_certificates}

Os certificados são usados para autenticação de dispositivo ou para substituir o certificado de servidor padrão do {{site.data.keyword.iot_full}} para o sistema de mensagens MQTT. Todos os dispositivos que não têm certificados assinados válidos têm acesso negado e não podem se comunicar com o servidor.

Para configurar certificados e acesso ao servidor para dispositivos, o operador do sistema registra os certificados de autoridade de certificação (CA) associados e, como opção, registra os certificados do servidor de mensagens na plataforma {{site.data.keyword.iot_short_notm}}.

Para obter informações sobre como usar as APIs para gerenciar certificados de CA e certificados do servidor de sistema de mensagens, consulte [APIs de autenticação e autorização do IBM Watson IoT Platform ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}.

## Certificados de autoridade de certificação
Os certificados de autoridade de certificação permitem que a organização reconheça os certificados de cliente em dispositivos como confiáveis para que os dispositivos possam se conectar ao servidor.

Se você incluir um certificado de CA ou substituir o certificado do servidor de sistema de mensagens, todos os dispositivos deverão se conectar usando um cliente MQTT que suporte a Server Name Indication (SNI) para que o servidor possa usar as CAs apropriadas para autenticar o dispositivo.

É possível configurar o nível de segurança de conexão, configurando a política de segurança de conexão. Para obter mais informações sobre como fazer isso, veja [Configurando políticas de segurança](set_up_policies.html).

## Certificados de cliente

Os certificados individuais de cliente (dispositivo ou gateway) permanecem nos dispositivos e não são transferidos por upload para a plataforma. O certificado de assinatura de CA usado para assinar todos os certificados de dispositivo e gateway é o único certificado que você transfere por upload para a plataforma. Se você está usando certificados autoassinados do servidor de sistema de mensagens, deve-se fazer upload dos certificados raiz e intermediários usados para assinar o certificado de cliente (cert.pem).

O certificado de dispositivo ou gateway individual que você assina com o certificado de CA deve ter o ID do dispositivo ou ID do gateway inserido como Nome Comum (CN) ou SubjectAltName no certificado.

Para dispositivos, o formato de campo **CN** é `CN=d:devtype:devid` e o formato de campo **SubjectAltName** é `SubjectAltName=email:d:*devtype:devid*`, em que `email:d` é constante e `*devtype*` é o Tipo de dispositivo do dispositivo e `*devid*` é o ID do dispositivo na plataforma.

Para gateways, o formato de campo **CN** é `CN=g:typeId:deviceId` e o formato de campo **SubjectAltName** é SubjectAltName=email:g:*devtype:devid*

Nota: não inclua o `orgId` nos campos **CN** ou **SubjectAltName** para certificados de dispositivo ou gateway. O `orgId` deve ser fornecido como parte das informações de SNI que são fornecidas pela implementação do cliente ao se conectar ao servidor de sistema de mensagens.

Para obter mais informações sobre certificados de cliente, consulte [a receita Conectar o Raspberry Pi ao IBM Watson IoT Platform usando certificados do lado do cliente ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}

## Certificados do servidor de mensagens

Os certificados do servidor de mensagens aceitam o domínio padrão, internetofthings.ibmcloud.com. O formato a seguir deve ser seguido para o CN ou SubjectAltName do certificado:

- `orgId.messaging.internetofthings.ibmcloud.com` (domínio IoTP)

O exemplo a seguir mostra uma CN válido para o certificado do servidor:

`mtxpd0.messaging.internetofthings.ibmcloud.com`

Para obter mais informações sobre certificados do servidor de sistema de mensagens, consulte [a receita Conectar o Raspberry Pi ao IBM Watson IoT Platform usando certificado de servidor autoassinado ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}

### Domínios customizados
{: #custom-domains}

Os certificados do servidor de mensagens aceitam domínios customizados. O formato a seguir deve ser seguido para o CN ou SubjectAltName do certificado:

- `orgId.messaging.<custom domain>`

O campo **CN** aceita caracteres curingas para domínios customizados, conforme mostrado no exemplo a seguir:

- `CN=*.messaging.mywiotpcustomdomain.com`

**Importante**: para domínios customizados, um serviço DNS externo é necessário para resolver o domínio customizado para o servidor de sistema de mensagens {{site.data.keyword.iot_full}}. Esse serviço DNS não é fornecido pela plataforma.

## Registrando certificados de Autoridade de Certificação (CA) para autenticação de dispositivo
{: #reg_ca_cert}

1. Efetue login no {{site.data.keyword.iot_short_notm}} e navegue para **Configurações gerais**.
2. Na seção **Segurança**, em **Certificados de autoridade de certificação**, clique em **Incluir certificado**.
3. Navegue para selecionar um arquivo de certificado para fazer upload ou arrastar um arquivo para a janela **Incluir certificado**. O arquivo pode ter apenas um certificado dentro dele e as datas do certificado devem ser válidas. Somente certificados no formato .pem ou .der são aceitos. É possível visualizar as informações de certificado dentro do arquivo selecionado.
4. Insira uma descrição para o arquivo de certificado.
5. Confirme se o arquivo correto está selecionado e clique em **Salvar**. O certificado selecionado está listado na tabela e está ativo por padrão.

## Registrando certificados do servidor de sistema de mensagens
{: #reg_msg_cert}

Um certificado de servidor padrão é fornecido com a plataforma. É possível usar o certificado padrão ou fazer upload de um de sua organização. Se você ainda não tiver um certificado para usar, poderá criar uma solicitação para um novo certificado. Depois de receber o novo certificado, deve-se assiná-lo e, em seguida, fazer upload dele de volta para a plataforma.

**Nota:** as páginas do painel da plataforma podem fazer conexões internas com o servidor de sistema de mensagens para recuperar informações do dispositivo. Ao configurar certificados de servidor autoassinados para uma organização, os usuários do painel devem incluir o certificado do servidor como um certificado confiável em seus navegadores para evitar problemas de conexão porque os navegadores, por padrão, não reconhecerão o servidor de sistema de mensagens como um servidor confiável.

## Fazendo upload de um certificado do servidor de sistema de mensagens de sua organização
{: #upload_cert}
1. Na seção **Segurança** de **Configurações gerais**, em **Certificados do servidor de sistema de mensagens**, clique em **Incluir certificado**.
2. Navegue para selecionar um arquivo de certificado para fazer upload ou arrastar um arquivo para a janela **Incluir certificado**.
3. Navegue para selecionar um arquivo de chave privado para fazer upload ou arrastar um arquivo para a janela **Incluir certificado**.
4. Insira a passphrase da chave privada, se a chave privada tiver sido criptografada com uma passphrase.
5. Confirme se o arquivo correto está selecionado e clique em **Salvar**.

## Ativando um certificado do servidor de sistema de mensagens

Para ativar o certificado padrão ou outro certificado que já tenha sido transferido por upload, selecione o certificado que você deseja usar na lista suspensa **Certificado do servidor de sistema de mensagens padrão** na tabela em **Certificados do servidor de sistema de mensagens**. O certificado selecionado é listado na tabela como o certificado ativo.

## Solicitando um novo certificado do servidor de sistema de mensagens
{: #request_cert}

Se você desejar usar um novo certificado de servidor de sistema de mensagens, será possível gerar uma Certificate Signing Request (CSR) para solicitar um novo certificado.

1. Na seção **Segurança** de **Configurações gerais**, em **Certificados do servidor de sistema de mensagens**, clique em **Gerar CSR**.
2. Insira os detalhes para solicitar uma CSR para seu servidor e clique em **Gerar**. A CSR é exibida na tabela.
3. Faça download da solicitação e envie-a para uma autoridade de certificação para assinatura.
4. Depois de obter um certificado, retorne para a entrada CSR na tabela e faça upload do novo certificado. Depois que o certificado for transferido por upload, a CSR na tabela será substituída pelo certificado transferido por upload.
