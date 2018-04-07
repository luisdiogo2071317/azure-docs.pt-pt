---
title: Privileged Identity Management para recursos do Azure - revisão do acesso completo para recursos do Azure | Microsoft Docs
description: Descreve como realizar uma revisão do acesso para recursos do Azure.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5ce02c2d27ec3de87fe44e9c904f19409600e5c5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---finish-access-review"></a>Revisão do acesso do Privileged Identity Management - função de recursos - concluir
Os administradores de com função privilegiada podem rever uma vez acesso privilegiado um [revisão de segurança tiver sido iniciado](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) para os recursos do Azure irá enviar automaticamente um e-mail a pedir aos utilizadores para rever o respetivo acesso. Se um utilizador não foi possível obter um e-mail, pode enviar-lhes as instruções [como executar uma revisão de segurança](pim-resource-roles-perform-access-review.md).

Após o período de revisão de segurança, ou todos os utilizadores tem concluído os respetivos rever automática, siga os passos neste artigo para gerir a rever e ver os resultados.

## <a name="manage-security-reviews"></a>Gerir revisões de segurança
1. Vá para o [portal do Azure](https://portal.azure.com/) e selecione o **recursos do Azure** aplicação no seu dashboard.
2. Selecione o tipo de recurso.
3. Selecione o **aceder revisões** secção do dashboard.
![](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)
4. Selecione a revisão do acesso que pretende gerir.

No painel de detalhes da revisão do acesso, existem várias opções para gerir essa revisão.
![](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Parar
Uma data de fim de ter todas as revisões de acesso, mas pode utilizar o **parar** botão para terminar numa fase inicial. Se os utilizadores ainda não foram revistos neste momento, não seria capazes de depois de interromper a revisão. Não é possível reiniciar uma revisão depois-é foi parado.

### <a name="reset"></a>Repor
Só pode repor uma revisão do acesso para remover todas as decisões que efetuou no mesmo. Depois de repor a revisão do acesso, todos os utilizadores estão marcados como unreviewed novamente. 

### <a name="apply"></a>Aplicar
Uma revisão do acesso esteja concluída, ou porque foi atingido a data de fim ou parado-lo manualmente, o **aplicar** botão implementa o resultado a revisão. Se acesso um utilizador foi negado de revisão, este é o passo que irá remover a atribuição de função.  

### <a name="delete"></a>Eliminar
Se não estiver interessado em mais de revisão, elimine-o. O **eliminar** botão remove a revisão da aplicação do PIM.

## <a name="results"></a>Resultados
Ver e transferir uma lista dos resultados da sua revisão no separador de resultados. ![](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisores
Ver e adicionar revisores a revisão do acesso existentes. Notificar os revisores para concluir as revisões.
![](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



