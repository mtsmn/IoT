---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-08"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Visualizando dados em tempo real usando placas e cartões
{: #boards_and_cards}

Crie placas e cartões para criar e compartilhar seus próprios painéis que visualizam os dados de seu dispositivo de tempo real.
{:shortdesc}

Usando placas e cartões, é possível visualizar graficamente valores do conjunto de dados de um ou mais dispositivos para fornecer uma visão geral rápida e aprimorar o entendimento dos dados. Crie placas e inclua cartões que exibam os dados como números brutos, gráficos em tempo real, medidores e mais. Inclua membros em suas placas para compartilhá-los com outros usuários em sua organização. Organize os cartões e inclua divisórias de texto explicativo para fazer um ajuste fino de sua apresentação.  

É possível também expandir o conjunto padrão de cartões [criando seus próprios cartões customizados](custom_cards/custom-cards.html).

![Mostrando dados em tempo real com cartões.](images/boards_and_cards.svg "Mostrando dados em tempo real com cartões.")

## Placas padrão
{: #default_boards}
O painel do {{site.data.keyword.iot_full}} têm as placas padrão a seguir:

|Nome do Painel | Descrição | Cartões incluídos
|:---|:---|:---|  
|Visão geral da utilização  | As estatísticas de uso para sua organização. Lista os tipos de dispositivo e os dados que são consumidos. | <ul><li>Tipos de dispositivos<li>Dados transferidos</ul>
|Análise de dados central da regra | As regras para sua organização. Cartões adicionais listam alertas acionados, dispositivos associados, propriedades dos dispositivos e informações de alerta. | <ul><li>Regras gerenciadas por mim<li>Alertas de regra<li>Informações de alerta de regra<li>Dispositivos Associados<li>Informações do Dispositivo<li>Propriedades do Dispositivo</ul>  
|Análise de dados central do dispositivo | Os dispositivos que estão conectados à sua organização. Cartões adicionais mostram alertas para um dispositivo selecionado, informações para um dispositivo selecionado, propriedades do dispositivo e informações de alerta. | <ul><li>Dispositivos com os quais eu me preocupo<li>Informações do Dispositivo<li>Alertas de regra para esse dispositivo<li>Informações de alerta de regra<li>Propriedades do Dispositivo</ul>
|Visão geral de risco e segurança (beta) | O status de segurança geral de sua organização. Os operadores do sistema e os analistas de segurança podem visualizar detalhes de conformidade, status de conexão para dispositivos, as causas das falhas de conexão e os dispositivos que estão bloqueados ou permitidos por uma lista de bloqueio ou uma lista de desbloqueio. No cartão Conformidade de conexão, os usuários podem realizar drill down para um relatório detalhado sobre dispositivos fora de conformidade e podem exportar o relatório para o Excel. | <ul><li>Conformidade de Políticas<li>Segurança da Conexão<li>Conformidade de lista de bloqueio/lista de desbloqueio</ul>

É possível atualizar essas placas incluindo, atualizando e removendo cartões.

Para reconfigurar uma placa padrão para seu estado original, é possível excluí-la. A placa será então recriada com os cartões originais.
{: tip}

## Criando placas e cartões
{: #visualizing_data}

O {{site.data.keyword.iot_short_notm}} fornece um painel integrado que é possível usar para exibir os dados em tempo real que seu dispositivo está retornando. Por padrão, a página Visão geral exibe informações de uso sobre o sua organização do {{site.data.keyword.iot_short_notm}}, como dados e o espaço de armazenamento consumido. Para ver os dados do dispositivo de tempo real conforme fluem para dentro, inclua cartões específicos do dispositivo nesta página.

Para uma orientação passo a passo sobre como exibir dados do dispositivo de tempo real, veja a orientação [Configurando placas e cartões no novo painel do Watson IoT ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/configuring-the-cards-in-the-new-watson-iot-dashboard/){: new_window}.
{: tip}

Para criar uma placa e incluir um cartão para essa placa:
1. No painel do {{site.data.keyword.iot_short_notm}}, selecione **Placas**.
2. Selecione uma placa para a qual você tem direitos de edição ou crie uma placa nova.
3. Na placa, clique em **Incluir novo cartão**.
3. Selecione um tipo de cartão.  
**Dica:** se você não tiver certeza de qual visualização escolher para um cartão do tipo dispositivos, selecione **Visualização genérica**. É possível mudar o tipo de cartão posteriormente.
<dl>
<dt>Dispositivos</dt>
<dd><table>
<thead>
<tr>
<th>Tipo</th>
<th>Dados exibidos</th>
</tr>
</thead>
<tbody>
<tr>
<td>Visualização genérica</td>
<td>O valor de um ou mais conjuntos de dados. </br>**Dica:** para ver até três valores de pontos de dados em uma tabela pequena, escolha o tamanho de widget grande. </td>
</tr>
<tr>
<td>Gráfico de Linhas</td>
<td>Um ou mais conjuntos de dados em um gráfico de rolagem em tempo real. Use o menu Configurações para configurar o intervalo de dados e a retenção, a aparência dos gráficos e mais. </td>
<tr>
<td>gráfico de barras</td>
<td>Valores do conjunto de dados em barras rotuladas. Use o menu Configurações para alternar a direção da barra horizontal ou vertical.</td>
</tr>
<tr>
<td>Gráfico de rosca</td>
<td>Dois ou mais conjuntos de dados em uma representação circular.</td>
</tr>
<tr>
<td>Value</td>
<td>O valor bruto de um ou mais conjuntos de dados.</td>
</tr>
<tr>
<td>Medidor</td>
<td>O valor de um conjunto de dados mostrado como um medidor. Use o menu Configurações para opcionalmente configurar limites do medidor para intervalos de dados inferior, intermediário e superior.  </td>
</tr>
<tr>
<td>Propriedades do Dispositivo</td>
<td>Propriedades específicas para um ou mais dispositivos.</td>
</tr>
<tr>
<td>Todas as propriedades do dispositivo</td>
<td>Todas as propriedades para um ou mais dispositivos.</td>
</tr>
<tr>
<td>Lista de dispositivos</td>
<td>Uma lista para monitorar vários dispositivos. Uma lista pode ser usada como uma origem de dados para outros cartões. </br>É possível filtrar listas por ID do dispositivo e o tipo nas configurações do cartão. As listas de dispositivos de tamanho L ou maior também podem ser filtradas de forma interativa clicando no ícone de filtro no cartão. As entradas de filtro podem ser incluídas como entradas únicas, intervalos (x-y) ou separadas por vírgula.</br> Por padrão, uma lista exibe o ID do dispositivo e o tipo. É possível definir as configurações de cartão de lista para que o cartão também exiba outros metadados do dispositivo.</td>
</tr>
<tr>
<td>Informações do dispositivo</td>
<td>Informações básicas para um único dispositivo.</td>
<tr>
<td>Mapa de dispositivos</td>
<td>O local de dispositivos em uma lista de dispositivos.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Analytics</dt>
<dd>
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Dados exibidos</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rules</td>
<td>Uma lista das regras que têm alertas.</td>
</tr>
<tr>
<td>Alertas de regra</td>
<td>Uma lista de alertas para um dispositivo.</td>
</tr>
<tr>
<td>Informações de alerta</td>
<td>Informações básicas para um único alerta.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Gerenciamento de risco (beta)</dt>
<dd>Disponível somente com [Segurança avançada](reference/security/RM_security.html).
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Dados exibidos</th>
</tr>
</thead>
<tbody>
<tr>
<td>Conformidade de Políticas</td>
<td>Uma visão geral de segurança de conexão e de dispositivos incluídos na lista de bloqueio e na lista de desbloqueio.</td>
</tr>
<tr>
<td>Conformidade de lista de bloqueio/lista de desbloqueio</td>
<td>O número de dispositivos incluídos na lista de bloqueio ou na lista de desbloqueio.</td>
</tr>
<tr>
<td>Segurança da Conexão</td>
<td>O número de dispositivos que falharam na verificação de segurança de conexão.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Uso</dt>
<dd>
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Dados exibidos</th>
</tr>
</thead>
<tbody>
<tr>
<td>Tipos de dispositivos</td>
<td>Um gráfico de pizza que exibe o número de dispositivos registrados por tipo de dispositivo para sua organização.</td>
</tr><tr>
<td>Dados transferidos</td>
<td>Estatísticas de uso para dados transferidos para sua organização.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Básica</dt>
<dd>
<table>
<thead>
<tr>
<th>Tipo</th>
<th>Dados exibidos</th>
</tr>
</thead>
<tbody>
<tr>
<td>Separator</td>
<td>Um separador horizontal para estruturar e agrupar cartões na placa.</td>
</tr>
</tbody>
</table>
</dd>
</dl>

4.	Selecione os dados de origem do cartão.  
Selecione uma ou mais origens de dados do cartão e, em seguida, clique em **Avançar**.  
As origens de dados podem ser dispositivos únicos registrados ou outros cartões. Para usar uma origem de dados do cartão, um cartão de lista ou mapa deve existir na placa.  
5. Inclua um ou mais conjuntos de dados para cada uma de suas origens de dados.
 - Dispositivos
    2. Selecione um evento que inclua o ponto de dados que você deseja exibir.
    3.	Selecione a propriedade que representa o ponto de dados.
    1.	Forneça ao conjunto de dados um nome de identificação.
    4.	Configure o tipo, a unidade, a precisão e os valores mínimo e máximo para o ponto de dados.  
    Quando estiver pronto, será possível clicar em **Novo conjunto de dados** para incluir mais conjuntos de dados ou clicar em **Avançar**.
 - As listas
    2. Selecione um tipo de dispositivo ou selecione **Qualquer tipo de dispositivo**.
    2. Selecione um evento que inclua o ponto de dados que você deseja exibir.
    3.	Selecione a propriedade que representa o ponto de dados.
    1.	Forneça ao conjunto de dados um nome de identificação.
    4.	Configure o tipo, a unidade, a precisão e os valores mínimo e máximo para o ponto de dados.  
    Quando estiver pronto, será possível clicar em **Novo conjunto de dados** para incluir mais conjuntos de dados ou clicar em **Avançar**.
5.	Customize a visualização do cartão em visualização prévia do cartão.  
 7. Selecione o tamanho da apresentação.  
Além de configurar o tamanho do cartão em sua placa, a configuração de tamanho do cartão também controla outras variáveis de apresentação, como o número de dispositivos que estão listados, os metadados de gráfico que são exibidos e assim por diante.   
**Dica:** clique nos diferentes rótulos de tamanho para ver visualizações das cartas em tamanhos diferentes.
 8. Defina quaisquer configurações adicionais.  
Se o cartão suportar isso, clique em **Configurações** para ver configurações adicionais que podem ser definidas, como intervalos de dados para cartões do tipo de calibrador e opções de filtragem para cartões de lista de dispositivos.
6. Atualize as informações do cartão.  
 1. Forneça um título e uma descrição para o cartão e, opcionalmente, selecione um esquema de cores.   
 2. Clique em **Enviar** para criar o cartão.
7.	Posicione o novo cartão em sua placa arrastando-o para uma boa localização.  
