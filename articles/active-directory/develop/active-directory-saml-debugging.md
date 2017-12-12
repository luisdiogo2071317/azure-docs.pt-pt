---
title: "Como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory | Microsoft Docs"
description: "Saiba como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 7aa7ca90f9098f30565524470ca23783e97195e0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory
Quando a depuração uma integração de aplicações baseados em SAML, muitas vezes, é útil utilizar uma ferramenta como o [Fiddler](http://www.telerik.com/fiddler) para ver o pedido SAML, a resposta SAML e o token SAML real, que é emitido para a aplicação. Ao examinar o token SAML, pode certificar-se de que todos os atributos necessários, o nome de utilizador no assunto SAML e o URI do emissor são vem conforme esperado.

![][1]

A resposta do Azure AD que contém o token SAML normalmente é aquele que ocorre após um redirecionamento HTTP 302 de https://login.windows.net e é enviada para configurada **URL de resposta** da aplicação. 

Pode ver o token SAML ao selecionar esta linha e, em seguida, selecionar o **Inspetores > Web Forms** separador no painel à direita. A partir daí, clique com botão direito do **SAMLResponse** valor e selecione **enviar para TextWizard**. Em seguida, selecione **de Base64** do **transformar** menu descodificar o token e ver o respetivo conteúdo.

**Tenha em atenção**: para ver o conteúdo deste pedido de HTTP, Fiddler pode solicitar-lhe configurar a desencriptação de tráfego HTTPS, o que precisa de fazer.

## <a name="related-articles"></a>Artigos relacionados
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](../active-directory-apps-index.md)
* [Configurar o início de sessão único em aplicações que não fazem parte da galeria de aplicações do Azure Active Directory](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Como personalizar afirmações emitidas no Token SAML para aplicações previamente integradas](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png