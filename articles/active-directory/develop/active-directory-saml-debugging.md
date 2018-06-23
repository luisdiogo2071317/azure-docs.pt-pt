---
title: Depurar baseados em SAML-início de sessão único - Azure Active Directory | Microsoft Docs
description: Depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory.
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock, smalser
ms.openlocfilehash: d0c006b21e00693fe6c8b35237d4ce55f67c0f75
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320598"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Depuração baseados em SAML-início de sessão único para aplicações no Azure Active Directory

Saiba como localizar e corrigir [de sessão único-](../manage-apps/what-is-single-sign-on.md) problemas de aplicações no Azure Active Directory (Azure AD), que suportem [Security Assertion Markup Language (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Antes de começar
Recomendamos que instale o [segura de aplicações My início de sessão extensão](../active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension). Esta extensão de browser torna mais fácil recolher as informações de resposta SAML que precisa para resolver problemas com o início de sessão único e pedido SAML. No caso de não é possível instalar a extensão, este artigo mostra como resolver problemas com e sem a extensão instalada.

Para transferir e instalar a minha segura de aplicações início de sessão extensão, utilize uma das ligações seguintes.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Limite](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>Teste baseados em SAML-início de sessão único

Para testar baseados em SAML-início de sessão único entre AAD e uma aplicação de destino:

1.  Iniciar sessão para o [portal do Azure](https://portal.azure.com) como um administrador global ou outro administrador que está autorizado a gerir aplicações.
2.  No painel esquerdo, clique em **do Azure Active Directory**e, em seguida, clique em **aplicações empresariais**. 
3.  Na lista de aplicações da empresa, clique na aplicação que pretende testar o início de sessão único e, em seguida, as opções do esquerdo, clique em **de sessão único-**.
4.  Para abrir a baseados em SAML único início de sessão de teste experiência, no **domínios e URLs** secção clique **teste SAML definição**. Se o botão de teste SAML definição está esbatido, terá de preencher o limite e guardar os atributos necessários pela primeira vez.
5.  No **testar o início de sessão único** painel, utilize as credenciais da sua empresa para iniciar sessão para a aplicação de destino. Pode iniciar sessão como o utilizador atual, ou como um utilizador diferente. Se iniciar sessão como outro utilizador, uma linha de comandos irá pedir autenticar.

Se com êxito tem sessão iniciada, o teste foi efectuada com êxito. Neste caso, o Azure AD emitido um token de resposta SAML para a aplicação. A aplicação utilizou o token SAML para iniciar sessão com êxito no.

Se tiver um erro na página de início de sessão de empresa ou a página da aplicação, utilize uma das secções seguintes para resolver o erro.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Resolver um erro de início de sessão na sua página de início de sessão da empresa

Ao tentar iniciar sessão no poderá ver um erro na sua página de início de sessão da empresa. 

![Erro de início de sessão](media/active-directory-saml-debugging/error.png)

Para depurar este erro, terá da mensagem de erro e o pedido SAML. A minha segura de aplicações início de sessão extensão automaticamente reúne estas informações e apresenta a orientação de resolução sobre o Azure AD. 

Para resolver o erro de início de sessão com a proteger o MyApps a extensão do início de sessão instalada:

1.  Quando ocorre um erro, a extensão redireciona-o novamente para o Azure Ad **testar o início de sessão único** painel. 
2.  No **testar o início de sessão único** painel, clique em **transferir o pedido SAML**. 
3.  Deverá ver a documentação de orientação com base no erro e os valores no pedido SAML de resolução específico. Reveja a documentação de orientação.

Para resolver o erro sem instalar MyApps Secure extensão de início de sessão:

1. Copie a mensagem de erro no canto inferior direito da página. A mensagem de erro inclui:
    - Um CorrelationID e Timestamp. Estes valores são importantes quando cria um incidente de suporte com a Microsoft porque ajudar os engenheiros para identificar o problema e fornecer uma resolução precisa para o seu problema.
    - Uma instrução identificar a causa do problema.
2.  Voltar para o Azure AD e localize o **testar o início de sessão único** painel.
3.  Na caixa de texto acima **obter orientação de resolução**, cole a mensagem de erro.
3.  Clique em **obter orientação de resolução** para apresentar os passos para resolver o problema. As orientações poderão necessitar de informações do pedido SAML ou resposta SAML. Se não estiver a utilizar a proteger o MyApps início de sessão extensão, poderá ser necessário uma ferramenta como [Fiddler](http://www.telerik.com/fiddler) para obter o pedido SAML e resposta.
4.  Certifique-se de que o destino no pedido SAML corresponde ao único início de sessão no URL do serviço SAML obtido a partir do Azure Active Directory
5.  Certifique-se de que o emissor no pedido SAML é o mesmo identificador que configurou para a aplicação no Azure Active Directory. AD do Azure utiliza o emissor para localizar uma aplicação no seu diretório.
6.  Certifique-se AssertionConsumerServiceURL onde a aplicação de espera receber o token SAML do Azure Active Directory. Pode configurar este valor no Azure Active Directory, mas não é obrigatória se faz parte do pedido SAML.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Resolver um erro de início de sessão na página da aplicação

Pode iniciar sessão com êxito e, em seguida, consulte o erro na página da aplicação. Isto ocorre quando o Azure AD emitido um token para a aplicação, mas a aplicação não aceita a resposta.   

Para resolver o erro:

1. Se a aplicação na galeria do Azure AD, certifique-se de que seguiu todos os passos para integrar a aplicação com o Azure AD. Para localizar as instruções de integração para a sua aplicação, consulte o [lista de tutoriais de integração de aplicações SaaS](../saas-apps/tutorial-list.md).
2. Obter a resposta SAML.
    - Se a extensão segura de aplicações My início de sessão estiver instalada, do **testar o início de sessão único** painel, clique em **transferir a resposta SAML**.
    - Se a extensão não está instalada, utilize uma ferramenta como [Fiddler](http://www.telerik.com/fiddler) para obter a resposta SAML. 
3. Tenha em atenção estes elementos no token SAML resposta:
    - Identificador exclusivo do utilizador do valor de NameID e formato
    - Afirmações emitidas no token
    - Certificado utilizado para assinar o token. Para obter informações sobre como analisar a resposta SAML, consulte [protocolo SAML de início de sessão único](active-directory-single-sign-on-protocol-reference.md).
4. Para obter mais informações sobre a resposta SAML, consulte [protocolo SAML de início de sessão único](active-directory-single-sign-on-protocol-reference.md).
5. Agora que reviu a resposta SAML, consulte [erro na página de uma aplicação depois de iniciarem sessão](../application-sign-in-problem-application-error.md) para obter orientações sobre como resolver o problema. 
6. Se ainda não é possível iniciar sessão com êxito, pode colocar o fornecedor da aplicação que está em falta de resposta SAML.


## <a name="next-steps"></a>Passos Seguintes
Agora que o início de sessão único está a funcionar à sua aplicação, foi [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações SaaS](../active-directory-saas-app-provisioning.md), ou [introdução ao acesso condicional](../active-directory-conditional-access-azure-portal-get-started.md).


