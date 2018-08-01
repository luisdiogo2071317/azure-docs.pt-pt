---
title: Problema ao criar uma aplicação de Proxy de aplicações | Documentos da Microsoft
description: Como resolver problemas de criação de aplicações de Proxy de aplicações no portal de administração do Azure AD
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
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 3f0c05673ec970e5763f27fc0045b9a529b2ffee
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365479"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problema ao criar uma aplicação de Proxy de aplicações 

Seguem-se alguns dos problemas comuns face de pessoas ao criar uma nova aplicação de proxy de aplicação.

## <a name="recommended-documents"></a>Documentos recomendados 

Para saber mais sobre como criar uma aplicação de Proxy de aplicações através do Portal de administração, veja [publicar aplicações com o Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md).

Se estiver a seguir os passos esse documento e estiver a receber um erro ao criar a aplicação, consulte os detalhes do erro para obter informações e sugestões para saber como corrigir o aplicativo. A maioria das mensagens de erro incluem uma correção sugerida. 

## <a name="specific-things-to-check"></a>Aspetos específicos a verificar

Para evitar erros comuns, certifique-se:

-   For um administrador com permissão para criar uma aplicação de Proxy de aplicações

-   O URL interno é exclusivo

-   O URL externo é exclusivo

-   Os URLs de começar com http ou https e terminar com uma "/"

-   O URL deve ser um nome de domínio e não um endereço IP

A mensagem de erro deve exibir no canto superior direito, ao criar a aplicação. Também pode selecionar o ícone de notificação para ver as mensagens de erro.

   ![Linha de notificação](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Passos Seguintes
[Ativar o Proxy de aplicações no portal do Azure](manage-apps/application-proxy-enable.md)
