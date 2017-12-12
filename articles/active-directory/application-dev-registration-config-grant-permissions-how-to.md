---
title: "Como conceder permissões a uma aplicação desenvolvida personalizado | Microsoft Docs"
description: "Como conceder permissões à sua aplicação personalizada desenvolvida utilizando o portal do Azure AD ou um parâmetro de URL"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 0f13efd5e629bde592a3ab3a3e2db4a22180b5bb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Como conceder permissões a uma aplicação desenvolvida personalizada

Se pretender conceder consentimento preventivamente na sua aplicação ou estiver a executar um erro que poderá não ter autorizado para uma aplicação, repita os passos abaixo.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Como efetuar a administração consentimento para a sua aplicação

Isto tem o efeito de conceder permissão para a aplicação para todos os utilizadores na sua organização.

1. Navegue para o **registos de aplicação** painel como uma **administrador global**, em seguida, selecione a aplicação.

2. Selecione **permissões obrigatórias**e, finalmente, prima a **conceder permissões** botão na parte superior do painel.

Em alternativa, pode construir um pedido para *login.microsoftonline.com* com as configurações de aplicação e de acréscimo em *& prompt = admin\_consentimento*. Depois de iniciar sessão com credenciais de administrador, a aplicação tiver sido concedida consentimento para todos os utilizadores.

## <a name="how-to-force-user-consent-for-your-application"></a>Como forçar o consentimento do utilizador para a sua aplicação

* Acrescentar para pedidos de autenticação *& prompt = consentimento* que requer que os utilizadores finais consentimento sempre que efetuam a autenticação.

## <a name="next-steps"></a>Passos seguintes

[Consentimento e a integração de aplicações para AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Consentimento e Permissioning para v 2.0 de AzureAD convergido aplicações](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
