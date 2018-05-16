---
title: Como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory | Microsoft Docs
description: 'Saiba como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory '
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: dastrock; smalser
ms.openlocfilehash: 1a33b5ab9e26ed497e3be2d430f66ef41402733d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory

Quando a depuração uma integração de aplicações baseados em SAML, muitas vezes, é útil utilizar uma ferramenta como o [Fiddler](http://www.telerik.com/fiddler) para ver o pedido SAML e a resposta SAML que contenham o token SAML que foi emitido a uma aplicação. 

![Fiddler][1]

Frequentemente os problemas relacionados com uma configuração incorreta no Azure Active Directory ou no lado de aplicação. Dependendo de onde pode ver o erro, terá de rever o pedido SAML ou a resposta:

- Vejo um erro na minha página de início de sessão da empresa [ligações para a secção]
- Vejo um erro na página da aplicação depois de iniciarem sessão [ligações para a secção]

## <a name="i-see-an-error-in-my-company-sign-in-page"></a>Vejo um erro na minha página de início de sessão da empresa.

Pode encontrar um mapeamento um para um entre o código de erro e os passos de resolução em [problemas em iniciar sessão a uma aplicação de galeria configurada para Federado de sessão único-](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML).

Se vir um erro na sua página de início de sessão da empresa, terá da mensagem de erro e o pedido SAML para depurar o problema.

### <a name="how-can-i-get-the-error--message"></a>Como posso obter a mensagem de erro?

Para obter a mensagem de erro, procure no canto inferior direito da página. Consulte o erro que inclui:

- Um CorrelationID
- Um carimbo
- Uma mensagem

![Erro][2] 

 
A Id de correlação e o timestamp formam um identificador exclusivo, que pode utilizar para localizar os registos de back-end associados com a falha de início de sessão. Estes valores são importantes quando cria um incidente de suporte com a Microsoft porque ajudar os engenheiros para fornecer uma rápida resolução para o seu problema.

A mensagem é a causa do problema início de sessão. 


### <a name="how-can-i-review-the-saml-request"></a>Como pode rever o pedido SAML?

O pedido SAML enviado pela aplicação para o Azure Active Directory é, geralmente, a resposta HTTP 200 última do [ https://login.microsoftonline.com ](https://login.microsoftonline.com).
 
Utilizar o Fiddler, pode ver o pedido SAML ao selecionar esta linha e, em seguida, selecionar o **Inspetores > Web Forms** separador no painel à direita. Clique com botão direito do **SAMLRequest** valor e, em seguida, selecione **enviar para TextWizard**. Em seguida, selecione **de Base64** do **transformar** menu descodificar o token e ver o respetivo conteúdo. 

Além disso, também pode copiar o valor de SAMLrequest e utilizar outro descodificador de Base64.

    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    Destination=https://login.microsoftonline.com/<Tenant-ID>/saml2
    AssertionConsumerServiceURL="https://contoso.applicationname.com/acs"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>

Com o pedido SAML descodificar, reveja o seguinte:

1. **Destino** no pedido SAML correspondem a único início de sessão no URL do serviço SAML obtido a partir do Azure Active Directory.
 
2. **Emissor** o SAML pedido é o mesmo **identificador** que configurou para a aplicação no Azure Active Directory. AD do Azure utiliza o emissor para localizar uma aplicação no seu diretório.

3. **AssertionConsumerServiceURL** é onde a aplicação de espera receber o token SAML do Azure Active Directory. Pode configurar este valor no Azure Active Directory, mas não é obrigatória se faz parte do pedido SAML.


## <a name="i-see-an-error-on-the-applications-page-after-signing-in"></a>Vejo um erro na página da aplicação depois de iniciar sessão

Neste cenário, a aplicação não está a aceitar a resposta emitida pelo Azure AD. É importante que verifique a resposta do Azure AD que contém o token SAML com o fabricante da aplicação para saber o que está em falta ou incorreta. 

### <a name="how-can-i-get-and-review-the-saml-response"></a>Como obter e reveja a resposta SAML?

A resposta do Azure AD que contém o token SAML normalmente é aquele que ocorre após um redirecionamento HTTP 302 de https://login.microsoftonline.com e são enviadas para configurada **URL de resposta** da aplicação. 

Pode ver o token SAML ao selecionar esta linha e, em seguida, selecionar o **Inspetores > Web Forms** separador no painel à direita. A partir daí, clique com botão direito do **SAMLResponse** valor e selecione **enviar para TextWizard**. Em seguida, selecione **de Base64** do **transformar** menu descodificar o token e ver o respetivo conteúdo utilizar outro descodificador de Base64. 

Também pode copiar o valor de **SAMLrequest** e utilizar outro descodificador de Base64.

Visite [erro na página da aplicação depois de iniciarem sessão](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML) orientações para obter mais informações sobre o que poderá estar em falta ou incorreta na resposta SAML de resolução de problemas.

Para obter informações sobre como rever o SAML resposta visite o artigo [protocolo SAML de início de sessão único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML#response).


## <a name="related-articles"></a>Artigos relacionados
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](../active-directory-apps-index.md)
* [Configurar o início de sessão único em aplicações que não fazem parte da galeria de aplicações do Azure Active Directory](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Como personalizar afirmações emitidas no Token SAML para aplicações previamente integradas](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png
[2]: ../media/active-directory-saml-debugging/error.png
