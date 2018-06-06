---
title: Conclua uma revisão do acesso para recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Descreve como concluir uma revisão do acesso para recursos do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1dd5eb4a2a0a4edd15e5299754028570806db09b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699459"
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Conclua uma revisão do acesso para recursos do Azure utilizando o Privileged Identity Management
Os administradores de com função privilegiada podem rever o acesso privilegiado após um [revisão de segurança tiver sido iniciado](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) para recursos do Azure envia uma mensagem de e-mail que solicita aos utilizadores para rever o respetivo acesso automaticamente. Se um utilizador não receber uma mensagem de e-mail, pode enviar-lhes as instruções [como executar uma revisão de segurança](pim-resource-roles-perform-access-review.md).

Após o período de revisão de segurança ou depois de todos os utilizadores tem concluído os respetivos rever automática, siga os passos neste artigo para gerir a rever e ver os resultados.

## <a name="manage-security-reviews"></a>Gerir revisões de segurança
1. Aceda ao [Portal do Azure](https://portal.azure.com/). Em seguida, no dashboard, selecione o **recursos do Azure** aplicação.

2. Selecione o tipo de recurso.

3. Selecione o **aceder revisões** secção do dashboard.
![Revisões de acesso](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Selecione a revisão do acesso que pretende gerir.

No painel de detalhes de revisão do acesso, existem várias opções para gerir essa revisão. As opções são os seguintes:

![Opções para gerir uma revisão](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Parar
Uma data de fim de ter todas as revisões de acesso, mas pode utilizar o **parar** botão para terminar numa fase inicial. Todos os utilizadores que ainda não concluiu a respetiva revisão até essa altura não será possível terminar depois de interromper a revisão. Não é possível reiniciar uma revisão depois-é foi parado.

### <a name="reset"></a>Repor
Só pode repor uma revisão do acesso para remover todas as decisões que são efetuadas no mesmo. Depois de repor a revisão do acesso, todos os utilizadores estão marcados como unreviewed novamente. 

### <a name="apply"></a>Aplicar
Depois de concluída a revisão do acesso, utilize o **aplicar** botão para implementar o resultado a revisão. Se acesso um utilizador foi negado de revisão, este passo remove a atribuição de função.  

### <a name="delete"></a>Eliminar
Se não estão interessadas na revisão mais, elimine-o. O **eliminar** botão remove a revisão da aplicação do PIM.

## <a name="results"></a>Resultados
No **resultados** separador, ver e transferir uma lista dos resultados da sua revisão. 
![Separador de resultados](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisores
Ver e adicionar revisores a revisão do acesso existentes. Notificar os revisores para concluir as revisões.
![Adicionar revisores](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



