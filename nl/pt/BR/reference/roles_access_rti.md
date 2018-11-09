---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Níveis de acesso para funções de usuário

As tabelas a seguir mostram o nível de acesso para cada uma das funções de usuário padrão.

As tabelas mostram níveis de acesso para:
- [Operações de dispositivo](#user-device-ops)
- [Operações de log](#user-log-ops)
- [Operações de cache](#user-cache-ops)
<!-- [Historian Operations](#user-historian) -->
- [Operações de organização](#user-org-ops)
- [Operações de controle de acesso](#user-access-ops)
- [Operações de análise de dados](#user-analytics-ops)
- [Operações de terceiros](#user-third-party)  
<!-- - [Risk Management Operations](#user-risk-mgt) -->

## Permissões de Função de Usuário {: #user-roles}

### Operações de Dispositivo {: #user-device-ops}

Operações de Dispositivo ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Criar, atualizar ou excluir dispositivos | P | P | P | - | -
Visualizar dispositivos | P | P | P | P | P
Ativar dispositivo | P | P | P | - | -
Publicar um evento | - | - | - | - | -
Subscrever para um evento | P | P | P | P | P
Publicar um comando | P | P | P | - | -
Assinar um comando | - | - | - | - | -
Iniciar ação de gerenciamento de dispositivo | P | P | P | - | -
Visualizar ações de gerenciamento de dispositivo | P | P | P | P | P
Limpar ações de gerenciamento de dispositivo | P | P | P | - | -
Gerenciar pacotes configuráveis de ação de gerenciamento de dispositivo | P | P | P | - | -
Criar, atualizar ou excluir tipos de dispositivo | P | P | P | - | -
Visualizar tipos de dispositivo | P | P | P | P | P
Gerenciar logs de diagnósticos | P | P | P | - | -
Visualizar logs de diagnósticos | P | P | P | - | -

### Operações de log {: #user-log-ops}

Operações de log ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Visualizar logs do servidor | P | P | P | P | P

### Operações de cache {: #user-cache-ops}

Operações de cache ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Visualizar dados ativos (cache de eventos) | P | P | P | P | P
Gerenciar dados ativos (cache de eventos) | X	| P | P |	X	| -

### Operações de organização {: #user-org-ops}

Operações de organização ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Configure os parâmetros de armazenamento|	P| - |-|-|-				
Configurar provedor de autenticação|	P|-|-|-|-				
Criar, visualizar, atualizar, excluir configuração de e-mail	|P|-|-|-|-				
Visualizar provedores de e-mail IoTP disponíveis	|P|	P|-|-|-			
Criar, visualizar, atualizar, excluir modelos de correio	|X	|X	|-|-|-		
Criar, atualizar, excluir usuários	|P|	P|-|-|-			
Visualizar usuários	|P|	P|	P|	P|-
Criar, atualizar, excluir convites de usuário|	X	|X	| -|-|-		
Visualizar convites de usuário	|X	|X	|- |- |-		
Concluir convite	|P|	P|	P|	P|	P
Criar, atualizar, excluir chaves API	|X	|X	| -|-|-		
Visualizar chaves API	|X	|X	|- |- |-		
Visualizar informações de uso da ORG	|X	|X	| -|-|-		

### Operações de controle de acesso {: #user-access-ops}

Operações de controle de acesso ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Visualizar propriedades de usuários, incluindo direitos de acesso	|P|	P|	P|	P| -
Visualizar propriedades próprias dos usuários, incluindo direitos de acesso	|P|	P|	P|	P|	P
Gerenciar usuários, incluindo direitos de acesso	|X	|X	|-|-|-		
Visualizar propriedades da chave API, incluindo direitos de acesso|	P|	P|	P|	P|-
Visualizar propriedades próprias da chave API, incluindo direitos de acesso	|-|	-|	-| -| -		
Criar, atualizar, excluir a chave API, incluindo direitos de acesso	|X	|X	|-|-|-		
Visualizar propriedades do dispositivo, incluindo direitos de acesso	|P|	P|	P|	P|	P
Visualizar propriedades próprias do dispositivo, incluindo direitos de acesso	|-	|- |- |- |-
Criar, atualizar, excluir dispositivo, incluindo direitos de acesso	|P|	P|	P|	-| -
Visualizar funções	|X	|X	|X	|X	|P
Criar, atualizar, excluir funções customizadas	|X	|P |- |- |-
Visualizar operações*	|X	|X	|X	|X	|P

### Operações de análise de dados {: #user-analytics-ops}

Operações de análise de dados ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Visualizar regras de análise de dados|	P|	P|	P|	P|	P
Gerenciar regras de analítica|	P|	P|	P|	P| -
Visualizar ações de analítica|	P|	P|	P|	P|	P
Gerenciar ações de analítica|	P|	P|	P|	P| -
Visualizar alertas de analítica|	P|	P|	P|	P|	P
Visualizar esquemas de mensagens de analítica|	P|	P|	P|	P|	P
Gerenciar esquemas de mensagens de analítica|	P|	P|	P|	P| -

### Operações de serviço de terceiro {: #user-third-party}

Operações de serviço de terceiro ||| Papéis do usuário|||
:--------: | -------------|-------------|---------------|-----|---
           | **Administrador** | **Operador** | **Desenvolvedor** | **Analista** | **Leitor**
Processar notificações em lote da plataforma externa	|P|	X	|P |-|-
Processar notificações em lote e enviá-las para a plataforma externa	|P|	X	|P| -| -		
Publicar um evento para um dispositivo	|P|	X	|P|	- |-
Assinar eventos de um dispositivo	|X	|X	|P |-| -		
Configurar uma URL de retorno de chamada para a plataforma externa	|X	|X	|P|	-| -
Configurar o nível de assinatura da plataforma externa|	P|	P|	P |- |-		
Obter status de funcionamento do status do conector	|P|	X	|X	|- |-
Verificar se um sistema externo está operacional e validar credenciais	|X	|P|	X	|- |-
