---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-07"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Simulando dados do dispositivo 
{: #sim_device_data}

Usando o simulador de configurar do {{site.data.keyword.iot_full}}, é
possível configurar eventos simulados para dispositivos. É possível usar os dados do
evento simulados para conhecer, testar e demonstrar os recursos do
{{site.data.keyword.iot_short_notm}} totalmente funcionais.
{: shortdesc}

É possível usar dispositivos e tipos de dispositivo existentes e o simulador
permite gerar novos dispositivos para tipos existentes. Você pode configurar os detalhes
do evento para cada dispositivo ou definir uma configuração padrão que é aplicada a
todos os dispositivos. É possível exportar uma configuração do evento simulada para que
ela possa ser reutilizada ou compartilhada para configurar outras simulações.

Para simular dados do dispositivo: 

1. Efetue login no {{site.data.keyword.iot_short_notm}}.
2. Na área de janela de navegação principal, selecione **Configurações**.
3. Na seção **Recursos experimentais**, ative o simulador de dispositivo.
4. Na área de janela de navegação principal, selecione **Dispositivos**. Uma
mensagem no canto inferior direito da tela indica que nenhuma simulação está em execução.
5. Clique na mensagem "0 simulações em execução".
6. Na janela pop-up **Simulações**, clique em **Incluir primeira simulação**.
7. Selecione o tipo de dispositivo para o qual você deseja simular dados.
8. Selecione uma das seguintes opções:
  - Para incluir um ou mais dispositivos novos, selecione a quantidade e clique em
**Novo dispositivo**. Os novos dispositivos são listados.
  - Para incluir um dispositivo existente, clique em **Usar dispositivo
registrado** e selecione um dispositivo na lista.
  - Para importar uma simulação existente, clique em
**Importar/Exportar**, selecione a guia **Importar** e importar um
arquivo JSON ou copie uma configuração exportada anteriormente da área de transferência.
9. Clique em ![ícone Configurações](images/settings_icon.png) e configure os detalhes de simulação para um tipo de dispositivo:
   1. Selecione o tipo de dispositivo na lista e clique em **+Tipo de
evento** para incluir um evento no tipo de dispositivo.
   2. Selecione um tipo de evento na lista e clique em **Planejar** para configurar a frequência do evento.
   3. Edite os detalhes do tipo de evento no formato JSON e salve o tipo de evento atualizado.
   
   <p> É possível usar as duas variáveis especiais a seguir ao configurar eventos:  
        - `range`: esta variável é substituída por um número
aleatório que está entre os dois valores fornecidos. Por exemplo: `{ "random":range(300, 100) }`  
        - `$counter`: esta variável é substituída pelo número de
dispositivos que são incluídos no simulador para o tipo atual. Por exemplo: `{"total": $counter}`</p>
   {: tip}
   
   4. Escolha se deseja usar essas configurações como valores padrão para todos os dispositivos desse tipo ou usar valores customizados para dispositivos individuais. 
   5. Repita as etapas de configuração para cada dispositivo ao qual você deseja
aplicar configurações customizadas. A mensagem na parte inferior direita da tela mostra o número de simulações ativas.
10. **Opcional:** Depois de configurar simulações para
dispositivos, exporte os detalhes de simulação para que eles possam ser reutilizados ou
compartilhados:
    1. Na janela pop-up **Simulações**, clique em **Importar/Exportar**.
    2. Selecione a guia **Exportar**.
    3. Copie os detalhes da simulação para a área de transferência ou faça download deles em um arquivo JSON.
11. Na página **Dispositivos**, navegue para um dos
dispositivos simulados e selecione a guia **Eventos recentes** para
verificar se os eventos simulados estão funcionando.
