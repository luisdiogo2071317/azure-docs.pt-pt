---
title: Adicionar fornecedores de identidade do OpenID Connect em políticas incorporadas no Azure Active Directory B2C | Documentos da Microsoft
description: Guia de visão geral sobre como adicionar fornecedores de ligação do OpenID em políticas incorporadas no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e5baf95cd2426c9753620cba7c5a67b4c1672788
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444230"
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>O Azure Active Directory B2C: Adicionar um fornecedor de identidade OpenID Connect personalizado em políticas incorporadas

>[!NOTE]
> Esta funcionalidade está em pré-visualização pública. Não utilize a funcionalidade em ambientes de produção.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) é um protocolo de autenticação, criado com base no OAuth 2.0, que pode ser utilizado para assinar com segurança os utilizadores. A maioria dos fornecedores de identidade que utilizam esse protocolo, como [do Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md), são suportadas no Azure AD B2C. Este artigo explica como pode adicionar fornecedores de identidade OpenID Connect personalizados para as políticas incorporadas.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Configurar um fornecedor de identidade OpenID Connect personalizado

Para adicionar um fornecedor de identidade OpenID Connect personalizado:

1. Siga estes passos para [navegue para as definições do Azure AD B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
1. Clique em **fornecedores de identidade**.
1. Clique em **Adicionar**.
1. Para o **tipo de fornecedor de identidade**, selecione **OpenID Connect**.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Como configurar o fornecedor de identidade OpenID Connect

#### <a name="metadata-url"></a>URL de Metadados

De acordo com a especificação, todos os fornecedores de identidade OpenID Connect descreve um documento de metadados que contém a maior parte das informações necessárias para executar o início de sessão. Isto inclui informações como os URLs para utilizar e a localização das chaves de assinatura pública do serviço. O documento de metadados OpenID Connect sempre está localizado num ponto final que termina em `.well-known\openid-configuration`.

Para o fornecedor de identidade OpenID Connect que deseja adicionar, introduza o URL de metadados.

#### <a name="client-id-and-secret"></a>ID de cliente e segredo

Para permitir que os utilizadores iniciem sessão, o fornecedor de identidade irá exigir que os desenvolvedores registar uma aplicação no seu serviço. Esta aplicação terá um ID (referido como o **ID de cliente**) e um **segredo do cliente**. Copie estes valores a partir do fornecedor de identidade e insira-os nos campos correspondentes.

> [!NOTE]
> O segredo do cliente é opcional. No entanto, tem de introduzir um segredo do cliente se pretender utilizar o [fluxo de código de autorização](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), que utiliza o segredo para trocar o código para o token.

#### <a name="scope"></a>Âmbito

Âmbitos de definem as informações e as permissões que quer para recolher através do seu fornecedor de identidade personalizada. Pedidos de ligação OpenID tem de conter o `openid` analisar o valor para receber o token de ID do fornecedor de identidade. Sem o token de ID, os utilizadores não será possível iniciar sessão para o Azure AD B2C usando o provedor de identidade personalizada.

Outros âmbitos podem ser anexados (separados por espaço). Consulte a documentação do fornecedor de identidade personalizado para ver o que podem ser outros âmbitos disponíveis.

#### <a name="response-type"></a>Tipo de resposta

O tipo de resposta descreve que tipo de informações será enviado de volta na chamada inicial para o `authorization_endpoint` do fornecedor de identidade personalizada. 

* `code`:, De acordo a [fluxo de código de autorização](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), será devolvido um código de volta para o Azure AD B2C. O Azure AD B2C irá prosseguir, em seguida, chamar o `token_endpoint` para trocar o código para o token.
* `token`: Um token de acesso será retornado para o Azure AD B2C do fornecedor de identidade personalizada.
* `id_token`: Um token de ID será retornado para o Azure AD B2C do fornecedor de identidade personalizada.


#### <a name="response-mode"></a>Modo de resposta

O modo de resposta define o método que deve ser utilizado para enviar os dados de volta do fornecedor de identidade personalizada para o Azure AD B2C.

* `form_post`: Para melhor segurança, recomenda-se neste modo de resposta. A resposta é transmitida via HTTP `POST` método, com o código ou um token a ser codificada no corpo com o `application/x-www-form-urlencoded` formato.
* `query`: O código ou um token será retornado como um parâmetro de consulta.


#### <a name="domain-hint"></a>Sugestão de domínio

A sugestão de domínio pode ser utilizada para avançar diretamente para o início de sessão na página do fornecedor de identidade especificada, em vez de ter a marca de usuário uma seleção na lista de fornecedores de identidade disponíveis. Para permitir que esse tipo de comportamento, introduza um valor para a sugestão de domínio.

Para ir para o fornecedor de identidade personalizada, acrescentar o parâmetro `domain_hint=<domain hint value>` ao final do seu pedido ao chamar o Azure AD B2C para início de sessão.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Mapeamento de afirmações do fornecedor de identidade de OpenID Connect

Depois de identidade personalizada fornecedor envia um token de ID para o Azure AD B2C, do Azure AD B2C tem de ser capaz de mapear as declarações do token recebido para as afirmações que o Azure AD B2C reconhece e utiliza. 

Para cada um dos mapeamentos abaixo, consulte a documentação do fornecedor de identidade personalizada para compreender as afirmações que são devolvidas em tokens do fornecedor de identidade.

* `User ID`: Introduza a afirmação que fornece o identificador exclusivo para o utilizador com sessão iniciada.
* `Display Name`: Introduza a afirmação que fornece o nome a apresentar ou o nome completo do utilizador.
* `Given Name`: Introduza a afirmação que fornece o nome do utilizador.
* `Surname`: Introduza a afirmação que fornece o apelido do utilizador.
* `Email`: Introduza a afirmação que fornece o endereço de e-mail do utilizador.

## <a name="next-steps"></a>Passos Seguintes

Adicionar personalizado OpenID Connect fornecedor de identidade para sua [política incorporada](active-directory-b2c-reference-policies.md).
