---
title: Problema ao criar uma aplicação de Proxy de aplicações | Documentos da Microsoft
description: Como resolver problemas de criação de aplicações de Proxy de aplicações no portal de administração do Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
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
ms.openlocfilehash: 3e60ed3c291f3acd4372216fb34c7a089a78a414
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54460996"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problema ao criar uma aplicação de Proxy de aplicações 

Seguem-se alguns dos problemas comuns face de pessoas ao criar uma nova aplicação de proxy de aplicação.

## <a name="recommended-documents"></a>Documentos recomendados 

Para saber mais sobre como criar uma aplicação de Proxy de aplicações através do Portal de administração, veja [publicar aplicações com o Proxy de aplicações do Azure AD](application-proxy-add-on-premises-application.md).

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
[Ativar o Proxy de aplicações no portal do Azure](application-proxy-add-on-premises-application.md)
