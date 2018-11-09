---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configurando  {{site.data.keyword.iot_short_notm}}  Regras de Roteamento de Edge (Visualização)
{: #edge_rules}

As regras de roteamento do {{site.data.keyword.iot_full}} Edge definem como as mensagens são encaminhadas dentro do nó Edge.

**Importante:** o recurso {{site.data.keyword.iot_full}} Edge está disponível apenas como parte de um programa de visualização limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

É possível definir múltiplas regras de roteamento para um nó Edge. As regras são avaliadas para cada mensagem que um dispositivo ou aplicativo local publica diretamente no nó Edge. As mensagens que são publicadas para o nó Edge por meio da nuvem são sempre publicadas localmente e não são impactadas pelo roteamento.

## Formato de regras de roteamento
{: #edge_rules_format}

Uma regra de roteamento é definida como um objeto JSON no formato a seguir:

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

Uma regra de roteamento é especificada como matriz de objetos JSON no arquivo `routing.json` que está armazenado na pasta `/etc/wiotp-edge` no nó Edge.

As propriedades de cada regra de roteamento são definidas conforme a seguir:

Propriedade      | Tipo     | Descrição       
------------- | -----------| -----------
rule_id | inteiro, obrigatório | Um identificador para a regra. As regras de roteamento são avaliadas em ordem crescente de acordo com o rule_id. Não deve haver mais de uma regra com o mesmo rule_id.
filching_filo_mat | cadeia, obrigatória | Usado para avaliar se a regra deve ser aplicada a uma mensagem publicada. Deve estar no formato de uma assinatura do MQTT. Os curingas de nível único (+) e multinível (#) do MQTT são suportados. Esse filtro é avaliado com relação ao tópico sobre o qual a mensagem é publicada. Essa propriedade é definida no formato de tópico de aplicativo do {{site.data.keyword.iot_short_notm}} e inclui os campos `/type/deviceType/id/deviceId`. Por exemplo:  ` "matching_filter": " iot-2/type/ + /id/ + /evt/EvtTypeAlarm/#" `
Destino | cadeia, obrigatória | Define onde a mensagem deve ser encaminhada. Configure um dos valores a seguir: `"CLOUD"`, `"IM"` (para encaminhar a mensagem para o componente Information Management) ou `"LOCAL"` (para encaminhar a mensagem localmente no nó Edge). Por exemplo,  ` "destination": "CLOUD" `
continue_matching | boolean, opcional, padrão = true | Especifica se mensagens adicionais devem ser avaliadas usando essa regra se a regra for correspondida uma vez. Essa propriedade fornece melhor controle sobre o roteamento quando as regras de roteamento de sobreposição existem.
forward_skip_levels | inteiro, opcional, padrão = 0) | Especifica o número de níveis do tópico de mensagem original que deverá ser removido quando a mensagem for publicada novamente. O valor padrão é 0, o que significa que o tópico original é mantido. Por exemplo, se `forward_skip_levels` estiver configurado como `1` e uma mensagem for publicada no tópico `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`, a mensagem será publicada novamente no tópico `iot-2/type/T1/id/D1/evt/E1/fmt/json`.
armazenamento | boolean, opcional, padrão = true) | Define se o recurso de armazenamento e encaminhamento deve ser aplicado a mensagens que são roteadas por essa regra. Essa propriedade se aplica somente quando a propriedade de destino é configurada como `"CLOUD"`. Se essa propriedade for configurada como false, as mensagens que corresponderem à regra não serão armazenadas quando o nó Edge for desconectado da nuvem.

## Exemplo de um arquivo routing.json
{: #edge_rules_example}

No exemplo a seguir, três regras são configuradas:
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

A primeira regra roteia todos os eventos do tipo `AlarmEvent` diretamente para a nuvem.

A segunda regra roteia todos os eventos que são publicados em um tópico iniciando com `sendToCloud/iot-2` diretamente para a nuvem. A mensagem é encaminhada em um tópico que inicia com `iot-2` e não inclui o prefixo `sendToCloud/`.

A terceira regra é a regra padrão, que roteia todas as outras mensagens para o broker MQTT local. Essas mensagens podem então ser usadas por qualquer consumidor local com uma assinatura correspondente. A instalação do nó Edge inclui a regra padrão para rotear tudo localmente, em `/etc/wiotp-edge/routing.json`.

## Editando Regras de Roteamento
{: #edge_rules_edit}

É possível editar regras diretamente no arquivo `/etc/wiotp-edge/routing.json`. Após ter editado o arquivo, salve suas mudanças e, em seguida, execute `docker ps` para determinar o ID do contêiner do conector de borda. Reinicie o contêiner do docker usando esse ID.

## Resolução de problemas de regras de rote
{: #edge_rules_ts}

Se o contêiner do conector de borda continuar reiniciando após uma mudança em `routing.json`, isso significa que o arquivo de regras de roteamento contém erros de sintaxe. Verifique o arquivo de log `/var/wiotp-edge/trace/edgeConnector_trace.log` para ver as mensagens de erro.
