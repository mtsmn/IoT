---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-12"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Gerenciamento de risco e segurança
{: #RM_security}

É possível aprimorar a segurança para permitir criar, reforçar e relatar sobre a segurança de conexão de dispositivo. Com essa segurança aprimorada, os certificados e a autenticação de segurança da camada de transporte (TLS) são usados além dos IDs de usuário e tokens usados pelo {{site.data.keyword.iot_short_notm}} para determinar como e onde os dispositivos se conectarão à plataforma. 

## Certificados
{: #certificates}

Quando os certificados estiverem ativados, durante a comunicação entre dispositivos e o servidor, quaisquer dispositivos que não tenham certificados válidos configurados nas configurações de segurança terão o acesso negado, mesmo se usarem IDs de usuário e senhas válidos.

Para configurar certificados e acesso ao servidor para dispositivos, o operador do sistema registra os certificados de autoridade de certificação (CA) associados e registra opcionalmente os certificados do servidor de mensagens na plataforma Watson IoT Platform.
Para configurar os certificados de cliente e o acesso ao servidor para dispositivos, o operador do sistema importa os certificados de autoridade de certificação (CA) associados e os certificados do servidor do sistema de mensagens para o {{site.data.keyword.iot_short_notm}}. O analista de segurança configura, então, as políticas de segurança de conexão para conexões padrão entre os dispositivos e a plataforma. O analista pode incluir diferentes políticas para diferentes tipos de dispositivos.

Para obter informações sobre como configurar certificados, veja [Configurando certificados](set_up_certificates.html).

## Definições de Segurança
{: #connect_policy}

As configurações de segurança, incluindo o uso de configurações de política de segurança, certificados de CA e certificados do servidor de sistema de mensagens, impingem como os dispositivos se conectam à plataforma. É possível configurar políticas de conexão padrão para todos os tipos de dispositivo e criar configurações customizadas para tipos específicos de dispositivo. A política pode ser configurada para permitir conexões não criptografadas, para reforçar conexões Transport Layer Security (TLS) e para permitir que os dispositivos autentiquem com certificados do lado do cliente.

Para obter informações sobre como configurar políticas de segurança de conexão, veja [Configurando políticas de segurança](set_up_policies.html).

As políticas de lista de bloqueio e lista de desbloqueio fornecem a capacidade de controlar os locais dos quais os dispositivos têm permissão para se conectar à conta da organização. Uma lista de bloqueio identifica todos os endereços IP, CIDRs ou países que têm o acesso negado ao servidor. Uma lista de desbloqueio fornece acesso explícito a endereços IP específicos.

Para obter informações sobre como configurar políticas de lista de bloqueio e lista de desbloqueio, veja [Configurando listas de bloqueio e listas de desbloqueio](set_up_policies.html#config_black_white).

## Planos da organização e políticas de segurança
As políticas de segurança aprimoradas permitem que as organizações determinem como desejam que os dispositivos se conectem e sejam autenticados para a plataforma, usando políticas de conexão e políticas de lista de bloqueio e lista de desbloqueio. As opções de política de segurança que estão disponíveis para uma organização dependem do tipo de plano da organização:

**Plano padrão:**
- Os operadores do sistema podem configurar políticas de conexão com as opções a seguir:
    - TLS opcional
    - TLS com Autenticação do Token
    - TLS com Autenticação do Token e Autenticação por Certificado de Cliente

**Advanced Security Plan (ASP) ou Plano Lite:**
- Os operadores do sistema podem configurar políticas de conexão com as opções a seguir:
    - TLS opcional
    - TLS com Autenticação do Token
    - TLS com Autenticação por Certificado de Cliente
    - TLS com Autenticação do Token e Autenticação por Certificado de Cliente
    - TLS com Certificado de cliente ou Token
- Os operadores do sistema podem configurar listas de bloqueio ou listas de desbloqueio

## Painel e relatório de Gerenciamento de risco e segurança
{: #dashboard}

O operador do sistema e o analista de segurança podem usar o painel Gerenciamento de risco e segurança para visualizar o status geral de segurança. Os cartões no painel podem fornecer uma visão geral abrangente da conformidade e o status da conexão de dispositivos.

Os cartões do painel a seguir estão disponíveis para análise de risco e conformidade:
 - **Conformidade de segurança da conexão** - Mostra o nível de conformidade de dispositivos que estão conectados ao servidor.
 - **Conformidade da lista de bloqueio/lista de desbloqueio** - Mostra o nível de conformidade de dispositivos com base na política de lista de bloqueio ou lista de desbloqueio ativa.
 - **Conformidade de política geral** (Beta) - Mostra o nível geral de conformidade com base em todas as políticas que estão em vigor.
 - **Violações de política** (Beta) - Mostra as violações gerais para cada política.

**Importante**: os cartões **Conformidade de política geral** e **Violações de política** estão disponíveis somente como parte de um programa Beta limitado. Para ativar esses cartões do painel, ative os **Recursos experimentais** na página **Configurações**.

### Relatório de política de drill down (Beta)
{: #drill_down}

**Importante**: o recurso de relatório de drill down para políticas de Gerenciamento de risco está disponível somente como parte de um programa Beta limitado. Para ativar o relatório de drill down, ative os **Recursos experimentais** na página **Configurações**.

Como parte do recurso Beta, o operador do sistema pode realizar drill down em cada relatório para visualizar os detalhes específicos dos dispositivos que são compatíveis ou que estão em violação de políticas.

É possível acessar relatórios de drill down por meio dos cartões do painel na página **Políticas** na qual todas as políticas de segurança são listadas e por meio da página de detalhes para cada política. É possível realizar drill down para três níveis de detalhes nos relatórios:
1. O primeiro nível lista todas as políticas que estão contidas no tipo de política selecionado, por exemplo, a política de conexão padrão e quaisquer políticas de conexão customizadas.
2. O segundo nível mostra os dispositivos que estão em violação da política e a causa da violação.
3. O terceiro nível mostra os detalhes específicos de um dispositivo individual que está em violação da política.
