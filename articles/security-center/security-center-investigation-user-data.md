---
title: Gerir os dados de utilizador encontrados numa investigação de centro de segurança do Azure | Microsoft Docs
description: " Saiba como gerir os dados de utilizador encontrados na funcionalidade de investigação do Centro de segurança do Azure. "
services: operations-management-suite
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: 6685db4eeda72928753c74c64b4b26539ccb1eb9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655717"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Gerir os dados de utilizador encontrados numa investigação de centro de segurança do Azure
Este artigo fornece informações sobre como gerir os dados de utilizador encontrados na funcionalidade de investigação do Centro de segurança do Azure. Investigação dados são armazenados no [Log Analytics do Azure](../log-analytics/log-analytics-overview.md) e exposta no Centro de segurança. Gestão de dados de utilizador inclui a capacidade de eliminar ou exportar dados.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Procurar e identificar os dados pessoais
No portal do Azure, pode utilizar o Centro de segurança [funcionalidade de investigação](../security-center/security-center-investigation.md) para procurar dados pessoais. A funcionalidade de investigação está disponível em **alertas de segurança**.

A funcionalidade de investigação mostra todas as entidades, informações de utilizador e dados sob a **entidades** separador.

## <a name="securing-and-controlling-access-to-personal-information"></a>Proteger e controlar o acesso a informações pessoais
Um utilizador do Centro de segurança atribuída a função de leitor de proprietário, Contribuidor ou administrador de conta podem aceder a dados de cliente dentro da ferramenta.

Consulte [funções incorporadas para controlo de acesso baseado em funções do Azure](../role-based-access-control/built-in-roles.md) para saber mais sobre as funções do leitor, proprietário e contribuinte. Consulte [os administradores da subscrição do Azure](../billing/billing-add-change-azure-subscription-administrator.md) para saber mais sobre a função de administrador de conta.

## <a name="deleting-personal-data"></a>Eliminar dados pessoais
Um utilizador do Centro de segurança atribuída a função de proprietário, contribuinte, ou o administrador de conta pode eliminar as informações de investigação.

Para eliminar uma investigação, pode submeter um `DELETE` pedido para a API de REST do Azure Resource Manager:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

O `incidentName` entrada pode ser encontrada por listar todos os incidentes utilizando um `GET` pedido:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Exportar os dados pessoais
Um utilizador do Centro de segurança atribuída a função de proprietário, contribuinte, ou o administrador da conta pode exportar as informações de investigação. Para exportar as informações de investigação, vá para o **entidades** separador copie e cole as informações relevantes.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a gestão de dados do utilizador, consulte [gerir os dados de utilizador no Centro de segurança do Azure](security-center-privacy.md).
