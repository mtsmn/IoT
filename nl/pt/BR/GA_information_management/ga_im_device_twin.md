---

copyright: years: 2016, 2017 lastupdated: "2017-07-20"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao gerenciamento de dados
{: #device_twins}

Existe um número sem precedentes de dispositivos e sensores no mundo moderno. Muitos
desses dispositivos fornecem funcionalidade semelhante, mas as variações de marca, modelo
e versão significam que os dados são produzidos em formatos diferentes. Por exemplo, um
sensor de temperatura pode registrar temperatura em graus Fahrenheit ou Celsius. Não é
eficiente codificar os aplicativos para que possam consumir dados em todos esses
formatos; em vez disso, os dados precisam ser normalizados para criar uma única
visualização lógica que pode ser usada pelos aplicativos.
{: shortdesc}

Use o recurso de gerenciamento de dados no {{site.data.keyword.iot_full}} a
fim de configurar um dispositivo gêmeo para expor uma visualização normalizada dos dados
aos seus aplicativos.

Um dispositivo gêmeo é uma representação digital na nuvem de um dispositivo físico
ou sensor que está conectado ao {{site.data.keyword.iot_short_notm}}. Um
dispositivo gêmeo cria um modelo lógico das propriedades e eventos provenientes de um
determinado sensor ou dispositivo. Quando definido e instanciado, o dispositivo gêmeo
fornece um meio consistente de interagir com um dispositivo em um modo REST,
independentemente de se o dispositivo está on-line ou off-line. Como um modelo lógico
pode ser compartilhado por vários dispositivos de diferentes marcas e modelos, o
aplicativo IoT agora é isolado de variabilidade e mudanças dentro do ecossistema do
dispositivo. As propriedades de um dispositivo, incluindo informações sobre o estado
atual dele (estado do dispositivo) podem ser recuperadas usando uma solicitação
HTTP ou assinando um tópico.

Dispositivos gêmeos podem ajudar a:
- Fornecer aos seus desenvolvedores de aplicativos interfaces consistentes para
acessar dados de dispositivo orientados a evento em uma maneira do tipo REST.
- Normalizar dados de dispositivos de diferentes marcas ou modelos que publiquem
dados em formatos diferentes.

Para usar o recurso de gerenciamento de dados para configurar um dispositivo gêmeo,
você precisa definir as informações a seguir configurando recursos no {{site.data.keyword.iot_short_notm}}:
- A estrutura dos eventos que são enviados por seu dispositivo. A estrutura de um
evento de entrada é definida nos recursos de interface física, tipo de evento e esquema
de evento. 
- As propriedades que você deseja registrar. Essas propriedades definem a
estrutura lógica do estado do dispositivo que pode ser consumida por seus aplicativos. As
propriedades são definidas nos recursos de interface lógica e esquema lógico.
- Como os eventos da interface física são mapeados para as propriedades da
interface lógica. Use o recurso de mapeamentos para mapear eventos para as propriedades.

O diagrama a seguir mostra dados do dispositivo em formatos diferentes vindo para o
{{site.data.keyword.iot_short_notm}} e sendo transformados e normalizados em uma
única visualização lógica que pode ser facilmente consumida por aplicativos backend.  

![Visão geral do
gerenciamento de dados no {{site.data.keyword.iot_short_notm}}.](images/ga_im_resources_overview.svg "Visão geral dogerenciamento de dados no {{site.data.keyword.iot_short_notm}}")


Para obter mais informações sobre como definir e configurar informações e recursos
chave, consulte [Entendendo o gerenciamento de
dados](ga_im_definitions.html). Você pode criar seu próprio dispositivo gêmeo no
{{site.data.keyword.iot_short_notm}} concluindo as etapas descritas na
[Introdução ao gerenciamento de dados](ga_im_example.html). Para obter
informações mais detalhadas sobre cada uma das etapas descritas no guia, consulte o
cenário de exemplo que está documentado em
[Guia passo a passo: um exemplo detalhado
sobre como trabalhar com dispositivos por meio de uma interface comum](ga_im_index_scenario.html#scenario). 
