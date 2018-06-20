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
ms.component: compliance-reports
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d0638404ec6f5b6d13aa207ef54913c1bd3ecc1a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232504"
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Não é possível localizar a quaisquer dados nos registos de atividade do Azure Active Directory que posso transferido


## <a name="symptoms"></a>Sintomas

Transferi os registos de atividades (auditorias ou inícios de sessão) e não vejo todos os registos para o período de tempo que escolhi. Porquê? 

 ![Relatórios](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Causa

Quando transferir os registos de atividade no portal do Azure, limitamos a escala a 5000 registos, ordenados por mais recente primeiro. 

## <a name="resolution"></a>Resolução

Pode tirar partido das [APIs de Relatórios do Azure AD](active-directory-reporting-api-getting-started.md) para obter até um milhão de registos num determinado período. A nossa abordagem recomendada é executar um script de forma agendada que chama as APIs do Reporting Services para obter registos de forma incremental durante um período de tempo (por exemplo, diária ou semanal).

## <a name="next-steps"></a>Passos Seguintes
Veja as [FAQ de relatórios do Azure Active Directory](active-directory-reporting-faq.md).

