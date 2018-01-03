---
title: "Azure Active Directory latências de relatórios | Microsoft Docs"
description: "Saiba mais sobre a quantidade de tempo que demora para eventos de relatório apresentar no portal do Azure"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: 5ec41817fede495b8262e28d2d614a480d98ff3b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory latências de relatórios

Com [reporting](active-directory-preview-explainer.md) no Azure Active Directory, obtém todas as informações necessárias para determinar a forma como o ambiente está a fazer. A quantidade de tempo que demora para dados de relatórios apareçam no portal do Azure também é conhecido como latência. 

Este tópico lista as informações de latência para as categorias de todos os relatórios no portal do Azure. 


## <a name="activity-reports"></a>Relatórios de atividade

Existem duas áreas de criação de relatórios de atividade:

- **Atividades de início de sessão** – Informações sobre a utilização de aplicações geridas e atividades de início de sessão do utilizador
- **Registos de auditoria** - informações de atividades do sistema sobre gestão de utilizadores e de grupos, as suas aplicações geridas e atividades de diretório

A tabela seguinte lista as informações de latência para relatórios de atividade.

| Relatório | Mínimo | Média | Observações |
| :-- | --- | --- | :-- |
| Registos de auditoria | 30 minutos  | 1 hora  |Em alguns casos, pode demorar até 2 horas para dados de atividade de auditoria apresentar.|
| Inícios de sessão | 15 minutos  | 2 horas |Em alguns casos, pode demorar até 24 horas para dados de atividade de início de sessão apresentar. Isto inclui dados de atividade de inícios de sessão provenientes das aplicações do office legado. |







## <a name="security-reports"></a>Relatórios de segurança

Existem duas áreas de criação de relatórios de segurança:

- **Inícios de sessão de risco** – Um início de sessão de risco é um indicador de uma tentativa de início de sessão que pode ter sido efetuada por alguém que não é o proprietário legítimo de uma conta de utilizador. 
- **Utilizadores sinalizados para risco** – Um utilizador de risco é um indicador de uma conta de utilizador que pode ter sido comprometida. 

A tabela seguinte lista as informações de latência para relatórios de segurança.

| Relatório | Mínimo | Média | Máximo |
| :-- | --- | --- | --- |
| Utilizadores em risco          | 5 minutos   | 15 minutos  | 2 horas  |
| Inícios de sessão de risco         | 5 minutos   | 15 minutos  | 2 horas  |

## <a name="risk-events"></a>Eventos de risco

Azure Active Directory utiliza adaptável algoritmos de aprendizagem automática e heurística para detetar as ações suspeitas que estão relacionados com as contas de utilizador. Cada detetada ação suspeita é armazenada um evento de risco chamada de registo.

A tabela seguinte lista as informações de latência para eventos de risco.

| Relatório | Mínimo | Média | Máximo |
| :-- | --- | --- | --- |
| Inícios de sessão de endereços IP anónimos |5 minutos |15 minutos |2 horas |
| Inícios de sessão de localizações desconhecidas |5 minutos |15 minutos |2 horas |
| Utilizadores com fuga de credenciais |2 horas |4 horas |8 horas |
| Deslocação impossível para localizações atípicas |5 minutos |1 hora |8 horas  |
| Inícios de sessão de dispositivos infetados |2 horas |4 horas |8 horas  |
| Inícios de sessão de endereços IP com atividade suspeita |2 horas |4 horas |8 horas  |



## <a name="next-steps"></a>Passos Seguintes

Se pretender saber mais sobre os relatórios de atividade no portal do Azure, consulte:

- [Relatórios de atividade de início de sessão no portal do Azure Active Directory](active-directory-reporting-activity-sign-ins.md)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](active-directory-reporting-activity-audit-logs.md)

Se pretender saber mais sobre os relatórios de segurança no portal do Azure, consulte:

- [Utilizadores no relatório de segurança de risco no portal do Azure Active Directory](active-directory-reporting-security-user-at-risk.md)
- [Relatório de risco inícios de sessão no portal do Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md)

Se pretender saber mais sobre eventos de risco, consulte [eventos de risco do Azure Active Directory](active-directory-reporting-risk-events.md).
