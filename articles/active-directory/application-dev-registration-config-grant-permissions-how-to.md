---
title: Como pode conceder permissões para uma aplicação personalizada desenvolvida | Documentos da Microsoft
description: Como conceder permissões à sua aplicação personalizada desenvolvida com o portal do Azure AD ou um parâmetro de URL
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 75beeb35b740bb126fff905f4cfa5a0b455e025e
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365248"
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Como pode conceder permissões para uma aplicação personalizada desenvolvida

Se quiser conceder autorização preventivamente na sua aplicação ou se deparar com um erro que não tiver dado consentimento a uma aplicação, tente os passos abaixo.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Como realizar o consentimento de administrador para a sua aplicação

Isso tem o efeito de conceder permissão à aplicação para todos os utilizadores na sua organização.

1. Navegue para o **registos de aplicações** painel como uma **administrador global**, em seguida, selecione a aplicação.

2. Selecione **permissões obrigatórias**e finalmente chegar a **conceder permissões** botão na parte superior do painel.

Em alternativa, pode construir um pedido para *login.microsoftonline.com* com suas configurações de aplicação e associe *& linha de comandos = administração\_consentimento*. Depois de iniciar sessão com credenciais de administrador, a aplicação recebeu consentimento para todos os utilizadores.

## <a name="how-to-force-user-consent-for-your-application"></a>Como forçar o consentimento do utilizador para a sua aplicação

* Acrescentar para pedidos de autor *& linha de comandos = consentimento* exigir que os utilizadores finais dar consentimento sempre que fizer a autenticação.

## <a name="next-steps"></a>Passos Seguintes

[Consentimento e a integração de aplicações para o AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Aplicações de convergência de consentimento e concessão de permissões para o AzureAD v2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Stack Overflow do AzureAD](http://stackoverflow.com/questions/tagged/azure-active-directory)
