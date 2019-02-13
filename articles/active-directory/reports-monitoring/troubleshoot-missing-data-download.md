---
title: 'Resolução de problemas: Dados em falta nos registos de atividades transferidos do Azure Active Directory | Documentos da Microsoft'
description: Fornece uma resolução para os dados em falta nos registos de atividades transferidos do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ba5f803f59263f9bfebfd4ec8635d5cdd6d90a0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171779"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Não consigo encontrar todos os dados nos registos de atividades do Azure Active Directory que transferi

## <a name="symptoms"></a>Sintomas

Transferi os registos de atividades (auditorias ou inícios de sessão) e não vejo todos os registos para o período de tempo que escolhi. Porquê? 

 ![Relatórios](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Causa

Quando transfere registos de atividades no portal do Azure, limitamos o dimensionamento para 5000 registos, ordenados pelos mais recente primeiro. 

## <a name="resolution"></a>Resolução

Pode tirar partido das [APIs de Relatórios do Azure AD](concept-reporting-api.md) para obter até um milhão de registos num determinado período. Nossa abordagem recomendada é [executar um script de forma agendada](tutorial-signin-logs-download-script.md) que chama às APIs de relatórios para obter registos de uma forma incremental durante um período de tempo (por exemplo, diária ou semanal). 

## <a name="next-steps"></a>Passos Seguintes

* [FAQ de relatórios do Azure Active Directory](reports-faq.md)

