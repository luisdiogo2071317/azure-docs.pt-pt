---
title: Como selecionar permissões para uma determinada API | Documentos da Microsoft
description: Como encontrar os pontos finais de autenticação para um aplicativo personalizado estiver a desenvolver ou a registar com o Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: f6c2540d8f0bb49491a428b085d20067a36d970a
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724185"
---
# <a name="how-to-select-permissions-for-a-given-api"></a>Como selecionar permissões para uma determinada API

Pode encontrar os pontos finais de autenticação para a sua aplicação no [portal do Azure](https://portal.azure.com).

-   Navegue para o [portal do Azure](https://portal.azure.com).

-   No painel de navegação esquerdo, clique em **do Azure Active Directory**.

-   Clique em **registos de aplicações** e escolha **pontos de extremidade**.

-   Isso abrirá o **pontos de extremidade** página, que lista todos os pontos finais de autenticação para o seu inquilino.

-   Utilizar o ponto final específico para o protocolo de autenticação que estiver a utilizar, em conjunto com o ID da aplicação para a autenticação, crie o pedido específico para a sua aplicação.

## <a name="next-steps"></a>Passos Seguintes
[Guia para programadores do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide#authentication-and-authorization-protocols)
