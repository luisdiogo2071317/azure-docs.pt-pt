---
title: Utilizar b2clogin.com | Documentos da Microsoft
description: Saiba como utilizar b2clogin.com em vez de login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c24582fce44006d9a3972d73078aa8cb0d212c11
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053849"
---
# <a name="using-b2clogincom"></a>Utilizar B2Clogin.com

Daqui em diante, estamos incentivando todos os clientes a utilizar `<YourDirectoryName>.b2clogin.com` e vamos preterir `login.microsoftonline.com`. B2Clogin.com dá-lhe benefícios adicionais, tais como:
* Já não partilham o mesmo cookie com outros serviços Microsoft.
* Pode remover todas as referências a Microsoft em sua URL (pode substituir `<YourDirectoryName>.onmicrosoft.com` com o seu ID de diretório). Por exemplo: `https://<YourDirectoryName>.b2clogin.com/tfp/<YourDirectoryID>/<policyname>/v2.0/.well-known/openid-configuration`.

Aqui está o que precisa fazer para migrar para b2clogin.com

* Alterar os URIs de redirecionamento para as suas aplicações do fornecedor de identidade de redes sociais
* Editar a sua aplicação para utilizar B2Clogin.com em vez de `login.microsoftonline.com` para referências de política e pontos finais de token.
* Se estiver a utilizar a MSAL, precisa definir `ValidateAuthority=false`.  

##<a name="redirect-uris-for-social-identity-providers"></a>URIs de redirecionamento para fornecedores de identidade social

Se tiver que configurar no seu diretório de fornecedores de identidade de conta de redes sociais terá de fazer modificações nas respetivas aplicações.  Existe um parâmetro para a aplicação para cada fornecedor de redes sociais que contém uma lista de URLs fidedignos para redirecionar novamente para o Azure AD B2C.  Atualmente, é provavelmente o tenha configurado para redirecionar novamente para algumas `login.microsoftonline.com` site, terá de alterar este URL, para que `YourDirectoryName.b2clogin.com` vai ser um URI de redirecionamento autorizado.  Certifique-se remover o `/te` também.  Existem pequenas variações para este URL para cada fornecedor de identidade por isso, verifique a página correspondente para obter o URL exato.  

| Fornecedor de identidade |
|-------------------|
|[Conta Microsoft](active-directory-b2c-setup-msa-app.md)|
|[Facebook](active-directory-b2c-setup-fb-app.md)|
|[Google](active-directory-b2c-setup-goog-app.md)|
|[Amazon](active-directory-b2c-setup-amzn-app.md)|
|[LinkedIn](active-directory-b2c-setup-li-app.md)|
|[Twitter](active-directory-b2c-setup-twitter-app.md)|
|[GitHub](active-directory-b2c-setup-github-app.md)|
|[Weibo](active-directory-b2c-setup-weibo-app.md)|
|[QQ](active-directory-b2c-setup-qq-app.md)|
|[WeChat](active-directory-b2c-setup-wechat-app.md)|
|[Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)|
|[Custom OIDC](active-directory-b2c-setup-oidc-idp.md)|

##<a name="update-your-application-references"></a>Atualizar suas referências de aplicação

A aplicação refere-se provavelmente a `login.microsoftonline.com` em vários locais, como suas referências de política e os pontos finais de token.  Certifique-se de que o ponto final de autorização, o ponto final do token e o emissor foram atualizados.  

##<a name="set-validateauthorityfalse-in-msal"></a>Definir `ValidateAuthority=false` no MSAL

Se estiver a utilizar a MSAL, precisará definir `ValidateAuthority=false`.  Para obter mais informações, consulte [esta documentação](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase?view=azure-dotnet).