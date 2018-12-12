---
title: Fluxos de utilizador no Azure Active Directory B2C | Documentos da Microsoft
description: Um tópico sobre a estrutura de política extensível do Azure Active Directory B2C e sobre como criar vários tipos de fluxo de utilizador.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d4a93b04b8ad86a6a6d36a5bdaf3209b49e7a9dc
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877087"
---
# <a name="azure-active-directory-b2c-user-flows"></a>Do Azure Active Directory B2C: Fluxos de utilizador


A estrutura de política extensível do Azure Active Directory (Azure AD) B2C é a força do principal do serviço. Políticas de descrevem completamente experiências de identidade do consumidor como inscrição, início de sessão ou edição de perfis. Para ajudá-lo a configurar as tarefas mais comuns de identidade, o portal do Azure AD B2C inclui políticas predefinidas e configuráveis chamadas **fluxos de utilizador**. Por exemplo, um fluxo de inscrição de utilizador permite-lhe controlar comportamentos configurando as seguintes definições:

* Tipos de conta (contas de redes sociais, como o Facebook) ou contas locais, como endereços de e-mail que os consumidores podem utilizar para se inscrever para a aplicação
* Atributos (por exemplo, nome próprio, código postal e tamanho do calçado) a recolher do consumidor durante a inscrição
* Utilização da autenticação Multifator do Azure
* O aspeto e funcionalidade de todas as páginas de inscrição
* Informações (que se manifesta como afirmações num token) que o aplicativo recebe quando terminar de execução do fluxo de utilizador

Pode criar vários fluxos de utilizador de tipos diferentes no seu inquilino e utilizá-los em seus aplicativos, conforme necessário. Fluxos de utilizador podem ser reutilizados em todas as aplicações. Essa flexibilidade permite aos programadores definir e modificar experiências de identidade do consumidor com mínima ou sem alterações ao seu código.

Fluxos de utilizador estão disponíveis para utilização por uma interface de programação simples. O aplicativo aciona um fluxo de utilizador utilizando um pedido de autenticação HTTP padrão (passar um parâmetro de fluxo de utilizador no pedido) e recebe um token personalizado como resposta. Por exemplo, a única diferença entre as solicitações que invocar um fluxo de inscrição de utilizador e as solicitações que invocar um fluxo de início de sessão do utilizador é o nome do fluxo de utilizador que é utilizado no parâmetro de cadeia de caracteres de consulta "p":

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow

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
&p=b2c_1_siin                                       // Your sign-in user flow

```

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>Criar um fluxo de utilizador de inscrição ou início de sessão

Este fluxo de utilizador lida com ambas as experiências de inscrição e início de sessão do consumidor com uma configuração única. Os consumidores são liderados para baixo o caminho certo, (inscrição ou início de sessão), dependendo do contexto. Também descreve os conteúdos de tokens que a aplicação receberá após inscrições com êxito ou inícios de sessão.  Um exemplo de código para o **inscrição ou início de sessão** é o fluxo de utilizador [disponível aqui](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  É recomendado que utilize este fluxo de utilizador através de um **Inscreva-se** fluxo de utilizador ou uma **início de sessão** fluxo de utilizador.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-user-flow"></a>Criar um fluxo de inscrição de utilizador

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-user-flow"></a>Criar um fluxo de início de sessão do utilizador

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-user-flow"></a>Criar um fluxo de utilizador de edição de perfil

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-user-flow"></a>Criar um fluxo de utilizador de reposição de palavra-passe

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-user-flows"></a>Fluxos de utilizador de pré-visualização

Como fazemos lançamentos de novos recursos, alguns deles podem não estar disponíveis em políticas existentes ou fluxos de utilizador.  Planeamos substituir as versões mais antigas com a versão mais recente do mesmo tipo, uma vez que estes fluxos de utilizador introduza GA.  As políticas existentes ou fluxos de utilizador não serão alterado e para tirar proveito desses novos recursos tem de criar novos fluxos de utilizador.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="how-do-i-link-a-sign-up-or-sign-in-user-flow-with-a-password-reset-user-flow"></a>Como posso ligar um fluxo de utilizador de inscrição ou início de sessão com um fluxo de utilizador de reposição de palavra-passe?
Quando cria um **inscrição ou início de sessão** utilizador fluxo (com contas locais), verá um **palavra-passe de Forgot?** link na primeira página da experiência. Ao clicar nesta ligação não automaticamente acionador uma palavra-passe de reposição de fluxo de utilizador. 

Em vez disso, o código de erro **`AADB2C90118`** é devolvido à sua aplicação. A aplicação tem de lidar com este código de erro ao invocar um fluxo de utilizador de reposição de palavra-passe específica. Para obter mais informações, consulte um [amostra que demonstra a abordagem da vinculação de fluxos de utilizador](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-user-flow-or-a-sign-up-user-flow-and-a-sign-in-user-flow"></a>Posso utilizar um fluxo de utilizador de inscrição ou início de sessão ou de um fluxo de inscrição de utilizador e de um fluxo de início de sessão do utilizador?
Recomendamos que utilize um **inscrição ou início de sessão** fluxo de utilizador através de um **Inscreva-se** fluxo de utilizador e um **início de sessão** fluxo de utilizador.  

O **inscrição ou início de sessão** fluxo de utilizador tem funções adicionais que o **início de sessão** fluxo de utilizador. Ele também permite-lhe utilizar a personalização da interface do Usuário de página e tem um suporte melhor para a localização. 

O **início de sessão** fluxo de utilizador é recomendado se não precisa de localizar os fluxos de utilizador, só precisam de capacidades de personalização de pequenas de identidade visual e quiser palavra-passe reposição incorporada a ele.

## <a name="next-steps"></a>Passos Seguintes
* [Token, sessão e configuração de início de sessão único](active-directory-b2c-token-session-sso.md)
* [Desativar a verificação de e-mail durante a inscrição do consumidor](active-directory-b2c-reference-disable-ev.md)

