---
title: Notas do programador sobre como utilizar políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Notas para os desenvolvedores de configuração e manutenção do Azure AD B2C com as políticas personalizadas.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 0a5255974c7399f9307d8b06a58f4f977be89829
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55172889"
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Notas de versão para a pré-visualização pública do Azure Active Directory B2C política personalizada
O conjunto de recursos de política personalizada agora está disponível para avaliação em pré-visualização pública para todos os Azure Active Directory B2C (Azure AD B2C) de clientes. Esse conjunto de recursos é direcionado para programadores de identidades avançada a criar soluções de identidade mais complexas.  

Hoje, esse conjunto de recursos requer que os desenvolvedores o configurem o Framework de experiência de identidade diretamente através da edição de arquivos XML. Esse método de configuração é complexa e poderosa. Avançada de identidade, os desenvolvedores que usam o Framework de experiência de identidade devem planejar para investir algum tempo a concluir as instruções passo a passo e ler os documentos de referência. 

## <a name="features-included-in-this-public-preview"></a>Funcionalidades incluídas nesta pré-visualização pública
Com os novos recursos introduzidos na pré-visualização pública, os desenvolvedores podem realizar as seguintes tarefas:<br>

* A criar e carregar jornadas de utilizador de autenticação personalizado com as políticas personalizadas. 
   * Descreva jornadas de usuário passo a passo como trocas entre fornecedores de afirmações. 
   * Defina a ramificação condicional no jornadas de utilizador. * Integre serviços de ativada para API do REST de mensagens em fila no seu jornadas de utilizador de autenticação personalizado.  
* Adicione federação com fornecedores de identidade que estão em conformidade com o OpenIDConnect padrão. <br>
* Adicione federação com fornecedores de identidade que sigam para o protocolo SAML 2.0. 

## <a name="terms-of-the-public-preview"></a>Termos da pré-visualização pública

* Incentivamos a usar os novos recursos para essas finalidades.<br>
* Os novos recursos não foram concebidos para utilização num ambiente de produção.<br>
* Contratos de nível de serviço (SLAs) não se aplicam aos novos recursos. <br>
* Pedidos de suporte podem ser arquivados por meio de canais de suporte regular. <br>
* Não há nenhuma data promised para disponibilidade geral.<br>
* À nossa escolha e, por qualquer motivo, a Microsoft pode sinalizar e rejeitar ou restringir cenários e jornadas de utilizador que excedem o âmbito da lista de produtos do Azure AD B2C para servir como uma plataforma de gestão (CIAM) de identidades e acessos dos clientes.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Responsabilidades de desenvolvedores do conjunto de funcionalidades de política personalizada
Configuração da política manual concede acesso de nível inferior na plataforma subjacente do Azure AD B2C e resulta na criação de uma estrutura de confiança exclusivo e totalmente personalizável. As combinações possíveis de fornecedores de identidade personalizada, relações de confiança, integrações com serviços externos e fluxos de trabalho passo a passo atribua maior demandas os desenvolvedores avançados consumi-las.

Para se beneficiar totalmente da pré-visualização pública, sugerimos que os desenvolvedores consumindo o conjunto de recursos de política personalizada seguir as diretrizes seguintes:
* Familiarize-se com a linguagem de configuração da arquitetura de experiências de identidade e gestão de chaves/segredos.
* Assumir a propriedade dos cenários e integrações personalizadas.
* Realize os testes de cenário metódico.
* Siga o desenvolvimento de software e melhores práticas com um mínimo de um ambiente de teste e desenvolvimento e um ambiente de produção de teste.
* Mantenha-se informado sobre novos desenvolvimentos do fornecedores de identidade e integrar com os serviços. Por exemplo, mantenha um registo de alterações no segredos e das alterações programadas e não programadas para o serviço.
* Configurar a monitorização de Active Directory e monitorize a capacidade de resposta de ambientes de produção.
* Manter os endereços de correio eletrónico de contacto atualizados na subscrição do Azure e permaneça responsivo ao mensagens de e-mail de equipa do site em direto da Microsoft.
* Tome medidas atempada somente quando aconselhado para fazê-lo pela equipe de sites ativos da Microsoft. 

