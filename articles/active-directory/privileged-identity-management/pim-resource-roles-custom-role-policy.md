---
title: Privileged Identity Management para recursos do Azure - funções personalizadas de utilização para as definições do destino PIM | Microsoft Docs
description: Descreve como utilizar funções personalizadas no PIM para recursos do Azure.
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
ms.openlocfilehash: 6336d99df1bbdd71c66a9757af1d9fb356a91bf6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="use-custom-roles-to-target-pim-settings"></a>Utilize funções personalizadas para as definições de PIM de destino

Poderá ser necessário aplicar strict PIM as definições para alguns membros de uma função, ao fornecer autonomia em maior para outras pessoas. Considere um cenário em que a sua organização hires sevral contrato associa para ajudá-lo no desenvolvimento de uma aplicação que será executada uma subscrição do Azure. 

Como um administrador de recursos, gostaria que os funcionários da sua organização para ter acesso elegível sem aprovação necessária, mas associa todas as do contrato tem de pesquisa de aprovação quando pedir a ativação. Siga os passos abaixo contorno o processo para ativar as definições do PIM direcionadas para funções de recursos do Azure.

## <a name="create-the-custom-role"></a>Criar a função personalizada

[Utilize este guia para criar uma função personalizada para um recurso](../../role-based-access-control/custom-roles.md).

Incluem um nome descritivo para que pode facilmente Lembre-se de que função incorporada que se destina a duplicado.

>[!NOTE]
>Certifique-se de que a função personalizada é um duplicado da função esperados e a função incorporada de corresponde ao seu âmbito.

## <a name="apply-pim-settings"></a>Aplicar as definições do PIM

Depois da função é criada no seu inquilino, navegue para o PIM e selecione o recurso que está no âmbito da função.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[Configurar as definições de função do PIM](pim-resource-roles-configure-role-settings.md) que devem ser aplicadas a estes membros

Por fim, [atribuir funções](pim-resource-roles-assign-roles.md) para o grupo distinto de membros que pretende visar com estas definições.

## <a name="next-steps"></a>Passos Seguintes

[Reveja os proprietários da subscrição](pim-resource-roles-perform-access-review.md)
