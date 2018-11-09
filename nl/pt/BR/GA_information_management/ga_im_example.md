---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao gerenciamento de dados usando APIs de REST
{: #im_example}

Use as etapas a seguir para ajudá-lo a configurar os recursos que você precisa para começar a usar os recursos gêmeos de dispositivo e ativo do componente de gerenciamento de dados do {{site.data.keyword.iot_full}}.

Para obter detalhes sobre a API, consulte a documentação da [API de REST HTTP do {{site.data.keyword.iot_short_notm}}![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Fluxo de trabalho de alto nível
{: #workflow}

Use as etapas a seguir para ajudá-lo a configurar os recursos de que você precisa para começar a mapear os seus dados de dispositivo ou Coisa usando o recurso gêmeo de dispositivo ou ativo.

**Dica:** para obter informações mais detalhadas sobre cada uma das etapas, consulte os cenários de exemplo ou use os links para acessar diretamente uma etapa específica no cenário de exemplo. O [Guia passo a passo 1](ga_im_index_scenario.html#scenario) guia você pelas etapas para criar uma interface lógica de tipo de dispositivo para dispositivos termômetro heterogêneos e o [Guia passo a passo 2](../information_management/im_index_scenario_thing.html#scenario) aumenta o cenário descrevendo como construir uma interface lógica que permite que seu aplicativo consuma dados de dois tipos diferentes de dispositivos de clima associados em uma Coisa do tipo de sala.

O processo para criar e consumir interfaces lógicas difere um pouco, dependendo se você está criando uma interface lógica que está associada a um tipo de dispositivo ou a um tipo de Coisa.

### Antes de iniciar
Para criar uma interface lógica associada a um tipo de dispositivo ou tipo de Coisa, deve-se ter [pelo menos um dispositivo registrado com o {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) e eventos de envio com propriedades de estado.  


### Etapas

1. 	Defina as propriedades de estado de entrada.  
Primeiro, defina as propriedades de estado recebidas que você deseja que sua interface lógica torne disponível para seus aplicativos.  
Dependendo da interface lógica que você está criando, faça uma das duas coisas:
<dl>
<dt>Tipo de dispositivo: Criar uma interface física.</dt>
<dd>
<ol>
<li>[Crie um arquivo de esquema de evento ](ga_im_index_scenario.html#step1). O arquivo de esquema de evento é um arquivo .JSON local que define a estrutura e o formato de um evento de entrada.
<li>[Crie um recurso de esquema de evento para seu tipo de evento](ga_im_index_scenario.html#step2). O recurso de esquema de evento é uma construção programática que é usada pelo {{site.data.keyword.iot_short_notm}}.
<li>[Crie um tipo de evento que referencie o esquema de evento](ga_im_index_scenario.html#step3). O tipo de evento é utilizado pelo {{site.data.keyword.iot_short_notm}} para mapear um ou mais recursos de esquema de evento para uma interface física.
<li>[Crie uma interface física ](ga_im_index_scenario.html#step7).
<li>[Inclua o tipo de evento na interface física](ga_im_index_scenario.html#step8).
<li>[Inclua sua interface física em seu tipo de dispositivo](ga_im_index_scenario.html#step9).
</ol>
</dd>
<dt>Tipo de encadeamento: Defina um tipo de encadeamento.</dt>
<dd>
<ol>
<li>[Crie um arquivo de esquema de tipo de Coisa](../information_management/im_index_scenario_thing.html#crt_composition_file).  
Um arquivo de esquema de tipo de Coisa é um arquivo .JSON local que define a composição do tipo de coisa, apontando para as interfaces lógicas existentes.
<li>[Crie o recurso de esquema do tipo de Coisa](../information_management/im_index_scenario_thing.html#crt_composition_resource).  
Faça upload do arquivo .JSON local para o {{site.data.keyword.iot_short_notm}}.
<li>[Crie um tipo de Coisa](../information_management/im_index_scenario_thing.html#crt_thing_type). Um tipo de Coisa serve o mesmo propósito que um tipo de dispositivo no que representa uma classe de Coisas.
</ol>
</dd>
</dl>
4. 	Crie a interface lógica.
 1. 	Crie um arquivo de esquema de interface lógica para o [tipo de dispositivo](ga_im_index_scenario.html#step4) ou [tipo de Coisa](../information_management/im_index_scenario_thing.html#crt_ai_schema_file).  
Um arquivo de esquema de interface lógica é um arquivo .JSON local que define o estado do dispositivo que é disponibilizado para seus aplicativos.
 2. 	Crie um recurso de esquema de interface lógica para o [tipo de dispositivo](ga_im_index_scenario.html#step5) ou [tipo de Coisa](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource).
 3.	Crie uma interface lógica para o [tipo de dispositivo](ga_im_index_scenario.html#step6) ou [tipo de Coisa](../information_management/im_index_scenario_thing.html#crt_thing_ai).
 4.	Inclua a interface lógica para o [tipo de dispositivo](ga_im_index_scenario.html#step10) ou [tipo de Coisa](../information_management/im_index_scenario_thing.html#add_thing_ai).
5. 	Defina os mapeamentos para o [tipo de dispositivo](ga_im_index_scenario.html#step11) ou [tipo de Coisa](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings).   
Os mapeamentos são usados para mapear propriedades de entrada para propriedades na interface lógica.  
6. 	Valide e ative a configuração que está associada ao [tipo de dispositivo](ga_im_index_scenario.html#step15) ou [Thing_type](../information_management/im_index_scenario_thing.html#activate) de rascunho.
7. 	Recupere o estado do [dispositivo](ga_im_index_scenario.html#step13) ou [Coisa](../information_management/im_index_scenario_thing.html##verify_Thing_state).  
Verifique se suas assinaturas mostram os dados do dispositivo atualizados ou se os dados do dispositivo atualizados são retornados usando uma chamada REST ou assinando uma sequência de tópicos.  



