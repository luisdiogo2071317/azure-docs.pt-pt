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
ms.openlocfilehash: 5a4f08718175dd7ef369d801807e6c8e1ff28fd9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39485670"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Registar a aplicação com o inquilino do AD
Em primeiro lugar, terá de registar a sua aplicação com o seu inquilino do Azure Active Directory (Azure AD). Esta ação dar-lhe-á um ID de Aplicação para a aplicação e ativá-lo-á para receber tokens.

* Inicie sessão no [portal do Azure](https://portal.azure.com).
* Escolha o seu inquilino do Azure AD ao clicar na sua conta no canto superior direito da página, em seguida, clicando no **trocar diretório** navegação e, em seguida, selecione o inquilino adequado. 
  * Ignore este passo, se tiver apenas um inquilino do Azure AD na sua conta ou se já tiver selecionado o adequado inquilino do Azure AD.
* No painel de navegação da esquerda, clique em **Azure Active Directory**.
* Clique em **registos de aplicações** e clique em **novo registo de aplicação**.
* Siga os avisos e crie uma nova aplicação. Para este tutorial não importa se é uma aplicação Web ou uma aplicação nativa, mas se pretender exemplos específicos de aplicações Web ou aplicações nativas, veja os [inícios rápidos](../articles/active-directory/develop/azure-ad-developers-guide.md).
  * Para aplicações Web, indique o **URL de início de sessão**, que é o URL de base da sua aplicação, onde os utilizadores podem iniciar sessão, por exemplo `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Para aplicativos nativos fornecem uma **URI de redirecionamento**, que o Azure AD irá utilizar para devolver respostas token. Introduza um valor específico para a aplicação, por exemplo `http://MyFirstAADApp`
* Depois de concluir o registo, Azure AD atribuirá seu aplicativo um identificador exclusivo do cliente, o **ID da aplicação**. Este valor é necessário nas secções seguintes, por isso, copie-o partir da página de aplicativo.
* Para localizar seu aplicativo no portal do Azure, clique em **registos de aplicações**e, em seguida, clique em **ver todas as aplicações**.