## <a name="features-by-stage-and-known-issues"></a>Funcionalidades ao estágio e problemas conhecidos
Recursos personalizados da arquitetura de experiências de identidade/política estão em desenvolvimento de constante e rápido.  Esta tabela é um índice de disponibilidade de recursos/componentes.

Poste perguntas no Stack Overflow em [https://aka.ms/aadb2cso](https://aka.ms/aadb2cso)


### <a name="identity-providers-tokens-protocols"></a>Protocolos de fornecedores, Tokens, de identidade
Interfaces com aplicativos e componentes externos

| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|---------------------------------------------|-------------|---------|----|-------|
| IDP-OpenIDConnect |  | x |  | Por exemplo, Google + |
| IDP OAUTH2 |  | x |  | Por exemplo, o Facebook  |
| IDP-OAUTH1 |  | x |  | Por exemplo, do Twitter |
| IDP-SAML |  | x |  | Por exemplo, Salesforce, AD FS |
| IDP-WSFED | x |  |  |  |
| Da entidade confiadora de terceiros OAUTH |  | x |  |  |
| Da entidade confiadora de terceiros OIDC |  | x |  |  |
| Da entidade confiadora de Party SAML | x |  |  |  |
| Entidade confiadora Party WSFED | x |  |  |  |
| API de REST com autenticação básica e o certificado. |  | x |  | Por exemplo, as funções do Azure |


### <a name="component-support"></a>Suporte de componente


| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|-------------------------------------------|-------------|---------|----|-------|
| Multi-Factor Authentication do Azure |  | x |  |  |
| O Azure Active Directory como o diretório local |  | x |  |  |
| Subsistema de E-Mail do Azure para 2FA |  | x |  |  |
| Suporte a vários idiomas|  | x |  |  |
| Complexidade da palavra-passe | x |  |  |  |


### <a name="content-definition"></a>Definição de conteúdo

| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Página de erro, api.error |  | x |  |  |
|   Página de seleção de IDP, api.idpselections |  | x |  |  |
|   Seleção do IDP para a inscrição, api.idpselections.signup |  | x |  |  |
|   Esqueci-me da palavra-passe, api.localaccountpasswordreset |  | x |  |  |
|   Conta local. o início de sessão, api.localaccountsignin |  | x |  |  |
|   Api.localaccountsignup inscrição, da conta local |  | x |  |  |
|   Página MFA, api.phonefactor |  | x |  |  |
|   Autodeclarativas-por exemplo api.selfasserted inscrição, da conta de redes sociais |  | x |  |  |
|   Autodeclarativas atualização de perfil, api.selfasserted.profileupdate |  | x |  |  |
|   A unificação de inscrição ou início de sessão da página, api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>Integração de aplicações-IEF
| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Id_token_hint de parâmetro de cadeia de caracteres de consulta | x |  |  |  |
| Query string parameter domain_hint |  | x |  | disponível como afirmação, pode ser passado para o IDP |
| Login_hint de parâmetro de cadeia de caracteres de consulta |  | x |  | disponível como afirmação, pode ser passado para o IDP |
| Inserir JSON UserJourney via client_assertion | x |  |  | vão ser preteridos |
| Inserir JSON UserJourney como id_token_hint | x |  |  | abordagem de go-forward passar JSON |


### <a name="session-management"></a>Gerenciamento de sessões

| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|---------------------------------|-------------|---------|----|-------|
| Fornecedor de sessões SSO |  | x |  |  |
| Fornecedor de sessão de início de sessão externo |  | x |  |  |
| Fornecedor de sessão de SAML SSO |  | x |  |  |


### <a name="security"></a>Segurança
| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|---------------------------------------------|-------------|---------|----|-------|
| Chaves de política - gerar, Manual, carregamento |  | x |  |  |
| Política de chaves - RSA/Cert, segredos |  | x |  |  |


### <a name="developer-interface"></a>Interface do desenvolvedor
| Funcionalidade | Desenvolvimento | Pré-visualização | GA | Notas |
|---------------------------------------------|-------------|---------|----|-------|
| Portal do Azure-IEF UX |  | x |  |  |
| Registos de UserJourney do Application Insights  |  | x |  |  |
| Registos de eventos do Application Insights |x|  |  |  |



## <a name="next-steps"></a>Passos Seguintes
[Introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md).
