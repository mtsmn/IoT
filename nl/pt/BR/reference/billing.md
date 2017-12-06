---
copyright:
  years: 2016, 2017
lastupdated: "2017-09-06"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Faturamento

Os planos de serviços pagos do {{site.data.keyword.iot_full}} (diferentes
de 'Lite') se baseiam no conceito de megabytes trocados e analisados no período de um mês. Esse
documento detalha como o {{site.data.keyword.iot_short_notm}} mede os dados para
criar informações de uso que determinem o custo do uso do serviço. As informações de uso
podem ser utilizadas para aproximar o custo do uso do
{{site.data.keyword.iot_short_notm}} com base no design e no número de
dispositivos, aplicativos e gateways.

Para obter informações sobre o custo de cada megabyte de dados trocados ou
analisados, consulte o serviço do {{site.data.keyword.iot_short_notm}} no catálogo do Bluemix para a região necessária.

É possível usar a
[Calculadora de
preço![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link
externo")](http://iot-cost-calculator.ng.bluemix.net/) para ajudar a calcular o custo de
um serviço do {{site.data.keyword.iot_short_notm}}.

Os três itens a seguir são medidos separadamente e faturados de acordo com o uso: 
- dados trocados
- dados analisados
- dados de borda analisados

## Dados trocados
O cálculo de *dados trocados* inclui dados da conta que são trocados
por aplicativos, dispositivos e gateways usando o sistema de mensagens MQTT ou HTTP, bem
como os dados trocados por aplicativos usando a API HTTP.

### Dados trocados MQTT
Ao usar MQTT, o conteúdo do fluxo TCP conta para *dados trocados*. A
carga útil de qualquer mensagem MQTT é incluída no acúmulo dos dados que são trocados
usando o protocolo MQTT. O acúmulo do tamanho dos dados de carga útil ocorre quando a
mensagem é publicada, bem como quando a mensagem é entregue a um assinante.

O protocolo MQTT é projetado para ser o mais leve possível. Embora o
{{site.data.keyword.iot_short_notm}} inclua os tamanhos dos pacotes MQTT ao
acumular *dados trocados*, por ser um protocolo leve, essa sobrecarga é
baixa. A tabela a seguir fornece uma indicação do tamanho mínimo de cada pacote MQTT:

|PACOTE MQTT                    |Bytes de tamanho mínimo  |Variáveis e tamanho mínimo associado em bytes|
|-------------------------------|--------------------|-------------------------------------------------|
|CONECTAR                        |56                  |ID de cliente (12 se d:xxxxxx:t:i), tópico (0) e mensagem (0) se configurado, nome do usuário (14 para dispositivos - 'use-token-auth') e senha (18 se gerado automaticamente)|
|CONNACK                        |4                   |Nada|
|PUBLISH                        |21                  |Nome do tópico (17 se iot-2/evt/i/fmt/f) e carga útil (0 é o mínimo). Aplica-se a pacotes PUBLISH de entrada e de saída.|
|PUBACK                         |4                   |Nada|
|PUBREC                         |4                   |Nada|
|PUBREL                         |4                   |Nada|
|PUBCOMP                        |4                   |Nada|
|SUBSCRIBE                      |26                  |Pode incluir vários filtros de tópico (19 se iot-2/evt/i/fmt/f) e QoS (1)|
|SUBACK                         |5                   |(1) para cada filtro de tópicos em SUBSCRIBE|
|UNSUBSCRIBE                    |23                  |Pode incluir vários filtros de tópicos (19 se iot-2/evt/i/fmt/f)|
|UNSUBACK                       |4                   |Nada|
|PINGREQ                        |2                   |Nada|
|PINGRESP                       |2                   |Nada|
|DISCONNECT                     |2                   |Nada|

Ao usar TLS, o handshake seguro também é considerado. O handshake é aproximadamente 8 kilobytes. Portanto, haverá mais efetividade em custo para manter conexões MQTT abertas enquanto possível. As conexões abertas incorrem apenas na sobrecarga de PINGREQ e PINGRESP (4 bytes) por intervalo keep-alive que é específico do cliente e depende do período de keep-alive especificado. Reabrir uma conexão TLS usando uma sessão TLS existente diminui o número de bytes que são trocados porque os certificados não são enviados em ambas as direções.  Muitos clientes TLS suportam isso por padrão, mas a sessão tem um tempo de vida limitado.  Usar os certificados de cliente aumenta o tamanho do handshake TLS, dependendo do tamanho do certificado de cliente. 

Os registros de TLS e as sobrecargas de TCP e IP não são considerados.

**Nota** - Ao usar o painel do
{{site.data.keyword.iot_short_notm}} para visualizar um dispositivo, uma
assinatura MQTT é feita para que o painel possa exibir eventos em tempo real. Essa
assinatura MQTT também conta para a quantidade total de dados trocados.

### Dados trocados do sistema de mensagens HTTP
Ao usar o sistema de mensagens HTTP, a carga útil de mensagem é incluída no acúmulo
de *dados trocados*, ao publicar eventos ou receber comandos.

Como com MQTT, a sobrecarga HTTP é contada. A sobrecarga HTTP é significativamente
maior que a sobrecarga MQTT. A sobrecarga HTTP por solicitação é aproximadamente 300
bytes. O tamanho da carga útil da mensagem também precisa ser incluído.

Ao usar TLS, o handshake seguro é considerado. O handshake é aproximadamente 8 kilobytes.  Portanto,
haverá mais efetividade em custo para manter conexões HTTPS abertas enquanto possível. As
conexões abertas não incorrem em sobrecarga adicional em termos de *dados trocados*.  Reabrir uma conexão TLS usando uma sessão TLS existente diminui o número de bytes que são trocados porque os certificados não são enviados em ambas as direções.  Muitos clientes TLS suportam isso por padrão, mas a sessão tem um tempo de vida limitado.  Usar os certificados de cliente aumenta o tamanho do handshake TLS, dependendo do tamanho do certificado de cliente.

Os registros de TLS e as sobrecargas de TCP e IP não são considerados.

### Dados trocados da API HTTP
Quando qualquer API é chamada por um aplicativo, os corpos de solicitação e
resposta são acumulados para *dados trocados*. Os tamanhos de cada podem variar significativamente dependendo da API.

Ao contrário do sistema de mensagens MQTT e HTTP, nem a sobrecarga HTTP nem a
sobrecarga TLS são consideradas para dados trocados da API HTTP.

**Nota** - Ao usar o painel do
{{site.data.keyword.iot_short_notm}}, as APIs HTTP são usadas pelo painel para
listar informações que incluem dispositivos, tipos de dispositivo e logs de conexão de
dispositivo. Essas chamadas API HTTP contam para *dados trocados*.

## Dados analisados
O cálculo de *dados analisados* mede dados de eventos que são
processados pelo mecanismo de regras dentro da plataforma. Os dados são considerados
processados pelo mecanismo de regras quando eventos de dispositivos são avaliados por uma
ou mais regras, com base em um determinado dispositivo e tipo de evento. 

## Dados de borda analisados
O cálculo de *dados analisados de borda* mede dados de eventos processados em um dispositivo de gateway pelo
{{site.data.keyword.iot_short_notm}} Edge Analytics Agent.  Os dados são
considerados processados pelo agente de borda quando eventos de dispositivos são avaliados
por uma ou mais regras de borda, com base em um determinado dispositivo e tipo de
evento. 
