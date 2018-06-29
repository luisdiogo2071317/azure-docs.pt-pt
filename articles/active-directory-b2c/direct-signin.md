---
title: Configurar direta início de sessão utilizando o Azure Active Directory B2C | Microsoft Docs
description: Saiba como pré-povoar o nome de início de sessão ou redirecionar diretamente para um fornecedor de identidade de redes sociais.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/18/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: cc3baa8fe4139acec94a722bf8c2bccb708a9470
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102531"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Configurar direta início de sessão utilizando o Azure Active Directory B2C

Quando configurar o início de sessão para a sua aplicação utilizar o Azure Active Directory (AD) B2C, pode pré-povoar o nome de início de sessão ou direto início de sessão para um fornecedor de identidade de redes sociais específicas, tais como o Facebook, LinkedIn ou uma conta Microsoft. 

## <a name="prepopulate-the-sign-in-name"></a>Pré-povoar o nome de início de sessão

Durante uma journey de início de sessão do utilizador, uma aplicação da entidade confiadora pode ter como destino um nome de utilizador ou domínio específico. Quando a filtrar um utilizador, uma aplicação pode especificar, no pedido de autorização, o `login_hint` parâmetro com o nome de utilizador de início de sessão de consulta. O Azure AD B2C preenche automaticamente o nome de início de sessão, enquanto o utilizador só tem de fornecer a palavra-passe.

![utilizar a sugestão de início de sessão](./media/direct-signin/login-hint.png) 

O utilizador é capaz de alterar o valor na caixa de início de sessão no texto.

Se estiver a utilizar uma política personalizada, substitua o `SelfAsserted-LocalAccountSignin-Email` perfil técnica. No `<InputClaims>` secção, defina o atributo ValorPredefinido de afirmação signInName para `{OIDC:LoginHint}`. O `{OIDC:LoginHint}` variável contém o valor do `login_hint` parâmetro. O Azure AD B2C lê o valor da afirmação signInName e previamente preenche a caixa de texto signInName.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="redirect-sign-in-to-a-social-provider"></a>Redireccionamento de início de sessão para um fornecedor de redes social

Se tiver configurado o journey início de sessão para a sua aplicação incluir as contas de redes sociais, como o Facebook, LinkedIn ou Google, pode especificar o `domain_hint` parâmetro. Este parâmetro de consulta oferece uma sugestão ao Azure AD B2C sobre o fornecedor de identidade sociais que deve ser utilizado para início de sessão. Por exemplo, se a aplicação especifica `domain_hint=facebook.com`, início de sessão vai diretamente para a página de início de sessão do Facebook.

![utilizar a sugestão de domínio](./media/direct-signin/domain-hint.png) 

Se estiver a utilizar uma política personalizada, pode configurar o nome de domínio utilizando o `<Domain>domain name</Domain>` elemento XML de qualquer `<ClaimsProvider>`. 

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


