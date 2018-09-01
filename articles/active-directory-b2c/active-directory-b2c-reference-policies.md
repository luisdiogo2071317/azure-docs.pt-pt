---
title: Políticas incorporadas no Azure Active Directory B2C | Documentos da Microsoft
description: Um tópico sobre a estrutura de política extensível do Azure Active Directory B2C e sobre como criar vários tipos de política.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f26db8bcb50fa09a8d2829d477f90cac8c52533f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337579"
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Políticas incorporadas


A estrutura de política extensível do Azure Active Directory (Azure AD) B2C é a força do principal do serviço. Políticas de descrevem completamente experiências de identidade do consumidor como inscrição, início de sessão ou edição de perfis. Por exemplo, uma política de inscrição permite-lhe controlar comportamentos através da configuração das seguintes definições:

* Tipos de conta (contas de redes sociais, como o Facebook) ou contas locais, como endereços de e-mail que os consumidores podem utilizar para se inscrever para a aplicação
* Atributos (por exemplo, nome próprio, código postal e tamanho do calçado) a recolher do consumidor durante a inscrição
* Utilização da autenticação Multifator do Azure
* O aspeto e funcionalidade de todas as páginas de inscrição
* Informações (que se manifesta como afirmações num token) que o aplicativo recebe quando a política de execução estiver concluída

Pode criar várias políticas de tipos diferentes no seu inquilino e utilizá-los em seus aplicativos, conforme necessário. As políticas podem ser reutilizadas em todas as aplicações. Essa flexibilidade permite aos programadores definir e modificar experiências de identidade do consumidor com mínima ou sem alterações ao seu código.

As políticas estão disponíveis para utilização por uma interface de programação simples. Seu aplicativo aciona uma política ao utilizar uma solicitação de autenticação HTTP padrão (passar um parâmetro de política no pedido) e recebe um token personalizado como resposta. Por exemplo, a única diferença entre as solicitações que invocam uma política de inscrição e as solicitações que invocam uma política de início de sessão é o nome da política que é utilizado no parâmetro de cadeia de caracteres de consulta "p":

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

## <a name="create-a-sign-up-or-sign-in-policy"></a>Criar uma política de inscrição ou início de sessão

Esta política lida com ambas as experiências de inscrição e início de sessão do consumidor com uma configuração única. Os consumidores são liderados para baixo o caminho certo, (inscrição ou início de sessão), dependendo do contexto. Também descreve os conteúdos de tokens que a aplicação receberá após inscrições com êxito ou inícios de sessão.  Um exemplo de código para o **inscrição ou início de sessão** diretiva é [disponível aqui](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  É recomendado que utilize esta política através de um **Inscreva-se** política ou uma **início de sessão** política.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Criar uma política de inscrição

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Criar uma política de início de sessão

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Criar uma política de edição de perfil

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Criar uma política de reposição de palavra-passe

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-policies"></a>Políticas de pré-visualização

Como fazemos lançamentos de novos recursos, alguns deles podem não estar disponíveis sobre as políticas existentes.  Planeamos substituir as versões mais antigas com a versão mais recente do mesmo tipo, uma vez que estas políticas introduza GA.  As políticas existentes não serão alterado e para tirar proveito desses novos recursos tem de criar novas políticas.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Como posso ligar uma política de inscrição ou início de sessão com uma política de reposição de palavra-passe?
Quando cria um **inscrição ou início de sessão** política (com contas locais), verá um **palavra-passe de Forgot?** link na primeira página da experiência. Ao clicar nesta ligação não automaticamente uma palavra-passe de Acionador a política de reposição. 

Em vez disso, o código de erro **`AADB2C90118`** é devolvido à sua aplicação. A aplicação tem de lidar com este código de erro ao invocar uma política de reposição de palavra-passe específica. Para obter mais informações, consulte um [amostra que demonstra a abordagem da vinculação políticas](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Posso utilizar uma política de inscrição ou início de sessão ou de uma política de inscrição e de uma política de início de sessão?
Recomendamos que utilize um **inscrição ou início de sessão** política através de um **Inscreva-se** política e um **início de sessão** política.  

O **inscrição ou início de sessão** política tem funções adicionais que o **início de sessão** política. Ele também permite-lhe utilizar a personalização da interface do Usuário de página e tem um suporte melhor para a localização. 

O **início de sessão** política é recomendada se não precisa de localizar as suas políticas, apenas precisam de capacidades de personalização de pequenas de identidade visual e quiser palavra-passe reposição incorporada a ele.

## <a name="next-steps"></a>Passos Seguintes
* [Token, sessão e configuração de início de sessão único](active-directory-b2c-token-session-sso.md)
* [Desativar a verificação de e-mail durante a inscrição do consumidor](active-directory-b2c-reference-disable-ev.md)

