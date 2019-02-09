---
title: Problema ao criar uma aplicação de Proxy de aplicações | Documentos da Microsoft
description: Como resolver problemas de criação de aplicações de Proxy de aplicações no portal de administração do Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.openlocfilehash: 832624844930b724c54b6df47601ae2f5d37e2d2
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961708"
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
