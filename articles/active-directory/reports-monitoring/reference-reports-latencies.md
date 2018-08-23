---
title: O Azure Active Directory latências dos relatórios | Documentos da Microsoft
description: Saiba mais sobre a quantidade de tempo que demora para eventos de relatório a aparecer no portal do Azure
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 12/15/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f0de2f8700bef83b5a8a9303e90c97aab29722a3
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42060193"
---
# <a name="azure-active-directory-reporting-latencies"></a>O Azure Active Directory latências dos relatórios

Com o [reporting](../active-directory-preview-explainer.md) no Azure Active Directory, obtém todas as informações de que precisa para determinar o estado de funcionamento do ambiente. A quantidade de tempo que demora para informar os dados a aparecer no portal do Azure é também conhecida como latência. 

Este tópico lista as informações de latência para as categorias de todos os relatórios no portal do Azure. 


## <a name="activity-reports"></a>Relatórios de atividade

Existem duas áreas de relatórios de atividade:

- **Atividades de início de sessão** – Informações sobre a utilização de aplicações geridas e atividades de início de sessão do utilizador
- **Registos de auditoria** - informações de atividades do sistema sobre gestão de utilizadores e de grupos, as suas aplicações geridas e atividades de diretório

A tabela seguinte lista as informações de latência para relatórios de atividade.

| Relatório | Latência (P95) |Latência (P99)|
| :-- | --- | --- | 
| Registos de auditoria | 2 minutos  | 5 minutos  |
| Inícios de sessão | 2 minutos  | 5 minutos |







## <a name="security-reports"></a>Relatórios de segurança

Existem duas áreas de relatórios de segurança:

- **Inícios de sessão de risco** – Um início de sessão de risco é um indicador de uma tentativa de início de sessão que pode ter sido efetuada por alguém que não é o proprietário legítimo de uma conta de utilizador. 
- **Utilizadores sinalizados para risco** – Um utilizador de risco é um indicador de uma conta de utilizador que pode ter sido comprometida. 

A tabela seguinte lista as informações de latência para relatórios de segurança.

| Relatório | Mínimo | Média | Máximo |
| :-- | --- | --- | --- |
| Utilizadores em risco          | 5 minutos   | 15 minutos  | 2 horas  |
| Inícios de sessão de risco         | 5 minutos   | 15 minutos  | 2 horas  |

## <a name="risk-events"></a>Eventos de risco

O Azure Active Directory utiliza algoritmos de aprendizagem automática adaptáveis e heurística para detetar as ações suspeitas relacionadas com as contas de utilizador. Cada detetado, ação suspeita é armazenada num evento de risco de chamada de registo.

A tabela seguinte lista as informações de latência para eventos de risco.

| Relatório | Mínimo | Média | Máximo |
| :-- | --- | --- | --- |
| Inícios de sessão de endereços IP anónimos |5 minutos |15 Minutos |2 horas |
| Inícios de sessão de localizações desconhecidas |5 minutos |15 Minutos |2 horas |
| Utilizadores com fuga de credenciais |2 horas |4 horas |8 horas |
| Deslocação impossível para localizações atípicas |5 minutos |1 hora |8 horas  |
| Inícios de sessão de dispositivos infetados |2 horas |4 horas |8 horas  |
| Inícios de sessão de endereços IP com atividade suspeita |2 horas |4 horas |8 horas  |



## <a name="next-steps"></a>Passos Seguintes

Se quiser saber mais sobre os relatórios de atividade no portal do Azure, veja:

- [Relatórios de atividade de início de sessão no portal do Azure Active Directory](concept-sign-ins.md)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](concept-audit-logs.md)

Se quiser saber mais sobre os relatórios de segurança no portal do Azure, veja:

- [Utilizadores no relatório de segurança de risco no portal do Azure Active Directory](concept-user-at-risk.md)
- [Relatório de inícios de sessão de risco no portal do Azure Active Directory](concept-risky-sign-ins.md)

Se quiser saber mais sobre eventos de risco, consulte [eventos de risco do Azure Active Directory](concept-risk-events.md).
