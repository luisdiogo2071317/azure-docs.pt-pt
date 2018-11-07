---
title: Conjunto redirecionar URLs b2clogin.com para o Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre como utilizar b2clogin.com no seu redirecionamento URLs para o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 00fde1145b038f3bf82e3dbc5e1dc967e1865ad6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234930"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Conjunto redirecionar URLs b2clogin.com para o Azure Active Directory B2C

Quando configurar um fornecedor de identidade para inscrição e início de sessão na sua aplicação do Azure Active Directory (Azure AD) B2C, tem de especificar um URL de redirecionamento. No passado, login.microsoftonline.com foi utilizado, agora deve usar b2clogin.com.

Utilizar b2clogin.com dá-lhe benefícios adicionais, tais como:

- Cookies já não são partilhados com outros serviços Microsoft.
- Suas URLs deixarão de incluir uma referência à Microsoft. Por exemplo, `https://your-tenant-name.b2clogin.com/tfp/your-tenant-ID/policyname/v2.0/.well-known/openid-configuration`.

Considere estas definições que poderão ter de alterar quando utilizar b2clogin.com:

- Definir o redirecionamento URLs em seus aplicativos de fornecedor de identidade para utilizar b2clogin.com. 
- Defina a sua aplicação do Azure AD B2C para utilizar b2clogin.com para referências de política e pontos finais de token. 
- Se estiver a utilizar a MSAL, terá de definir o **ValidateAuthority** propriedade `false`.
- Certifique-se de que alterar quaisquer **origens permitidas** definidas nas definições de CORS para [personalização da interface do usuário](active-directory-b2c-ui-customization-custom-dynamic.md).  

## <a name="change-redirect-urls"></a>URLs de redirecionamento de alteração

Para utilizar b2clogin.com, nas definições para a sua aplicação de fornecedor de identidade, procure e alterar a lista de URLs fidedignos para redirecionar novamente para o Azure AD B2C.  Atualmente, provavelmente terá de configurá-lo até redirecionar novamente para algum site login.microsoftonline.com. 

Terá de alterar o URL de redirecionamento para que `your-tenant-name.b2clogin.com` está autorizado. Certifique-se de substituir `your-tenant-name` com o nome do seu Azure AD B2C inquilino e remover `/te` se existir no URL. Existem pequenas variações para este URL para cada fornecedor de identidade por isso, verifique a página correspondente para obter o URL exato.

Pode encontrar informações de configuração para fornecedores de identidade nos seguintes artigos:

- [Conta Microsoft](active-directory-b2c-setup-msa-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Google](active-directory-b2c-setup-goog-app.md)
- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn](active-directory-b2c-setup-li-app.md)
- [Twitter](active-directory-b2c-setup-twitter-app.md)
- [GitHub](active-directory-b2c-setup-github-app.md)
- [Weibo](active-directory-b2c-setup-weibo-app.md)
- [QQ](active-directory-b2c-setup-qq-app.md)
- [WeChat](active-directory-b2c-setup-wechat-app.md)
- [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)
- [Custom OIDC](active-directory-b2c-setup-oidc-idp.md)

## <a name="update-your-application"></a>Atualizar a sua aplicação

A aplicação do Azure AD B2C refere-se provavelmente a `login.microsoftonline.com` em vários locais, como suas referências de política e os pontos finais de token.  Certifique-se de que o ponto final de autorização, o ponto final do token e o emissor foram atualizados para utilizar `your-tenant-name.b2clogin.com`.  

## <a name="set-the-validateauthority-property"></a>Defina a propriedade ValidateAuthority

Se estiver a utilizar a MSAL, defina o **ValidateAuthority** para `false`. O exemplo seguinte mostra como pode definir a propriedade:

Na [MSAL para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet):

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

E, em [MSAL para Javascript](https://github.com/AzureAD/microsoft-authentication-library-for-js):

```Javascript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```
