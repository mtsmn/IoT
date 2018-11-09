---

copyright:

years: 2017, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Quotas
Esta seção fornece informações sobre os limites que são configurados para serviços
do {{site.data.keyword.iot_full}} por tipo de plano. Os limites fazem parte da
nossa política de uso justo para ajudar a assegurar que o desempenho não seja afetado de
forma adversa por uso indevido para todos os usuários do sistema de vários locatários. Os
limites também ajudam a evitar que a carga de trabalho do usuário real seja
inadvertidamente identificada como ataque de Negação de Serviço.

## Introdução
{: #metrics_about}

Cotas especificam os limites superiores que são configurados para um recurso. Quando
o uso exceder um limite especificado, as solicitações do usuário poderão ser
atrasadas ou negadas. Em circunstâncias excepcionais, limites excedidos que afetam a
estabilidade do sistema podem fazer com que a organização
{{site.data.keyword.iot_short_notm}} seja suspensa para evitar que outros
usuários sejam afetados.

As tabelas a seguir listam os limites do
{{site.data.keyword.iot_short_notm}}. A maioria dos limites especificados é por
organização {{site.data.keyword.iot_short_notm}}, a menos que explicitamente
listado como por dispositivo. Alguns dos limites têm um máximo imposto, enquanto outros
podem ser aumentados mediante solicitação. Se você precisar de limites mais altos do que
os especificados, abra um [chamado
![Ícone de link externo](../../../icons/launch-glyph.svg)](https://support.ng.bluemix.net/gethelp/){: new_window}.

## Sistema de mensagens
{: #messaging_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado     | Plano Dedicado
------------- | -------------|------------- |
Número máximo de tipos de dispositivo |50 |1000 |1000
Número máximo de dispositivos conectados simultaneamente | 500| 500K. Se você desejar
conectar mais de 50.000 dispositivos, entre em contato para discutir seus planos. | 500 K
Número máximo de dispositivos registrados |500 | 1 milhão | 100K por incremento
Máximo ['A' Aplicativos - compartilhados](../applications/mqtt.html#scalable_apps) |1 | 10 assinaturas compartilhadas, cada uma com 50 instâncias|10 assinaturas compartilhadas, cada uma com 50 instâncias
Máximo ['a' aplicativos - não compartilhados](../applications/mqtt.html#client_connections) |500 chaves API | 2.000 chaves API | 2.000 chaves API
Máximo de dados por mês | 200 MB | ilimitada |ilimitada
Conexões de dispositivo|10/s | 300/s |300/s
Envios de dispositivo para nuvem (MQTT) - Dispositivo | 5 msg/s ou 20 KB/s | 10 msg/s ou 40 KB/s |10 msg/s ou 40 KB/s
Envios de dispositivo para nuvem (MQTT) - Gateway  | 50 msg/s ou 200 KB/s | 100 msg/s ou 400 KB/s| 100 msg/s ou 400 KB/s
Envios de dispositivo para nuvem (MQTT) - Aplicativo | 10 msg/s ou 40 KB/s | 20 msg/s ou 80 KB/s| 20 msg/s ou 80 KB/s
Envios de dispositivo para nuvem (MQTT) - por Organização | 5000 msg/s | 6.000 msg/s | 6.000 msg/s
Envios de dispositivo para nuvem (HTTP) - Dispositivo | 30 msg/min ou 2 KB/s | 1 msg/s ou 4 KB/s | 1 msg/s ou 4 KB/s
Envios de dispositivo para nuvem (HTTP) - Gateway | 5 msg/s ou 20 KB/s | 10 msg/s ou 40 KB/s | 10 msg/s ou 40 KB/s
Envios de dispositivo para nuvem (HTTP) - Aplicativo | 1 msg/s ou 4 KB/s| 2 msg/s ou 8 KB/s | 2 msg/s ou 8 KB/s
Envios de dispositivo para nuvem (HTTP) - por Organização | 500 msg/s | 600 msg/s | 600 msg/s
Envios de nuvem para dispositivo (MQTT) - Dispositivo  |1 msg/s |2 msg/s ou 8 KB/s |2 msg/s ou 8 KB/s
Envios de nuvem para dispositivo (MQTT) - Gateway| 10 msg/s |20 msg/s ou 80 KB/s  |20 msg/s ou 80KB/s  
Envios de nuvem para dispositivo (MQTT) - Aplicativo | 10 msg/s |20 msg/s ou 80 KB/s |20 msg/s ou 80 KB/s  
Envios de nuvem para dispositivo (MQTT) - por Organização |1000 msg/s | 12 K msg/s|12 K msg/s
Envios de nuvem para dispositivo (HTTP) - taxa de distribuição por Dispositivo | 30 msg/min| 30 msg/min  | 30 msg/min
Envios de nuvem para dispositivo (HTTP) - por Organização |  1000 msg/s|  1200 msg/s  |  1200 msg/s
Número máximo de mensagens de entrada não reconhecidas - por Dispositivo | 10 |10 |10
Número máximo de mensagens de entrada não reconhecidas - por Gateway | 1000 |1000 |1000
Número máximo de mensagens de entrada não reconhecidas - por conexão de aplicativo  |2.000 | 2.000|2.000
Número máximo de mensagens de saída não reconhecidas - por Dispositivo |10  |10 |10
Persistência de dispositivo para nuvem | 24 horas|24 horas |24 horas
Persistência de nuvem para dispositivo |48 horas (padrão). Máximo de 7 dias e profundidade de 50 filas| 48 horas (padrão). Máximo de 7 dias e profundidade de 50 filas  |48 horas (padrão). Máximo de 7 dias e profundidade de 50 filas
Intervalo de repetição máxima para entregar mensagens de QoS 1 | Tentar novamente na reconexão |Tentar novamente na reconexão |Tentar novamente na reconexão
Limite de inatividade de conexão (intervalo keep-alive) | Especificado pelo cliente na conexão |Especificado pelo cliente na conexão  |Especificado pelo cliente na conexão
Duração da conexão de WebSocket |Especificado pelo cliente na conexão | Especificado pelo cliente na conexão  |Especificado pelo cliente na conexão
Tamanho máximo da mensagem | 128 KB |128 KB |128 KB
Máximo de assinaturas por dispositivo |50 |50 |50
Máximo de assinaturas por aplicativo | 500 |500 |500
Profundidade da fila - máximo de mensagens em buffer por assinatura por dispositivo | 50 |50 |50
Máximo de mensagens em buffer para aplicativos 'A' |50 K duráveis, 50 K não duráveis |50 K duráveis, 50 K não duráveis |50 K duráveis, 50 K não duráveis


## APIs
{: #api_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Chamadas API |10/s |10/s |10/s

## Mecanismo de regras
{: #rule_engine_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Máximo de regras por Organização |100|1000|1000
Ações por regra|10|10|10

## Device Management
{: #device_mgmt_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Tamanho máximo de logs de diagnóstico por dispositivo |500 K|500 K|500 K
Máximo de versões históricas de logs de diagnóstico mantidas|2  |2 |2
Tempo máximo de logs de diagnóstico mantidos|7 dias| 7 dias|7 dias
Número máximo de ações iniciadas por mensagem em andamento |200 |200 |200
Número máximo de dispositivos por ação |5000 |5000 | 5000

## Último cache de eventos
{: #last_event_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Número máximo de IDs de evento por dispositivo |5|5|5
Número máximo de formatos |2|3|3
Expiração do último cache de eventos |7 dias |45 dias | 45 dias

## Extensões
{: #extensions_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Número máximo de serviços com o qual você pode ligar |12|12|12

## Information Management
{: #information_management_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Profundidade máxima de aninhamento para carga útil JSON |5|5|5
Tamanho máximo da sequência JSON |512|512|512

## Segurança
{: #security_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Número máximo de funções customizadas |20|20|20
Número máximo de membros usuários |1000|1000|1000
Número máximo de dispositivos em um grupo de recursos |300|300|300
Número máximo de grupos de recursos designados a um gateway |10|10|10
Número máximo de grupos de recursos aos quais um dispositivo pode pertencer |10|10|10

## Interface com o usuário
{: #UI_metrics}

de Métrica        | Plano Lite      | Planos de segurança padrão e avançado       | Dedicado
------------- | -------------|------------- |
Número máximo de painéis |50|50|50
Número máximo de cartões na placa |30|30|30
