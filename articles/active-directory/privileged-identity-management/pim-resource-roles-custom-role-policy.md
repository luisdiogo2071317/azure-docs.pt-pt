---
title: Utilize funções personalizadas para as definições do destino Privileged Identity Management para recursos do Azure | Microsoft Docs
description: Descreve como utilizar funções personalizadas para recursos do Azure com o PIM.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: e3df34e761f17aa3c1949af390e57360d84a304f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-custom-roles-to-target-privileged-identity-management-settings"></a>Utilize funções personalizadas para as definições do destino Privileged Identity Management

Poderá ter de aplicar definições de Privileged Identity Management (PIM) strict para alguns membros de uma função, ao fornecer autonomia em maior para outras pessoas. Considere um cenário em que a sua organização hires várias associa contrato para ajudá-lo no desenvolvimento de uma aplicação que será executada uma subscrição do Azure.

Como um administrador de recursos, pretende que os funcionários sejam elegíveis para o acesso sem necessidade de aprovação. No entanto, todos os associa de contrato deve ser aprovado quando solicitarem o acesso aos recursos da organização.

Siga os passos descritos na secção seguinte para configurar as definições de PIM de destino para funções de recursos do Azure.

## <a name="create-the-custom-role"></a>Criar a função personalizada

Para criar uma função personalizada para um recurso, siga os passos descritos no [criar funções personalizadas para controlo de acesso em funções do Azure](../role-based-access-control-custom-roles.md).

Quando cria uma função personalizada, incluem um nome descritivo para que pode facilmente Lembre-se de que função incorporada que se destina a duplicado.

> [!NOTE]
> Certifique-se de que a função personalizada é um duplicado da função incorporada que pretende duplicado e que o seu âmbito corresponde da função incorporada.

## <a name="apply-pim-settings"></a>Aplicar as definições do PIM

Depois da função é criada no seu inquilino, no portal do Azure, avance para o **Privileged Identity Management - recursos do Azure** painel. Selecione o recurso que se aplica a função.

![O "Privileged Identity Management - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[Configurar as definições de função do PIM](pim-resource-roles-configure-role-settings.md) que devem ser aplicadas a estes membros da função.

Por fim, [atribuir funções](pim-resource-roles-assign-roles.md) para o grupo distinto de membros que pretende visar com estas definições.

## <a name="next-steps"></a>Passos Seguintes

[Os proprietários de subscrição de revisão e acesso](pim-resource-roles-perform-access-review.md)
