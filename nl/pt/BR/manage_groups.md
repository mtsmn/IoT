---

copyright:
years: 2018
lastupdated: "2018-04-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gerenciando grupos (Beta)
{: #groups_overview}

É possível usar grupos do {{site.data.keyword.iot_full}} para conceder acesso aos membros e às chaves API o acesso a dispositivos específicos. Depois de criar um grupo e incluir dispositivos nele, inclua membros e chaves API e designe a eles funções dentro do grupo. A combinação de funções e grupos determina quais dispositivos os usuários e as chaves API podem acessar e as ações que eles podem executar nos dispositivos.
{: shortdesc}

É possível gerenciar grupos usando a interface com o usuário do painel do
{{site.data.keyword.iot_short_notm}} ou usando as APIs de controle de acesso
do {{site.data.keyword.iot_short_notm}}.

**Importante:** o recurso de grupos na UI do {{site.data.keyword.iot_short_notm}} está disponível somente como parte de um
programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Para obter mais informações sobre o controle de acesso e os grupos e para obter instruções
sobre como usar as APIs de controle de acesso do {{site.data.keyword.iot_short_notm}} para gerenciar grupos, veja [Visão geral do controle de acesso no nível do recurso](reference/rlac_overview.html#RLAC_overview).

## Incluindo grupos

**Nota:** para começar a incluir grupos, ative o recurso
**Grupos** na página **Configurações**. 

1. Em seu painel do {{site.data.keyword.iot_short_notm}}, selecione ** Gerenciamento de acesso** na barra de navegação à esquerda.
2. Selecione a guia **Grupos** e visualize a lista de grupos.
3. Clique em **Incluir grupo**.
4. Na janela **Incluir Grupo**, insira o nome do grupo e uma
descrição opcional.
5. Clique em **Avançar**.
6. Clique em **Incluir dispositivos no grupo**.
7. Selecione os dispositivos que você deseja incluir no grupo e, em seguida, clique
em **Pronto**.
8. Clique em **Concluir** para criar o grupo.
É possível editar o grupo, incluindo ou removendo dispositivos, bem como incluir mais grupos.

