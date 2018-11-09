---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Níveis de acesso para funções de gateway

As tabelas a seguir mostram o nível de acesso para cada uma das funções de gateway.

As tabelas mostram níveis de acesso para:
- [Operações de dispositivo](#gateway-device-ops)
- [Operações de log](#gateway-log-ops)
- [Operações de cache](#gateway-cache-ops)
<!-- [Historian Operations](#gateway-historian) -->
- [Operações de organização](#gateway-org-ops)
- [Operações de controle de acesso](#gateway-access-ops)
<!-- - [Analytics Operations](#gateway-analytics-ops) -->
- [Operações de terceiros](#gateway-third-party)  
<!-- - [Risk Management Operations](#gateway-risk-mgt) -->

## Funções de gateway
{: #gateway-roles}

### Operações de Dispositivo {: #gateway-device-ops}

Operações de Dispositivo || Funções de gateway|
:--------: | ---------------------|------------------------
           | **Gateway Padrão** | **Gateway privilegiado**
Criar, atualizar ou excluir dispositivos|-|P
Visualizar dispositivos|P|P
Ativar dispositivo|-|P
Publicar um evento|P|P
Subscrever para um evento|-|-
Publicar um comando|-|-
Assinar um comando|P|P
Iniciar ação de gerenciamento de dispositivo|P|P
Visualizar ações de gerenciamento de dispositivo|P|P
Limpar ações de gerenciamento de dispositivo|-|-
Gerenciar pacotes configuráveis de ação de gerenciamento de dispositivo|-|P
Criar, atualizar ou excluir tipos de dispositivo|-|-
Visualizar tipos de dispositivo|P|P
Gerenciar logs de diagnósticos|-|-
Visualizar logs de diagnósticos|-|-

### Operações de log {: #gateway-log-ops}

Operações de log || Funções de gateway|
:--------: | ---------------------|------------------------
           | **Gateway Padrão** | **Gateway privilegiado**
Visualizar logs do servidor|-|-

### Operações de cache {: #gateway-cache-ops}

Operações de cache || Funções de gateway|
:--------: | ---------------------|------------------------
           | **Gateway Padrão** | **Gateway privilegiado**
Visualizar dados ativos (cache de eventos)|-|-
Gerenciar dados ativos (cache de eventos)|-|-


### Operações de organização {: #gateway-org-ops}

Operações de organização || Funções de gateway|
:--------: | ---------------------|------------------------
           | **Gateway Padrão** | **Gateway privilegiado**
Configure os parâmetros de armazenamento|-|-
Configurar provedor de autenticação|-|-
Criar, visualizar, atualizar ou excluir configuração de e-mail|-|-
Visualizar provedores de e-mail disponíveis|-|-
Criar, visualizar, atualizar ou excluir modelos de correio|-|-
Criar, atualizar ou excluir usuários|-|-
Visualizar usuários|-|-
Criar, atualizar, excluir convites de usuário|-|-
Visualizar convites de usuário|-|-
Concluir convite|-|-
Criar, atualizar ou excluir chaves API|-|-
Visualizar chaves API|-|-
Visualizar informações de uso da organização|-|-

### Operações de controle de acesso {: #gateway-access-ops}

Operações de controle de acesso || Funções de gateway|
:--------: | ---------------------|------------------------
           | **Gateway Padrão** | **Gateway privilegiado**
Visualizar propriedades de usuários (incluindo direitos de acesso)|-|-
Visualizar propriedades próprias dos usuários (incluindo direitos de acesso)|-|-
Gerenciar usuários (incluindo direitos de acesso|-|-
Visualizar propriedades de chave API (incluindo direitos de acesso)|-|-
Visualizar propriedades próprias da chave API (incluindo direitos de acesso)|-|-
Criar, atualizar ou excluir chaves API (incluindo direitos de acesso)|-|-
Visualizar propriedades do dispositivo (incluindo direitos de acesso)|P|P
Visualizar propriedades próprias do dispositivo (incluindo direitos de acesso)|P|P
Criar, atualizar, excluir dispositivo (incluindo direitos de acesso)|-|P
Visualizar Funções|-|-
Criar, atualizar, excluir funções customizadas|-|-
Visualizar Operações|-|-

<!-- ### Analytics Operations {: #gateway-analytics-ops}
Analytics Operations || Gateway Roles|
           | **Standard Gateway** | **Privileged Gateway** |
View analytics rules|-|-
Manage analytics rules|-|-
View analytics actions|-|-
Manage analytics actions|-|-
View analytics alerts|-|-
View analytics message schemas|-|-
Manage analytics message schemas|-|- -->

### Operações de serviço de terceiro {: #gateway-third-party}

Operações de serviço de terceiro || Funções de gateway|
:--------: | ---------------------|------------------------
           | **Gateway Padrão** | **Gateway privilegiado**
Processar notificações em lote da plataforma externa|-|-
Processar notificações em lote e enviá-las para a plataforma externa|-|-
Publicar um evento para um dispositivo|-|-
Assinar eventos de um dispositivo|-|-
Configurar uma URL de retorno de chamada para a plataforma externa|-|-
Configurar nível de assinatura da plataforma externa|-|-
Obter status de funcionamento do status do conector|-|-
Verificar se um sistema externo está operacional e validar credenciais|-|-
