---
title: 'Descrição geral: Executar uma revisão do acesso numa Privileged Identity Management para recursos do Azure | Microsoft Docs'
description: Este documento descreve como executar uma revisão de acesso no PIM para recursos do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e7fa85b4c6c84792a77972e2ad75ebaaf63a0e4c
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233721"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Utilize um dashboard de recursos para executar uma revisão do acesso

Pode utilizar um dashboard de recursos para executar uma revisão do acesso no Privileged Identity Management (PIM) para recursos do Azure. O dashboard de vista de administrador tem três componentes principais:

- Uma representação gráfica de ativações de função de recursos.
- Dois gráficos que apresentam a distribuição das atribuições de funções por tipo de atribuição.
- Uma área de dados relativas à nova atribuições de funções.

![Captura de ecrã do dashboard de vista de administração, gráficos e gráficos](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Captura de ecrã do dashboard de vista de administração, listas de dados](media/azure-pim-resource-rbac/role-settings.png)

A representação gráfica de ativações de função de recursos abrange os últimos sete dias. Estes dados tem um âmbito para o recurso selecionado e mostra ativações para as funções mais comuns (proprietário, contribuinte, administrador de acesso de utilizador) e para todas as funções combinadas.

À direita do gráfico ativações, dois gráficos mostra a distribuição das atribuições de funções por tipo de atribuição de utilizadores e grupos. Pode alterar o valor para uma percentagem (ou vice-versa), selecionando um setor do gráfico.

Abaixo os gráficos, consulte o número de utilizadores e grupos com atribuições de funções de novo ao longo dos últimos 30 dias e uma lista de funções ordenados por atribuições totais (por ordem descendente).


