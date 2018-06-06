---
title: Gerir programas e revisões de acesso de controlos do Azure AD | Microsoft Docs
description: Pode criar programas adicionais para cada governação, gestão de riscos e iniciativa de conformidade da sua organização para recolher e organizar revisões de acesso do Azure Active Directory como controlos.
services: active-directory
documentationcenter: ''
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: rolyon
ms.openlocfilehash: f457d2a7d23785a4af66b0578412defd38f4b410
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698109"
---
# <a name="manage-programs-and-their-controls"></a>Gerir programas e os controlos 

Azure Active Directory (Azure AD) inclui revisões de acesso dos membros do grupo e acesso de aplicação. Certifique-se destes exemplos de controlos de supervisão de quem tem acesso a aplicações e as associações de grupo da sua organização. As organizações podem utilizar estes controlos a abordar de forma eficiente os respetivos governação, gestão de riscos e requisitos de conformidade.

## <a name="create-and-manage-programs-and-their-controls"></a>Criar e gerir programas e os controlos
Pode simplificar a como monitorizar e recolher revisões de acesso para diferentes fins ao organizá-los em programas. Cada revisão do acesso pode ser associado a um programa. Em seguida, quando preparar relatórios um auditor, poder concentrar nas revisões de acesso no âmbito de uma iniciativa específico.  Programas e resultados de revisão do acesso são visíveis para os utilizadores a função de Administrador Global, o administrador de segurança ou o leitor de segurança.

Para obter uma lista de programas, vá para o [acesso revê página](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) e selecione **programas**.

**Programa predefinido** sempre está presente. Se tiver uma função de administrador global, pode criar programas adicionais. Por exemplo, pode optar por ter um programa para cada iniciativa de compatibilidade ou o objetivo de negócio.

Se já não necessita de um programa e não tem quaisquer controlos ligados ao mesmo, podem eliminá-lo.

## <a name="next-steps"></a>Passos Seguintes

- [Criar uma revisão de acesso para os membros de um grupo ou o acesso a uma aplicação](active-directory-azure-ad-controls-create-access-review.md)
- [Obter os resultados da revisão do acesso](active-directory-azure-ad-controls-retrieve-access-review.md)
