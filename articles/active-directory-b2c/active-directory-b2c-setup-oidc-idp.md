---
title: A adição de fornecedores de identidade OpenID Connect políticas incorporadas no Azure Active Directory B2C | Microsoft Docs
description: Guia de descrição geral sobre como adicionar fornecedores OpenID Connect nas políticas incorporadas no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e4325a7c87ac9975e819b22536838ec438fa1bcd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709568"
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>O Azure Active Directory B2C: Adicionar um fornecedor de identidade OpenID Connect personalizado nas políticas incorporadas

>[!NOTE]
> Esta funcionalidade está em pré-visualização pública. Não utilize a funcionalidade em ambientes de produção.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) é um protocolo de autenticação, desenvolvido com OAuth 2.0, que podem ser utilizados para assinar com segurança os utilizadores. A maioria dos fornecedores de identidade que utilizam este protocolo, tais como [do Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md), são suportadas no Azure AD B2C. Este artigo explica como adicionar fornecedores de identidade OpenID Connect personalizados para as políticas incorporadas.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Configurar um fornecedor de identidade OpenID Connect personalizado

Para adicionar um fornecedor de identidade OpenID Connect personalizado:

1. Siga estes passos para [navegar para as definições do Azure AD B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
1. Clique em **fornecedores de identidade**.
1. Clique em **Adicionar**.
1. Para o **tipo de fornecedor de identidade**, selecione **OpenID Connect**.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Configurar o fornecedor de identidade OpenID Connect

#### <a name="metadata-url"></a>URL de Metadados

De acordo com a especificação de todos os fornecedores de identidade OpenID Connect descreve um documento de metadados que contém a maioria das informações necessárias para executar o início de sessão. Isto inclui informações como os URL a utilizar e a localização de chaves públicas de assinatura do serviço. O documento de metadados OpenID Connect é sempre localizado num ponto final que termine em `.well-known\openid-configuration`.

Para o fornecedor de identidade OpenID Connect que procura para adicionar, introduza o URL de metadados.

#### <a name="client-id-and-secret"></a>ID de cliente e o segredo

Para permitir aos utilizadores iniciar sessão, o fornecedor de identidade irá exigir que os programadores de registar uma aplicação no seu serviço. Esta aplicação irá ter um ID (referido como o **ID de cliente**) e um **segredo do cliente**. Copiar estes valores do fornecedor de identidade e introduzi-las nos campos correspondentes.

> [!NOTE]
> O segredo do cliente é opcional. No entanto, tem de introduzir um segredo do cliente se gostaria de utilizar o [fluxo de código de autorização](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), que utiliza o segredo para trocar o código para o token.

#### <a name="scope"></a>Âmbito

Âmbitos de definem as informações e permissões que pretender para recolher através do seu fornecedor de identidade personalizada. Pedidos de OpenID Connect tem de conter o `openid` analisar o valor para receber o token de ID do fornecedor de identidade. Sem o token de ID, os utilizadores não conseguirão iniciar sessão no Azure AD B2C utilizando o fornecedor de identidade personalizada.

Outros âmbitos possam ser acrescentados (separados por espaço). Consulte a documentação do fornecedor de identidade personalizada para ver o que podem ser outros âmbitos disponíveis.

#### <a name="response-type"></a>Tipo de resposta

O tipo de resposta descreve de que tipo de informações será enviado volta na chamada inicial para o `authorization_endpoint` do fornecedor de identidade personalizada. 

* `code`: Como pelo [fluxo de código de autorização](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth), será devolvido um código de volta para o Azure AD B2C. O Azure AD B2C, em seguida, irá prosseguir para chamar o `token_endpoint` para trocar o código para o token.
* `token`: Um token de acesso será devolvido ao Azure AD B2C do fornecedor de identidade personalizada.
* `id_token`: Um token de ID será devolvido ao Azure AD B2C do fornecedor de identidade personalizada.


#### <a name="response-mode"></a>Modo de resposta

O modo de resposta define o método que deve ser utilizado para enviar os dados novamente a partir do fornecedor de identidade personalizado ao Azure AD B2C.

* `form_post`: Para melhor segurança, recomenda-se neste modo de resposta. A resposta é transmitida através de HTTP `POST` método, com o código ou um token que está a ser codificado em corpo, utilizando o `application/x-www-form-urlencoded` formato.
* `query`: O código ou token vai ser devolvido como um parâmetro de consulta.


#### <a name="domain-hint"></a>Sugestão de domínio

A sugestão de domínio pode ser utilizada para avançar diretamente para o início de sessão na página do fornecedor de identidade especificado, em vez de ter a marca de utilizador uma seleção entre a lista de fornecedores de identidade disponíveis. Para permitir que este tipo de comportamento, introduza um valor para a sugestão de domínio.

Para ir para o fornecedor de identidade personalizada, acrescentar o parâmetro `domain_hint=<domain hint value>` ao fim do seu pedido quando chamar o Azure AD B2C para início de sessão.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Mapeamento de afirmações do fornecedor de identidade OpenID Connect

Depois de identidade personalizada fornecedor envia um token de ID novamente para o Azure AD B2C, tem de Azure AD B2C para poder mapear as afirmações do token recebida para as afirmações que o Azure AD B2C reconhece e utiliza. 

Para cada um dos mapeamentos abaixo, consulte a documentação do fornecedor de identidade personalizada para compreender as afirmações que são devolvidas novamente em tokens do fornecedor de identidade.

* `User ID`: Introduza a afirmação que fornece o identificador exclusivo para o utilizador com sessão iniciada.
* `Display Name`: Introduza a afirmação que fornece o nome a apresentar ou o nome completo para o utilizador.
* `Given Name`: Introduza a afirmação que fornece o nome próprio do utilizador.
* `Surname`: Introduza a afirmação que fornece o apelido do utilizador.
* `Email`: Introduza a afirmação que fornece o endereço de e-mail do utilizador.

## <a name="next-steps"></a>Passos Seguintes

Adicionar personalizado OpenID Connect fornecedor de identidade para o [política incorporada](active-directory-b2c-reference-policies.md).
