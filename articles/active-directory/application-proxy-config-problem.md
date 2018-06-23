---
title: Problema ao criar uma aplicação de Proxy de aplicações | Microsoft Docs
description: Como resolver problemas de criação de aplicações de Proxy da aplicação no portal de administração do Azure AD
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
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 97795593ecc83a0b15f018861f5cd9139d26443b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335113"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problema ao criar uma aplicação de Proxy de aplicações 

Seguem-se alguns dos problemas comuns enfrentam pessoas ao criar uma nova aplicação de proxy de aplicações.

## <a name="recommended-documents"></a>Documentos recomendados 

Para obter mais informações sobre como criar uma aplicação de Proxy de aplicações através do Portal de administração, consulte [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md).

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

## <a name="next-steps"></a>Passos Seguintes
[Ativar o Proxy da aplicação no portal do Azure](manage-apps/application-proxy-enable.md)
