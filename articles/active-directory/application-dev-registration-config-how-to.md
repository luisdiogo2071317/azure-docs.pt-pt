---
title: "Como selecionar permissões para uma determinado API | Microsoft Docs"
description: "Como localizar os pontos finais de autenticação para uma aplicação personalizada estiver a desenvolver ou registar com o Azure AD."
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
ms.openlocfilehash: 6c8b8bb81e62747b7ab5eaca94d2820d2e0661d2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-select-permissions-for-a-given-api"></a>Como selecionar permissões para uma determinado API

Pode encontrar os pontos finais de autenticação para a sua aplicação no [portal do Azure](https://portal.azure.com).

-   Navegue para o [portal do Azure](https://portal.azure.com).

-   No painel de navegação esquerdo, clique em **do Azure Active Directory**.

-   Clique em **registos de aplicação** e escolha **pontos finais**.

-   Isto abrir o **pontos finais** página, que lista todos os pontos finais a autenticação para o seu inquilino.

-   Utilizar o ponto final específico para o protocolo de autenticação que está a utilizar, em conjunto com o ID de aplicação para craft a autenticação do pedido específicas da aplicação.

## <a name="next-steps"></a>Passos seguintes
[Guia para programadores do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide#authentication-and-authorization-protocols)
