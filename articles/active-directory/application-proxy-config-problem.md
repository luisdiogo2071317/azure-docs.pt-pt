---
title: "Problema ao criar uma aplicação de Proxy de aplicações | Microsoft Docs"
description: "Como resolver problemas de criação de aplicações de Proxy da aplicação no portal de administração do Azure AD"
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
ms.openlocfilehash: 5b8346ee2e02ea62b7a11b88a790cff56a7d13f4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>Problema ao criar uma aplicação de Proxy de aplicações 

Seguem-se alguns dos problemas comuns enfrentam pessoas ao criar uma nova aplicação de proxy de aplicações.

## <a name="recommended-documents"></a>Documentos recomendados 

Para obter mais informações sobre como criar uma aplicação de Proxy de aplicações através do Portal de administração, consulte [publicar aplicações através do Proxy de aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Se estiver a seguir os passos esse documento e está a obter um erro ao criar a aplicação, consulte os detalhes do erro para obter informações e sugestões para saber como corrigir a aplicação. A maioria das mensagens de erro incluem uma correção sugerida. 

## <a name="specific-things-to-check"></a>Específicos aspetos a verificar

Para evitar erros comuns, certifique-se:

-   É um administrador com permissão para criar uma aplicação de Proxy de aplicações

-   O URL interno é exclusivo

-   O URL externo é exclusivo

-   Os URLs começar a utilizar http ou https e terminar com um "/"

-   O URL deve ser um nome de domínio e não um endereço IP

A mensagem de erro deverá apresentar no canto superior direito ao criar a aplicação. Também pode selecionar o ícone de notificação para ver as mensagens de erro.

   ![Pedido de notificação](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Passos seguintes
[Ativar o Proxy da aplicação no portal do Azure](active-directory-application-proxy-enable.md)
