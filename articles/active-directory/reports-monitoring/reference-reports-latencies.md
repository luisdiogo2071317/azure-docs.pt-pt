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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: e5ceae2959f79c677f5b89c0c3f0a487f92ad1c6
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623184"
---
# <a name="azure-active-directory-reporting-latencies"></a>O Azure Active Directory latências dos relatórios

Latência é a quantidade de tempo necessário para o Azure Active Directory (Azure AD) dados de relatórios a aparecer no [portal do Azure](https://portal.azure.com). Este artigo apresenta a latência esperada para os diferentes tipos de relatórios. 

## <a name="activity-reports"></a>Relatórios de atividade

Existem dois tipos de relatórios de atividade:

- [Inícios de sessão](concept-sign-ins.md) – fornece informações sobre a utilização de aplicações geridas e utilizador atividades de início de sessão
- [Registos de auditoria](concept-audit-logs.md) -fornece informações de atividade do sistema sobre utilizadores e grupos, aplicações geridas e atividades de diretório

A tabela seguinte lista as informações de latência para relatórios de atividade. 

> [!NOTE]
> **Latência (o percentil 95)** refere-se até ao momento em que serão comunicadas 95% dos registos, e **latência (99 por cento dos)** refere-se ao tempo pelo qual serão comunicadas 99% dos registos. 
>

| Relatório | Latência (o percentil 95) |Latência (99 por cento dos)|
| :-- | --- | --- | 
| Registos de auditoria | 2 mins  | 5 mins  |
| Inícios de sessão | 2 mins  | 5 mins |

## <a name="security-reports"></a>Relatórios de segurança

Existem dois tipos de relatórios de segurança:

- [Inícios de sessão de risco](concept-risky-sign-ins.md) – Um início de sessão de risco é um indicador de uma tentativa de início de sessão que pode ter sido efetuada por alguém que não é o proprietário legítimo de uma conta de utilizador. 
- [Utilizadores sinalizados para risco](concept-user-at-risk.md) – Um utilizador de risco é um indicador de uma conta de utilizador que pode ter sido comprometida. 

A tabela seguinte lista as informações de latência para relatórios de segurança.

| Relatório | Mínimo | Média | Máximo |
| :-- | --- | --- | --- |
| Utilizadores em risco          | 5 minutos   | 15 minutos  | 2 horas  |
| Inícios de sessão de risco         | 5 minutos   | 15 minutos  | 2 horas  |

## <a name="risk-events"></a>Eventos de risco

Azure AD utiliza de machine learning e heurística de algoritmos para detectar ações suspeitas relacionadas com as contas de utilizador. Cada detetado, ação suspeita é armazenada num registo denominado um **evento de risco**.

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

* [Descrição geral de relatórios do Azure AD](overview-reports.md)
* [Acesso programático aos relatórios do Azure AD](concept-reporting-api.md)
* [Eventos de risco do Azure Active Directory](concept-risk-events.md)
