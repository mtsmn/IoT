---

copyright:
  years: 2017, 2018
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Analisando dados usando o Watson Analytics
{: #WA_integration}  

É possível usar o {{site.data.keyword.iot_full}} com o Watson Analytics (WA) para visualizar e aprender sobre os dados que são enviados de dispositivos que estão conectados à plataforma.
{: shortdesc}

## Visão geral e objetivos

Este guia orienta você, passo a passo, pelo processo de visualização de dados de evento de dispositivo do {{site.data.keyword.iot_short_notm}} usando o Watson Analytics (WA) como ferramenta de análise.

Os dados do dispositivo enviados para {{site.data.keyword.iot_short_notm}} podem ser coletados e armazenados no {{site.data.keyword.Bluemix}} usando o serviço {{site.data.keyword.cloudantfull}} NoSQL DB. Para coletar os dados, deve-se primeiro conectar a {{site.data.keyword.iot_short_notm}} ao serviço {{site.data.keyword.cloudant_short_notm}}. Após os dados serem coletados, exporte-os para um arquivo CSV. Faça o upload desse arquivo no WA em que você possa visualizar e analisar os dados do dispositivo. Os dados do dispositivo são armazenados nos bancos de dados do {{site.data.keyword.cloudant_short_notm}} de forma diária, semanal ou mensal, dependendo do intervalo de depósito que está configurado.

![Visão geral de como usar o WA para analisar dados](images/WA_overview.png)

Como parte deste guia, você aprenderá:

 - Como configurar o armazenamento de dados da plataforma para que o Cloudant NoSQL DB seja usado como o serviço de historiador.
 - Como usar o simulador Weather Sensors para gerar dados para serem usados pela plataforma.
 - Como exportar os dados e, em seguida, importá-los para o WA para analisá-los.


## Pré-requisito

Para concluir estas etapas deve-se ter acesso ao [{{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} com [Cloudant NoSQL DB ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")] (https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}e acesso ao [Watson Analytics ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson-analytics){: new_window}.


## Etapa 1. Configurar o simulador
{: #WA_sensor_data}


Para conduzir uma análise significativa, deve-se ter dados significativos. É
possível simular os dados do sensor real para aprender sobre como os dados do dispositivo
do Watson IoT Platform podem ser analisados usando o Wastson Analytic. Esta etapa fornece instruções para:
 - [Configurando o simulador com uma **instância existente de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Configurando o simulador com uma **nova instância de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)
 - [Fazendo download de um arquivo de amostra CSV
pré-criado com dados](#WA_sensor_premade), se você não desejar usar o simulador


### Configurando o simulador Weather Sensors com uma instância existente de {{site.data.keyword.iot_short_notm}}
{: #sim_existing_platform}

Para simular eventos reais de dados do sensor com relação às suas organizações usando o simulador Weather Sensors, deve-se primeiro configurar o simulador. Estas etapas presumem que você já tem uma instância de {{site.data.keyword.iot_short_notm}} funcionando.

1. [Gere a apikey e o token que são necessários para executar o simulador. ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Implemente o app da web de simulador Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} e siga as instruções detalhadas.

   Para obter mais informações sobre o Weather Sensors, veja [o guia do simulador Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Continue com a [Etapa 2. Configurar o conector do banco de dados](#WA_config_db).


### Configurando o simulador Weather Sensors com uma nova instância de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Para simular eventos reais de dados do sensor com relação às suas organizações usando o simulador Weather Sensors, deve-se primeiro configurar o simulador. Estas etapas incluem as instruções para criar uma instância de {{site.data.keyword.iot_short_notm}} juntamente com o simulador.

1. [Implemente o app da web de simulador Weather Sensors com uma instância de {{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} e siga as etapas detalhadas.

   Para obter mais informações sobre o Weather Sensors, veja [o guia do simulador Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Aguarde até que a implementação seja concluída e, em seguida, navegue para o painel do IBM Cloud.
3. Ative o serviço "wiotp-for-weather-sensors-simulator" de {{site.data.keyword.iot_short_notm}} que foi criado pelo processo de implementação.
4. Continue com a [Etapa 2. Configurar o conector do banco de dados](#WA_config_db).


### Usando dados do sensor de um arquivo CSV de amostra pré-criado
{: #WA_sensor_premade}

Para simular eventos de dados do sensor real com relação a sua organização usando um arquivo CSV pré-criado:

1. [Faça download do arquivo CSV do Cloudant
![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator/releases/download/v1.0/cloudant.csv){: new_window}.
2. Continue com a [Etapa 5. Configurar o WA e visualizar os dados](#WA_import_data).


## Etapa 2. Configurar o conector do banco de dados
{: #WA_config_db}

Para usar o {{site.data.keyword.cloudant_short_notm}} com o Watson Analytics, deve-se
configurar o armazenamento de dados da plataforma para que o Cloudant NoSQL DB seja usado
como serviço historiador.

1. No painel do {{site.data.keyword.cloudant_short_notm}}, clique em **Extensões** na barra de navegação.
2. Em **Armazenamento de dados históricos**, clique em **Configuração**. A seção **Configurar armazenamento de dados históricos** lista todos os serviços do Cloudant NoSQL DB que estão disponíveis no mesmo espaço do IBM Cloud que o {{site.data.keyword.cloudant_short_notm}}.
3. Selecione o serviço Cloudant NoSQL DB que você deseja conectar.
4. Especifique as opções de configuração do Cloudant NoSQL DB a seguir:
  - Intervalo de depósito = dia
  - Fuso horário = UTC
  - Nome do banco de dados = padrão
5. Clique em **Pronto** e confirme a autorização para a conexão com o serviço Cloudant. Assegure-se de que os pop-ups estejam ativados em seu navegador para ter acesso à janela de confirmação. Quando
você tiver configurado com êxito o Cloudant NoSQL DB, o status de Armazenamento de
dados históricos será mudado para configurado e os dados do dispositivo serão armazenados
no {{site.data.keyword.cloudant_short_notm}} NoSQL DB.
6. Continue com a [Etapa 3. Executar o simulador](#run_simulator).


## Etapa 3. Executar o simulador
{: #run_simulator}

O simulador publica dados reais dos sensores de clima, de 17 estações meteorológicas localizadas na área de Haifa, em sua organização de {{site.data.keyword.iot_short_notm}}.

1. Navegue para o simulador.
2. Insira os detalhes a seguir:
   - Organização do Watson IoT Platform
   - Chave API
   - token de autenticação

3. Clique em **Executar simulador**. Os dados levarão alguns minutos para serem gerados.
4. Acesse a plataforma Watson IoT enquanto o simulador está sendo executado e verifique se os dispositivos foram criados e se os eventos estão vindo para esses dispositivos. 
5. Prossiga para a [Etapa 4. Exportar o banco de dados do Cloudant](#WA_export_csv).


## Etapa 4. Exportar o banco de dados do Cloudant
{: #WA_export_csv}

Ao configurar um {{site.data.keyword.cloudant_short_notm}} NoSQL DB para
armazenar dados do dispositivo, três bancos de dados são automaticamente criados pelo
conector. Um banco de dados é criado para o intervalo de depósito atual, um para o próximo intervalo e um para o banco de dados de configuração. Quando o término do intervalo é atingido, os dados do dispositivo são armazenados no banco de dados do depósito para o novo intervalo e um novo banco de dados é criado para o depósito subsequente.

O recurso de extensão de Armazenamento de dados históricos no
{{site.data.keyword.cloudant_short_notm}} cria um documento de design no Cloudant
chamado "iotp". Esse documento tem uma função "lista" chamada "csv" que pode ser usada
para exportar eventos de dispositivo, armazenados como documentos no Cloudant, para o
formato CSV. Apenas eventos no formato JSON são enviados para o arquivo CSV. Esse
documento de design é propagado automaticamente para cada novo banco de dados nos
intervalos de depósito futuro.

O arquivo CSV contém informações sobre os metadados de evento de dispositivo e sua
carga útil. A lista a seguir mostra exemplos de metadados de eventos:
 -	DeviceId
 -	DeviceType
 - 	EventType
 - 	Registro de data e hora no formato ISO 8601

A função lista csv divide o registro de data e hora original em dois novos
campos de Hora e Data separados. Além de metadados, a função lista CSV inclui os
atributos de dados da carga útil dispositivo. Essa carga útil é exibida no documento do
Cloudant sob a chave "dados". Os documentos que são gerados pelo simulador Weather
Sensors têm uma estrutura semelhante ao exemplo a seguir:

```
{"deviceType": "WS",
  "deviceId": "Old-Market",
  "eventType": "sensorData",
  "format": "json",
  "timestamp": "2017-08-09T16:28:14.666Z",
  "data": { "NO2": 3.2, … }}
```

No arquivo CSV resultante, todos os atributos de carga útil são representados como colunas e são prefixados com:

```
<deviceType>_<eventType>_  
```

No exemplo acima, uma coluna denominada WS_sensorData_NO2 é incluída no arquivo CSV.

Para exportar o banco de dados do Cloudant em formato CSV:  

1. Efetue login no Cloudant NoSQL DB.
2. Selecione um banco de dados a ser exportado.
3. Abra o banco de dados selecionado.
4. Abra uma nova guia no navegador e digite a URL a seguir:
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-date?include_docs=true
```
   O ID de serviço do Cloudant e dbName devem ser mudados de acordo com seu ID de serviço do Cloudant e o nome do banco de dados selecionado. O ID de serviço cloudant pode ser copiado da URL do painel de gerenciamento do Cloudant.

   **Exemplo:**
   ```
   https://ccf73725-b617-4f3e-8a7e-f5fb09569af4-bluemix.cloudant.com/iotp_115ccv_default_2017-08-23/_design/iotp/_list/csv/by-date?include_docs=true
   ```

   Neste exemplo, os dados serão classificados por registro de data e hora, visto que a
visualização por data é utilizada para chamar a função lista. É possível também filtrar
os dados usando o recurso de filtro nativo de visualizações do Cloudant, mudando a
visualização que é usada na URL e aplicando os atributos startkey e endkey.

   **Exemplo:**
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-deviceType?include_docs=true&startkey='WS'&endkey='WS'
   ```
   Neste exemplo, a visualização deviceType é usada para gerar o csv e somente documentos
com deviceType=WS são incluídos no arquivo transferido por download. Para selecionar
documentos dentro de um intervalo de tempo específico, use a visualização por data e use
a URL de consulta a seguir (substituindo os registros de data e hora para o intervalo desejado):
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-date?statkey="2017-08-29T12:25:50.995Z"&endkey="2017-08-29T12:25:51.514Z"
   ```
5. Forneça as credenciais do Cloudant se necessário e faça download do arquivo CSV. O
nome do arquivo é gerado de acordo com a visualização definida na URL. Por exemplo, o
nome do arquivo pode ser by-date.csv ou by-deviceType.csv.
6. Continue com a [Etapa 5. Configurar o WA e visualizar os dados](#WA_import_data).


## Etapa 5. Configurar o WA e visualizar dados
{: #WA_import_data}

Para configurar o WA e começar a visualizar dados:

1. Efetue login no WA em: https://watson.analytics.ibmcloud.com.
2. Na página inicial do WA, selecione **Dados**.
3. Clique em **Arquivo local** para importar o arquivo CSV local. O
nome do arquivo CSV depende da visualização que você usou para exportar os dados (por
exemplo, by-deviceType ou by-date.)
4. Selecione o ativo de dados CSV transferido por upload.
5. No campo **Faça uma pergunta sobre seus dados**, faça uma pergunta usando a língua natural.
5. Abra a sugestão de visualização que melhor corresponde a sua pergunta. Você
pode revisar manualmente a sugestão.
7. Salve a visualização.


## Exemplos de visualização de dados usando o WA
{: #WA_visualize}

Esta seção mostra exemplos de análise de dados usando o WA como ferramenta de análise.

**Nota:** Estes exemplos devem dar a você uma ideia do que
esperar quando executar suas próprias visualizações. Os resultados nos exemplos que são
mostrados aqui podem ser diferentes dos resultados que você vê ao executar essas
visualizações com os dados de amostra, devido, por exemplo, aos dados serem reunidos em
datas e horas diferentes.

### Visualizando o funcionamento do dispositivo

Nesta seção, aprendemos sobre o preenchimento de dispositivos IoT e respondemos a perguntas como:

1. Quantos dispositivos têm relatado?
2. Qual é o detalhamento dos dispositivos por tipo de dispositivo?
3. Quantos relatórios tinha um dispositivo?
4. Quantos relatórios foram enviados por cada dispositivo?

**Quantos dispositivos têm relatado?**

Neste exemplo, contamos o número de dispositivos que relataram durante o intervalo
especificado para detectar se os dispositivos têm relatado como esperado. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Quantos deviceId estão aí?"*

Aqui está o resultado mostrando que há 17 dispositivos:

![Resultado da contagem de dispositivos](images/device_count.png)

**Qual é o detalhamento dos dispositivos por tipo de dispositivo?**

Neste exemplo, comparamos o número de dispositivos por tipo de dispositivo que
relataram durante o intervalo para determinar se os dispositivos de todos os tipos têm
relatado conforme o esperado. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Como comparar o número de deviceId por deviceType?"*

Aqui está o resultado mostrando o detalhamento de dispositivos por tipo de dispositivo:

![Resultado do detalhamento de dispositivos por tipo de dispositivo](images/deviceID_deviceType.png)

Para visualizar esses dados em um gráfico de pizza, clique em
**Visualização** à esquerda e selecione **Pizza**.

![Gráfico de pizza mostrando o detalhamento de dispositivos por tipo de dispositivo](images/deviceID_deviceType_pie.png)


**Quantos relatórios tinham um dispositivo?**

Neste exemplo, contamos o número de relatórios que foram feitos por um dispositivo
para detectar as condições de rede e outros problemas relacionados a dispositivos. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Quantas linhas há? filtrado por deviceId: Ahuza"*

**Nota:** Você não precisa digitar os nomes completos dos campos. O
WA tenta adivinhar o nome completo do campo, mas os valores de filtro (por exemplo.
"Ahuza") devem ser escritos na forma completa e correta. Se você não vir uma sugestão
correto com o filtro, clique no link **Mostrar próximo** ou tente a
pergunta *"Quantas linhas há?"*. Em seguida, abra o diagrama, clique na
caixa **Multiplicador** abaixo do diagrama e selecione o parâmetro
deviceId na lista. Desmarque todos os deviceIds irrelevantes.

Este é o resultado mostrando que há 25 linhas ou relatórios que foram feitos pelo
dispositivo Ahuza:

![Número de resultados do relatório](images/25_rows.png)


**Quantos relatórios tiveram cada um dos dispositivos diferentes?**

Neste exemplo, comparamos o nível de atividade dos dispositivos com base no número
de relatórios que cada dispositivo enviou durante o intervalo inspecionado. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Como o número de Linhas é comparado por deviceId?"*

Este é o resultado mostrando um gráfico de barras com a atividade dos diferentes
dispositivos:

![Resultados da comparação deatividades do dispositivo](images/compare_activity.png)



### Visualizando dados do sensor de tipo de dispositivo

Nesta seção, aprendemos sobre os dados do sensor de resumo que são relatados por
todos os dispositivos de um tipo, respondendo a perguntas como:

1. Qual é a Média/Mín./Máx. de todos os valores relatados do sensor?
2. Posso ver um histograma do resultado de um sensor.  
3. Qual é a correlação entre dois sensores?


**Qual é a Média/Mín./Máx. de todos os valores relatados do sensor?**

Neste exemplo, nós resumimos os parâmetros numéricos que são relatados por todos os
dispositivos de um tipo em uma tabela. Nessa tabela, podemos aprender sobre o intervalo
de valores que são detectados no ambiente e obter uma perspectiva ampla dos dados
detectados.

Esta visualização deve ser construída manualmente, usando as etapas a seguir:

1.	Na seção **Criar sua própria visualização**, selecione **Tabela**.
2.	Clique no botão de sinal de mais "criar nova coluna" e selecione **Cálculo**.
3.	Nomeie a nova coluna, selecione a coluna para esse cálculo na lista suspensa **Colunas** e clique em **Pronto** para duplicar a coluna. A nova coluna é incluída na extremidade direita da bandeja de dados.
4.	Clique com o botão direito no título da nova coluna, selecione um tipo de
agregação (mínima, máxima ou média) e depois feche a janela de propriedades.
6.	Repita o processo para incluir mais colunas e, em seguida, oculte a bandeja de dados.
7.	Clique em **Colunas** e selecione
**Medidas** no fim da lista.
8.	Clique em **Agregado por** e selecione todos os cálculos que você incluiu.
9.	Clique em **Pronto**.
10.	Salve a visualização.

Aqui está o resultado mostrando o intervalo de valores:

![Resultado dos valores dointervalo do sensor](images/sensor_range_values.png)



**Posso visualizar um histograma do resultado de um sensor de dispositivo?**

Neste exemplo, vamos avaliar o comportamento de um sensor em todos os dispositivos
de um tipo, identificando a distribuição de valores que são detectados no
ambiente. Podemos usar essa visualização para aprender sobre o ambiente que é detectado
pelos sensores, assim como sobre limitações nos sensores. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Como o número de Linhas é comparado por TEMP?"*

Este é o resultado mostrando a comparação do número de linhas:

![Resultado da comparação da contagem de linhas](images/number_rows.png)


**Qual é a correlação entre dois sensores?**

Neste exemplo, aprendemos sobre correlações no ambiente, comparando as medidas de
dois sensores de dispositivo em todos os dispositivos do tipo. Para concluir esta análise, copie e cole ou digite uma das perguntas no WA:

*"Qual é o relacionamento entre NO2 e NOX?"* ou *"Como são
associados os valores de NO2 e NOX?"*

Aqui está o resultado mostrando o relacionamento entre os dois sensores:

![Resultado do relacionamento do sensor](images/sensor_relationship.png)

Você também pode visualizar os dados do sensor usando pontos coloridos por ID do
dispositivo. Para fazer isso, selecione o deviceID na caixa **Cor**.

Este é o resultado mostrando um subconjunto limitado dos dispositivos:

![Relacionamento do sensor usando pontos coloridos](images/sensor_color_dots.png)


### Visualizando detalhes do sensor (detalhamento)

Nesta seção, estudamos os parâmetros específicos que são relatados por um
dispositivo específico, respondendo às perguntas a seguir:

1.	Qual é o valor Médio/Mín./Máx. relatado?
2.	Posso ver um histograma de saída de um sensor de dispositivo?
3.	Como um valor de sensor de dispositivo específico muda ao longo do tempo?
4.	Como os valores do sensor de dois dispositivos são comparados ao longo do tempo?
5.	Como os valores do sensor do mesmo dispositivo são comparados ao longo do tempo?
6.	Qual é a correlação entre dois sensores de um dispositivo?


**Qual é o valor Médio/Mín./Máx. relatado?**

Neste exemplo, nós resumimos os parâmetros numéricos que são relatados por um
dispositivo específico em uma tabela para aprender, por exemplo, sobre o intervalo de
valores detectados no ambiente ou sobre mau funcionamento do sensor.

Esta visualização deve ser construída manualmente, usando as etapas a seguir:

1)	Na seção **Criar sua própria visualização**, selecione **Tabela**.
2)	Clique no botão de sinal de mais "criar nova coluna" e selecione **Cálculo**.
3)	Nomeie a nova coluna, selecione a coluna para esse cálculo na lista suspensa **Colunas** e clique em **Pronto** para duplicar a coluna. A nova coluna é incluída na extremidade direita da bandeja de dados.
4)	Clique com o botão direito no título da nova coluna, selecione um tipo de
agregação (mínima, máxima ou média) e depois feche a janela **Propriedades**.
6)	Repita o processo para incluir mais colunas e, em seguida, oculte a bandeja de dados.
7)	Clique em **Colunas** e selecione **Medidas**.
8)	Clique em **Agregado por** e selecione todos os cálculos que você incluiu.
9)	Clique em **Pronto**.
10)	Na caixa multiplicador, selecione o parâmetro deviceId e selecione os
dispositivos relevantes a serem exibidos.
11)	Salve a visualização.

Este é o resultado mostrando os valores especificados:

![Relatar o resultado do detalhamento de valores](images/deep_avg_min_max.png)


**Posso ver um histograma de saída de um sensor de dispositivo?**

Neste exemplo, nós avaliamos o comportamento de um sensor de dispositivo
específico, identificando a distribuição de valores detectados no ambiente. Podemos usar
essa visualização para aprender sobre o ambiente que é detectado pelo sensor, assim como
sobre potencial mau funcionamento no sensor. Para concluir esta análise, copie e cole ou digite uma das perguntas no WA:

*"Qual é a distribuição de TEMP. filtrado por deviceId: Ahuza"* ou
*"Como o número de Linhas é comparado por TEMP? filtrado por deviceId: Ahuza"*

Este é o resultado mostrando os dados de saída do sensor de dispositivo em um histograma:

![Resultado do histograma da saída do sensor de dispositivo](images/deep_histogram.png)


**Como um valor de sensor de dispositivo específico muda ao longo do
tempo?**

Neste exemplo, aprendemos como as leituras de um sensor específico de um
dispositivo específico mudam, refletindo as mudanças no ambiente ao longo do tempo. Isso pode ajudar com o planejamento e detecção de problemas. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Qual é a tendência de TEMP ao longo do tempo? filtrado por deviceId: Ahuza".*

Este é o resultado mostrando a tendência de dados do sensor ao longo do tempo:

![Resultado da tendência de dados do sensor de dispositivo ao longo do tempo](images/deep_sensor_trend.png)


**Como os valores do sensor de dois dispositivos são comparados ao longo
do tempo?**

Neste exemplo, comparamos as tendências de leituras do sensor de diferentes
dispositivos, identificando relacionamentos entre os dispositivos para detectar
anomalias, mau funcionamento de dispositivo e assim por diante. Para concluir esta análise, copie e cole ou digite uma das perguntas no WA:

*"Qual é a tendência de TEMP ao longo do tempo por deviceId?"* ou
*"Qual é a tendência de TEMP ao longo do tempo por deviceId?  filtrado por deviceId: Ahuza, Igud "*

Este é o resultado mostrando a comparação de valor do sensor ao longo do tempo:

![Comparação de dados do sensor entre dispositivos ao longo do tempo](images/deep_sensor_compare.png)

Você também pode visualizar essas informações clicando no nome do parâmetro na
parte inferior do gráfico. Várias linhas são desenhadas (uma por deviceId). Os deviceIds
relevantes podem ser selecionados na lista.

![Comparação de dados do sensor entre dispositivos ao longo do tempo mostrados com linhas](images/deep_sensor_compare_lines.png)

É possível usar a caixa **Multiplicador** abaixo do gráfico e
escolher deviceId para apresentar os gráficos lado a lado.

![Comparação de dados do sensor lado a lado](images/deep_sensor_compare_side.png)


**Como os valores do sensor do mesmo dispositivo são comparados ao longo
do tempo?**

Neste exemplo, nós mutuamente visualizamos a tendência de dois sensores de
dispositivo para obter mais insight das mudanças no ambiente ao longo do tempo. Para concluir esta análise, copie e cole ou digite a pergunta a seguir no WA:

*"Qual é a tendência de NO2 e NOX ao longo do tempo por deviceId?  filtrado por deviceId: Ahuza"*

Este é o resultado mostrando a tendência de dois sensores de dispositivo ao longo do tempo.

![Comparação da tendência de dois sensores de dispositivo ao longo do tempo](images/deep_two_devices.png)


**Qual é a correlação entre dois sensores de um dispositivo?**

Neste exemplo, aprendemos sobre correlações no ambiente comparando as medidas de dois sensores de dispositivo. Para concluir esta análise, copie e cole ou digite uma das perguntas no WA:

*"Qual é o relacionamento entre NO2 e NOX. filtrado por deviceId:
Ahuza"* ou *"Como são associados os valores de NO2 e NOX? filtrado por deviceId: Ahuza"*

Este é o resultado mostrando a correlação entre dois sensores de um dispositivo:

![Comparação de dois sensores de um dispositivo](images/deep_two_sensors.png)


## O que Vem a Seguir?

Para obter mais informações sobre o WA, consulte os recursos a seguir:
- [Centro de desenvolvedores do Watson Analytics ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de linkexterno")](https://developer.ibm.com/watson-analytics/){: new_window}

- [Comunidade do Watson Analytics ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/communities/analytics/watson-analytics/){: new_window}
- [Fórum do Watson Analytics ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://community.watsonanalytics.com/discussions/spaces/15/view.html){: new_window}
