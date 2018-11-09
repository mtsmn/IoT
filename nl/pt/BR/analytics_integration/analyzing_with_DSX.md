---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Analisando dados usando o Data Science Experience
{: #DSX_integration}

É possível usar o {{site.data.keyword.iot_full}} com o Data Science
Experience (DSX) para visualizar e aprender sobre os dados que são enviados de dispositivos
que estão conectados à plataforma.
{: shortdesc}

## Visão geral e objetivos

Este guia orienta você, passo a passo, pelo processo de visualização de dados de evento de dispositivo do {{site.data.keyword.iot_short_notm}} usando o DSX como ferramenta de análise.

O DSX fornece um ambiente interativo, colaborativo, baseada em nuvem no qual você pode usar várias ferramentas para ativar seus insights. Use o Jupyter Notebook, que está disponível no IBM DSX, para carregar dados históricos do {{site.data.keyword.iot_short_notm}} e use os dados para criar visualizações para auxiliar em análise de dados e identificação de anomalia. É possível derivar valores de limites de dados históricos e usar esses valores para criar regras de nuvem no {{site.data.keyword.iot_short_notm}}. As regras de nuvem alertam os usuários quando um dispositivo IoT que está associado a uma regra publica uma leitura que está fora dos limites configurados.

Os dados do dispositivo enviados para {{site.data.keyword.iot_short_notm}} podem ser coletados e armazenados no {{site.data.keyword.Bluemix}} usando o serviço {{site.data.keyword.cloudantfull}} NoSQL DB. Para coletar os dados, deve-se primeiro conectar a {{site.data.keyword.iot_short_notm}} ao serviço {{site.data.keyword.cloudant_short_notm}}. Os dados do dispositivo são armazenados nos bancos de dados do {{site.data.keyword.cloudant_short_notm}} de forma diária, semanal ou mensal, dependendo do intervalo de depósito que está configurado.


![Visão geral de como usar o DSX para analisar dados](images/DSX_overview.png)

Como parte deste guia, você aprenderá:

 - Como configurar o armazenamento de dados da plataforma para que o Cloudant NoSQL DB seja usado como o serviço de historiador.
 - Como usar o simulador Weather Sensors para gerar dados para serem usados pela plataforma.
 - Como exportar os dados e, em seguida, importá-los para o DSX para analisar dados.
 - Como visualizar dados do IoT, detectar anomalias e configurar alertas.


## Pré-requisito

Para concluir estas etapas deve-se ter acesso ao [{{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} com [Cloudant NoSQL DB ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")] (https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}, acesso ao Serviço [Apache Spark ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} e acesso a uma [Conta do DSX ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window}.


## Etapa 1. Configurar o simulador
{:#DSX_sensor_data}

Para conduzir uma análise significativa, deve-se ter dados significativos. É possível simular os dados do sensor real para aprender sobre como os dados do dispositivo do Watson IoT Platform podem ser analisados usando o DSX. Esta etapa fornece instruções para:
 - [Configurando o simulador com uma **instância existente de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Configurando o simulador com uma **nova instância de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)


### Configurando o simulador Weather Sensors com uma instância existente de {{site.data.keyword.iot_short_notm}}
{: #sim_existing_platform}

Para simular eventos reais de dados do sensor com relação às suas organizações usando o simulador Weather Sensors, deve-se primeiro configurar o simulador. Estas etapas presumem que você já tem uma instância de {{site.data.keyword.iot_short_notm}} funcionando.

1. [Gere a apikey e o token que são necessários para executar o simulador. ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Implemente o app da web de simulador Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} e siga as instruções detalhadas.

   Para obter mais informações sobre o Weather Sensors, veja [o guia do simulador Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Continue com a [Etapa 2. Configurar o conector do banco de dados](#DSX_config_db).


### Configurando o simulador Weather Sensors com uma nova instância de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Para simular eventos reais de dados do sensor com relação às suas organizações usando o simulador Weather Sensors, deve-se primeiro configurar o simulador. Estas etapas incluem as instruções para criar uma instância de {{site.data.keyword.iot_short_notm}} juntamente com o simulador.

1. [Implemente o app da web de simulador Weather Sensors com uma instância de {{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} e siga as etapas detalhadas.

   Para obter mais informações sobre o Weather Sensors, veja [o guia do simulador Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Aguarde até que a implementação seja concluída e, em seguida, navegue para o painel do IBM Cloud.
3. Ative o serviço "wiotp-for-weather-sensors-simulator" de {{site.data.keyword.iot_short_notm}} que foi criado pelo processo de implementação.
4. Continue com a [Etapa 2. Configurar o conector do banco de dados](#DSX_config_db).


## Etapa 2. Configurar o conector do banco de dados
{: #DSX_config_db}

Dados do dispositivo podem ser armazenados no Cloudant em bancos de dados diários,
semanais ou mensais, dependendo da opção do depósito selecionado. Os dados que são
coletados de todos os dispositivos no mesmo intervalo do depósito (dia, semana ou mês)
são armazenados no mesmo banco de dados.

Independentemente da configuração do depósito, três bancos de dados são
automaticamente criados pelo conector. Um banco de dados é criado para o intervalo de depósito atual, um para o próximo intervalo e um para o banco de dados de configuração. Quando o término do intervalo é atingido, os dados do dispositivo são armazenados no banco de dados do depósito para o novo intervalo e um novo banco de dados é criado para o depósito subsequente.

Para usar o {{site.data.keyword.cloudant_short_notm}} com o DSX, deve-se
configurar o armazenamento de dados da plataforma para que o Cloudant NoSQL DB seja usado
como serviço historiador.

1. No painel do {{site.data.keyword.cloudant_short_notm}}, clique em **Extensões** na barra de navegação.
2. Em **Armazenamento de dados históricos**, clique em **Configuração**. A seção **Configurar armazenamento de dados históricos** lista todos os serviços do Cloudant NoSQL DB que estão disponíveis no mesmo espaço do IBM Cloud que o {{site.data.keyword.cloudant_short_notm}}.
3. Selecione o serviço Cloudant NoSQL DB que você deseja conectar.
4. Especifique as opções de configuração do Cloudant NoSQL DB a seguir:
  - Intervalo de depósito = dia
  - Fuso horário = UTC
  - Nome do banco de dados = padrão
5. Clique em **Pronto** e confirme a autorização para a conexão com o serviço Cloudant. Assegure-se de que os pop-ups estejam ativados em seu navegador para ter acesso à janela de confirmação. Quando você tiver configurado com êxito o Cloudant NoSQL DB, o status de Armazenamento de
dados históricos será mudado para Configurado e os dados do dispositivo serão armazenados no
{{site.data.keyword.cloudant_short_notm}} NoSQL DB.
6. Continue com a [Etapa 3. Executar o simulador](#run_simulator).


## Etapa 3. Executar o simulador
{: #run_simulator}

O simulador publica dados reais dos sensores de clima, de 17 estações meteorológicas localizadas na área de Haifa, em sua organização de {{site.data.keyword.iot_short_notm}}.

1. Navegue para o simulador.
2. Se você implementou o simulador com uma instância do
{{site.data.keyword.iot_short_notm}} ligada, prossiga para a etapa 3. Se
implementou uma versão independente do simulador, insira os detalhes a seguir:
   - Organização do Watson IoT Platform
   - Chave API
   - token de autenticação

3. Clique em **Executar simulador**. Os dados levarão alguns minutos para serem gerados.
4. Acesse a plataforma Watson IoT enquanto o simulador está sendo executado e verifique se os dispositivos foram criados e se os eventos estão vindo para esses dispositivos.
5. Prossiga para a [Etapa 4. Configurar o DSX e visualizar dados](#DSX_visualize_data).


## Etapa 4. Configurar o DSX e visualizar dados
{: #DSX_visualize_data}

Para configurar o DSX e começar a visualizar dados:

1. [Instale um Notebook Jupyter
pré-configurado](#setup_jupyter_notebook) para obter insights sobre seus dados e para detectar anomalias.
2. [Execute a análise.](#run_analysis)
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. Instalar um Notebook Jupyter pré-configurado
{: #setup_jupyter_notebook}

O Jupyter Notebook é um aplicativo da web que permite criar e compartilhar documentos que contêm código executável, fórmulas matemáticas, gráficos/visualização (matplotlib) e texto explicativo.

Para instalar um Notebook Jupyter pré-configurado para obter insights sobre seus dados e detectar anomalias:
1. Use um navegador suportado para efetuar login no [DSX ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://datascience.ibm.com/){: new_window} com seu ID do IBM Cloud.
2. Clique em **+ Novo projeto** para criar um novo projeto e selecione o tile **Jupyter Notebooks**. Os projetos criam um espaço para que você colete e compartilhe anotações, conecte-se a origens de dados, crie pipelines e inclua conjuntos de dados.
3. Clique no menu suspenso **+Incluir no projeto** e selecione **Conexão**. Na lista de serviços, selecione **Cloudant** e, em seguida, insira a URL do Cloudant, o Nome do usuário e a Senha localizados na guia Credenciais de serviço da página do serviço {{site.data.keyword.cloudant_short_notm}} e insira-os nos campos mostrados. Especifique um nome para a conexão e clique em **Criar**.
4. Verifique se a conexão se tornou disponível na seção Ativos de dados do painel.
5. Clique em **+ Novo bloco de notas** para criar um novo bloco de notas do Jupyter.
6. No menu suspenso Selecionar tempo de execução sob Serviços, selecione **spark-iw**. Se ele não estiver presente, isso significa que o serviço Apache Spark não foi provisionado corretamente. Verifique no IBM Cloud Dashboard se ele está presente e, se não, visite a página de serviço para provisioná-lo.
7. Configure a Linguagem como **Python 2** e a versão do Spark como **2.1**.
8. Selecione **Da URL** para carregar um bloco de
notas existente; em seguida, especifique um nome descritivo para o bloco de notas e
insira a URL a seguir para abrir o bloco de notas de amostra:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. Clique em **Criar bloco de notas**. Verifique se o bloco de
notas é criado com metadados e código.
10. Selecione a célula que inicia com '!pip install --upgrade pixiedust, ,' e, em seguida, clique em **Executar** para executar o código.
11. Quando a instalação estiver concluída, reinicie o kernel Spark clicando no ícone **Reiniciar kernel** ou selecionando **Reiniciar** no menu Kernel.
12. Espere cerca de 10 segundos para que o kernel seja reiniciado e, em seguida, clique na célula de código vazia com o comentário para selecioná-lo.
13. Importe suas credenciais do Cloudant para essa célula, concluindo as etapas a
seguir:

    1. Clique em  ![Find and add data](images/find_add_data_icon.png).
    2. Selecione a guia **Conexões**.
    3. Clique em **Inserir no código**.
Um dicionário denominado credentials_1" é criado com suas credenciais do Cloudant. Se o
nome não for especificado como "credentials_1", renomeie o dicionário para
"credentials_1" porque esse é o nome que é necessário para a execução do código do bloco
de notas.
14. Na célula com o nome do banco de dados (dbname), insira o nome do banco de
dados Cloudant que é a origem de dados, por exemplo, `iotp_yourWIoTPorgId_default_Year-month-day`.
15. Salve o bloco de notas. Ele está pronto para ser executado.


### 2. Executar a análise
{: #run_analysis}

1.	Selecione a célula que contém suas credenciais do Cloudant.
2.	Clique em **Reproduzir** para executar o código de célula.
3.	Verifique os resultados de execução e analise o código Python que é usado em cada célula.
4.	Repita as etapas 2 e 3 para cada célula. Para células com **Entrada do
usuário necessária** especificado, deve-se fornecer novos valores de entrada
para a variável que é definida na próxima célula de código antes de sua execução.

**Nota:** Algumas células executam tarefas Spark em segundo
plano e podem levar mais tempo para serem concluídas. Quando a execução de código dentro
de uma célula é concluída, o asterisco `*` se torna um número, por
exemplo, Em `[*]` se torna Em `[1]`. Depois de concluir
as etapas, será possível criar regras de nuvem no
{{site.data.keyword.iot_short_notm}} para automaticamente gerar alertas quando
anomalias forem detectadas.


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## O que Vem a Seguir?

Para obter mais informações sobre DSX, consulte os recursos a seguir:

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [Blocos de notas e tutoriais do DSX ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} seguindo os links para aprender mais sobre blocos de notas Jupyter.
 - [Receitas de análise no cookbook do Watson IoT Platform ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
