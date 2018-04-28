---
title: Descrição geral do Protocolo .NET do Azure AD | Microsoft Docs
description: Como utilizar mensagens HTTP para autorizar o acesso a aplicações Web e APIs Web no inquilino utilizando o Azure AD.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2018
ms.author: priyamo
ms.openlocfilehash: 0b78ed6cdb1209d70cf0d561f74cfcddc09b2391
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
## <a name="register-your-application-with-your-ad-tenant"></a>Registar a aplicação com o inquilino do AD
Em primeiro lugar, tem de registar a aplicação com o seu inquilino do Azure Active Directory (Azure AD). Esta ação dar-lhe-á um ID de Aplicação para a aplicação e ativá-lo-á para receber tokens.

* Inicie sessão no [portal do Azure](https://portal.azure.com).
* Escolha o inquilino do Azure AD ao clicar na conta no canto superior direito da página.
* No painel de navegação esquerdo, clique em **do Azure Active Directory**.
* Clique em **registos de aplicação** e clique em **novo registo de aplicação**.
* Siga os avisos e crie uma nova aplicação. Para este tutorial não importa se é uma aplicação Web ou uma aplicação nativa, mas se pretender exemplos específicos de aplicações Web ou aplicações nativas, veja os [inícios rápidos](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Para aplicações Web, forneça o **URL de início de sessão**, que é o URL de base da sua aplicação, onde os utilizadores podem iniciar sessão por exemplo `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Para aplicações nativas de fornecer um **URI de redirecionamento**, que vai ser utilizado para devolver respostas de token do Azure AD. Introduza um valor específico para a aplicação, por exemplo `http://MyFirstAADApp`
* Depois de concluir o registo, o Azure AD irá atribuir a aplicação um identificador exclusivo do cliente, o **ID da aplicação**. É necessário que este valor nas secções seguintes, por isso, copie-o partir da página da aplicação.
* Para localizar a aplicação no portal do Azure, clique em **registos de aplicação**e, em seguida, clique em **ver todas as aplicações**.