---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-19"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando e configurando um serviço historiador usando um {{site.data.keyword.cloudant_short_notm}}  
{: #cloudant_main}

Conectar um serviço do {{site.data.keyword.cloudantfull}} ao seu {{site.data.keyword.iot_full}} permite armazenar e acessar os dados de seu dispositivo. Os dados do dispositivo são armazenados em bancos de dados diários, semanais ou mensais, dependendo do intervalo de seu depósito selecionado.

Ao iniciar o uso de um {{site.data.keyword.cloudant_short_notm}} para armazenar dados do dispositivo, três bancos de dados são automaticamente criados, um banco de dados é criado para o intervalo atual do depósito atual, uma para o próximo intervalo e um banco de dados de configuração. Documentos de design podem ser incluídos no banco de dados de configuração e serão copiados para os novos bancos de dados conforme forem criados. Quando o fim de um intervalo é atingido, os dados do dispositivo são armazenados no banco de dados do depósito para o novo intervalo e um novo banco de dados é criado para o próximo intervalo.

Quando os dados do dispositivo são enviados para um banco de dados, eles podem ser armazenados de duas maneiras diferentes. Se os dados forem JSON válido e o formato do evento de dispositivo estiver configurado para `JSON`, os dados do dispositivo serão armazenados no formato a seguir:

```json

{
  "_id": "78bf4380-3311-11e6-a747-d7b140d1a70a",
  "_rev": "2-d13912b7c089f060a4ba7369fa86e46f",
  "typeId": "t",
  "deviceType": "0",
  "eventType": "json_payload",
  "format": "json",
  "timestamp": "2016-06-15T16:54:41.464+01",
  "data": {
    "a": 22
  }
}

```

Se os dados do dispositivo não for um JSON válido ou se o formato não estiver configurado para `JSON`, os dados do dispositivo serão armazenados como uma sequência codificada base64 no campo `payload` no formato a seguir:

```json

{
  "_id": "80f1ce10-3311-11e6-a747-d7b140d1a70a",
  "_rev": "1-bfcbf1e74389fe4188a9425c0cd2575a",
  "payload": "eHh4eHg=",
  "typeId": "t",
  "deviceType": "0",
  "eventType": "non_json_payload",
  "format": "notjson",
  "timestamp": "2016-06-15T16:54:55.217+01"
}

```
A qualidade de serviço (QoS) que é usada por um dispositivo MQTT para enviar mensagens para o {{site.data.keyword.iot_short_notm}} não se aplica quando as mensagens são enviadas do {{site.data.keyword.iot_short_notm}} para o {{site.data.keyword.cloudant_short_notm}}. Geralmente, uma mensagem é enviada para o {{site.data.keyword.cloudant_short_notm}} uma vez. Raramente, pode ser possível que uma mensagem seja enviada mais de uma vez ou nem seja enviada. 

## Antes de iniciar  
{: #byb}

Antes de conectar um {{site.data.keyword.cloudant_short_notm}} a seu serviço do {{site.data.keyword.iot_short}}, conclua as tarefas a seguir:

- Configure um {{site.data.keyword.cloudant_short_notm}} no mesmo espaço do {{site.data.keyword.Bluemix_notm}} que seu {{site.data.keyword.iot_short_notm}} usando o Catálogo do {{site.data.keyword.Bluemix_notm}}.

Assegure-se de ter privilégios de desenvolvedor na organização do {{site.data.keyword.Bluemix_notm}} e de estar conectado por meio do {{site.data.keyword.Bluemix_notm}}. Se você não estiver conectado por meio do {{site.data.keyword.Bluemix_notm}} ou não tiver privilégios de desenvolvedor nessa organização do {{site.data.keyword.Bluemix_notm}}, não será possível autorizar a ligação do {{site.data.keyword.cloudant_short_notm}} e do {{site.data.keyword.iot_short_notm}}.

## Usando o painel do {{site.data.keyword.iot_short_notm}} para ligar um serviço {{site.data.keyword.cloudant_short_notm}} ao {{site.data.keyword.iot_short_notm}}

Conclua as etapas a seguir para conectar um {{site.data.keyword.cloudant_short_notm}}:

1. Em seu painel do {{site.data.keyword.iot_short}}, clique em **Extensões** na barra de navegação.
2. No quadro Armazenamento de dados históricos, clique em **Configurar**.
2. Todos os serviços disponíveis do {{site.data.keyword.cloudant_short_notm}} dentro do mesmo espaço do {{site.data.keyword.Bluemix_notm}} que o seu serviço do {{site.data.keyword.iot_short}} são
listados na seção Configurar armazenamento de dados históricos.
3. Selecione o serviço do {{site.data.keyword.cloudant_short_notm}} que você deseja conectar.
4. Selecione as opções de configuração de seu {{site.data.keyword.cloudant_short_notm}}:

  a. Selecione um intervalo de depósito. O intervalo do depósito controla como que frequência novos bancos de dados são criados para armazenar dados do dispositivo. Novos depósitos são criados à meia-noite no fuso horário selecionado usando o intervalo do depósito selecionado.

  b. Selecione um fuso horário. O horário no fuso horário selecionado será usado para determinar em qual depósito os dados do dispositivo devem ser colocados, não o horário local do dispositivo. Os registros de data e hora nos dados do dispositivo que estão sendo enviados ao {{site.data.keyword.cloudant_short_notm}} serão convertidos para o fuso horário selecionado ao se decidir em qual banco de dados os dados serão inseridos.

  c. Escolha opções que determinam o nome do banco de dados. O nome do banco de dados será `iotp_<orgID>_<dbname>_<bucket_name>` em que:

   * `<orgID>` é o ID da organização.
   * `<dbname>` é sua opção para essa parte do nome do banco de dados controlado pelo campo `Nome do banco de dados`.
   * `<bucket_name>` é uma sequência determinada pela sua opção para o campo `Intervalo de depósito`:
     * Para intervalos do depósito  ` day ` ,  `<bucket_name>` será `yyyy-mm-dd`.  Por exemplo, `2016-07-06` para eventos em 6 de julho de 2016.
     * Para intervalos do depósito  ` week `   `<bucket_name>` será `yyyy-'w'ww` em que `'w'ww` indica um número de semana.  Por exemplo, `2016-w03` para eventos na terceira semana de 2016.
     * Para intervalos do depósito  ` mês `   `<bucket_name>` será `yyyy-mm`.  Por exemplo, `2016-07` para eventos em julho de 2016.

5. Clique em **Autorizar**.
6. Clique em **Confirmar** na caixa de diálogo de autorização.

Os dados de seu dispositivo agora estão sendo armazenados em seu {{site.data.keyword.cloudant}}.

## Orientações sobre o uso do Serviço Historian  
{: #recipes}

As orientações a seguir descrevem como usar o {{site.data.keyword.cloudant_short_notm}} como o armazenamento de Historian para o {{site.data.keyword.iot_short}}:

- A orientação [Configurar o {{site.data.keyword.cloudant_short_notm}} como o Armazenamento de dados Historian para o {{site.data.keyword.iot_short}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/cloudant-nosql-db-as-historian-data-storage-for-ibm-watson-iot-parti/){: new_window} descreve como os dados do dispositivo são armazenados no {{site.data.keyword.cloudant_short_notm}} e demonstra como configurar e armazenar dados do dispositivo no {{site.data.keyword.cloudant_short_notm}} como Armazenamento de dados Historian.

- A orientação [Consultar e processar dados do dispositivo do {{site.data.keyword.iot_short}} por meio do {{site.data.keyword.cloudant_short_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/cloudant-nosql-db-as-historian-data-storage-for-ibm-watson-iot-partii){: new_window} mostra como consultar e executar operações de processamento de dados nos dados do dispositivo armazenados no {{site.data.keyword.cloudant_short_notm}}.

- A orientação [Visualizar dados do dispositivo Watson IoT armazenados no Cloudant NoSQL DB ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/?post_type=pnext_tutorial&p=27327){: new_window} mostra como vincular entre os Cartões de gráfico de linha e o Armazenamento de dados Historian para exibir dados do dispositivo no Painel do Watson IoT Platform.


## Criando novos documentos de design  
{: #design_docs}

Novos documentos de design estão contidos no banco de dados de configuração e são copiados para cada banco de dados criado. O nome do banco de dados de configuração é `iotp_<orgid>_<choice>_configuration
e usa os mesmos parâmetros que os nomes dos bancos de dados descritos na etapa 3b na seção Antes de iniciar.

Os documentos de design padrão contidos no {{site.data.keyword.iot_short_notm}} implementam consultas disponíveis no historiador atual, além da função de resumo.

Documentos de design adicionais podem ser incluídos no banco de dados de configuração e serão copiados para os novos bancos de dados de intervalo do depósito à medida que forem criados. Para incluir documentos de design no banco de dados de configuração, veja a [documentação da API do Cloudant ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.cloudant.com/document.html){: new_window}.

<!--  # Related links
{: #rellinks}
* [Querying your {{site.data.keyword.cloudant_short_notm}}](link) -->
