---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-23"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Níveis de acesso para funções de aplicativo

As tabelas a seguir mostram o nível de acesso para cada uma das funções de aplicativo.

As tabelas mostram níveis de acesso para:
- [Operações de dispositivo](#app-device-ops)
- [Operações de log](#app-log-ops)
- [Operações de cache](#app-cache-ops)
<!-- [Historian Operations](#app-historian) -->
- [Operações de organização](#app-org-ops)
- [Operações de controle de acesso](#app-access-ops)
<!-- - [Analytics Operations](#app-analytics-ops) -->
- [Operações de terceiros](#app-third-party)  
<!-- - [Risk Management Operations](#app-risk-mgt) -->

## Application Roles
{: #app-roles}

### Operações de Dispositivo {: #app-device-ops}

Operações de Dispositivo ||| Application Roles||||
:--------: | -------------|-------------|---------------|-----|---
           | **Aplicativo padrão** | **Aplicativo de operações** | **Aplicativo confiável de backend** | **Aplicativo de processador de dados** | **Aplicativo de visualização** | **Aplicativo de dispositivo**
Criar, atualizar ou excluir dispositivos|P|P|P|-|-|-
Visualizar dispositivos|P|P|P|P|P|-
Ativar dispositivo|P|P|P|-|-|-
Publicar um evento|P|-|P|-|-|P
Subscrever para um evento|P|P|P|P|P|P
Publicar um comando|P|P|P|P|-|-
Assinar um comando|P|-|P|-|-|P
Iniciar ação de gerenciamento de dispositivo|P|P|-|-|-|-
Visualizar ações de gerenciamento de dispositivo|P|P|-|-|-|P
Limpar ações de gerenciamento de dispositivo|P|P|-|-|-|-
Gerenciar pacotes configuráveis de ação de gerenciamento de dispositivo|P|P|-|-|-|-
Criar, atualizar ou excluir tipos de dispositivo|P|P|P|-|-|-
Visualizar tipos de dispositivo|P|P|P|P|-|-
Gerenciar logs de diagnósticos|P|P|-|-|-|P
Visualizar logs de diagnósticos|P|P|P|-|-|-

### Operações de log {: #app-log-ops}

Operações de log ||| Application Roles||||
:--------: | -------------|-------------|---------------|-----|---
           | **Aplicativo padrão** | **Aplicativo de operações** | **Aplicativo confiável de backend** | **Aplicativo de processador de dados** | **Aplicativo de visualização** | **Aplicativo de dispositivo**
Visualizar logs do servidor|P|P|P|-|-|-

### Operações de cache {: #app-cache-ops}

Operações de cache ||| Application Roles||||
:--------: | -------------|-------------|---------------|-----|---
           | **Aplicativo padrão** | **Aplicativo de operações** | **Aplicativo confiável de backend** | **Aplicativo de processador de dados** | **Aplicativo de visualização** | **Aplicativo de dispositivo**
Visualizar dados ativos (cache de eventos)|P|P|P|P|P|P
Gerenciar dados ativos (cache de eventos)|P|P|P|P|P|P

### Operações de organização {: #app-org-ops}

Operações de organização ||| Application Roles||||
:--------: | -------------|-------------|---------------|-----|---
           | **Aplicativo padrão** | **Aplicativo de operações** | **Aplicativo confiável de backend** | **Aplicativo de processador de dados** | **Aplicativo de visualização** | **Aplicativo de dispositivo**
Configure os parâmetros de armazenamento|-|-|-|-|-|-
Configurar provedor de autenticação|-|-|-|-|-|-
Criar, visualizar, atualizar ou excluir configuração de e-mail|-|-|-|-|-|-
Visualizar provedores de e-mail disponíveis|P|P|-|-|-|-
Criar, visualizar, atualizar ou excluir modelos de correio|P|P|-|-|-|-
Criar, atualizar ou excluir usuários|-|P|-|-|-|-
Visualizar usuários|P|P|-|-|-|-
Criar, atualizar ou excluir convites de usuários|-|P|-|-|-|-
Visualizar convites de usuário|P|P|-|-|-|-
Concluir convite|P|P|-|-|-|-
Criar, atualizar ou excluir chaves API|-|P|-|-|-|-
Visualizar chaves API|P|P|-|-|-|-
Visualizar informações de uso da organização|P|P|-|-|-|-

### Operações de controle de acesso {: #app-access-ops}

Operações de controle de acesso ||| Application Roles||||
:--------: | -------------|-------------|---------------|-----|---
           | **Aplicativo padrão** | **Aplicativo de operações** | **Aplicativo confiável de backend** | **Aplicativo de processador de dados** | **Aplicativo de visualização** | **Aplicativo de dispositivo**
Visualizar propriedades de usuários, incluindo direitos de acesso|P|P|-|-|-|-
Visualizar propriedades próprias dos usuários, incluindo direitos de acesso|-|-|-|-|-|-
Gerenciar usuários, incluindo direitos de acesso|-|P|-|-|-|-
Visualizar propriedades da chave API, incluindo direitos de acesso|P|P|-|-|-|-
Visualizar propriedades próprias da chave API, incluindo direitos de acesso|P|P|P|P|P|P
Criar, atualizar, excluir chaves API, incluindo direitos de acesso|-|P|-|-|-|-
Visualizar propriedades do dispositivo, incluindo direitos de acesso|P|P|P|P|P|-
Visualizar propriedades próprias do dispositivo, incluindo direitos de acesso|-|-|-|-|-|-
Criar, atualizar, excluir dispositivo, incluindo direitos de acesso|P|P|P|-|-|-
Visualizar Funções|P|P|-|-|-|-
Criar, atualizar, excluir funções customizadas|-|P|-|-|-|-
Visualizar operações*|P|P|-|-|-|-

<!-- ### Analytics Operations {: #app-analytics-ops}
Analytics Operations ||| Application Roles||||
           | **Standard Application** | **Operations Application** | **Backend Trusted Application** | **Data Processor Application** | **Visualization Application** | **Device Application**
View analytics rules|X|X|-|X|X|-
Manage analytics rules|X|X|-|X|-|-
View analytics actions|X|X|-|X|X|-
Manage analytics actions|X|X|-|X|X|-
View analytics alerts|X|X|-|X|X|X
View analytics message schemas|X|X|-|X|X|-
Manage analytics message schemas|X|X|-|X|-|- -->

### Operações de serviço de terceiro {: #app-third-party}

Operações de serviço de terceiro ||| Application Roles||||
:--------: | -------------|-------------|---------------|-----|---
           | **Aplicativo padrão** | **Aplicativo de operações** | **Aplicativo confiável de backend** | **Aplicativo de processador de dados** | **Aplicativo de visualização** | **Aplicativo de dispositivo**
Processar notificações em lote da plataforma externa|P|P|-|-|-|-
Processar notificações em lote e enviá-las para a plataforma externa|P|P|-|-|-|-
Publicar um evento para um dispositivo|P|P|-|-|-|-
Assinar eventos de um dispositivo|P|P|-|-|-|-
Configurar uma URL de retorno de chamada para a plataforma externa|P|P|-|-|P|-
Configurar o nível de assinatura da plataforma externa|P|P|-|-|P|-
Obter status de funcionamento do status do conector|P|P|P|-|P|-
Verificar se um sistema externo está operacional e validar credenciais|P|P|P|-|P|-
