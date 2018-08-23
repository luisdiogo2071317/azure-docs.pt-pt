---
title: 'Resolução de problemas: dados em falta nos registos de atividades transferidos do Azure Active Directory | Microsoft Docs'
description: Fornece uma resolução para os dados em falta nos registos de atividades transferidos do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9138da42eeb87e45b86be10aff67792ee6de09b4
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060244"
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Não consigo encontrar todos os dados nos registos de atividades do Azure Active Directory que transferi


## <a name="symptoms"></a>Sintomas

Transferi os registos de atividades (auditorias ou inícios de sessão) e não vejo todos os registos para o período de tempo que escolhi. Porquê? 

 ![Relatórios](./media/troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Causa

Quando transfere registos de atividades no portal do Azure, limitamos o dimensionamento para 5000 registos, ordenados pelos mais recente primeiro. 

## <a name="resolution"></a>Resolução

Pode tirar partido das [APIs de Relatórios do Azure AD](concept-reporting-api.md) para obter até um milhão de registos num determinado período. A nossa abordagem recomendada é executar um script com base numa agenda que chama às APIs de relatórios para obter registos de uma forma incremental durante um período de tempo (por exemplo, diária ou semanal).

## <a name="next-steps"></a>Passos Seguintes
Veja as [FAQ de relatórios do Azure Active Directory](reports-faq.md).

