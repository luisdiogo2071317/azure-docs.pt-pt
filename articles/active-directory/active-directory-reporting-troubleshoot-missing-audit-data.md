---
title: "Resolução de problemas: dados em falta no registo de atividades do Azure Active Directory. | Microsoft Docs"
description: "Lista os vários relatórios disponíveis do Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 55587fb4ff6d8a2130eb838782a65788fb2dd2b3
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>Não consigo encontrar algumas ações que efetuei no registo de atividades do Azure Active Directory


## <a name="symptoms"></a>Sintomas

Efetuei algumas ações no portal do Azure e esperava ver os registos de auditoria dessas ações no painel `Activity logs > Audit Logs`, mas não consegui encontrá-los.

 ![Relatórios](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Causa

As ações não aparecem de imediato no registo Auditoria da Atividade. Pode demorar entre 15 minutos a uma hora para ver os registos de auditoria no portal desde que a operação foi efetuada.

## <a name="resolution"></a>Resolução

Aguarde entre 15 minutos a uma hora e veja se as ações aparecem no registo. Se continuar a não serem visualizadas, emita um pedido de suporte e vamos analisar o problema.


## <a name="next-steps"></a>Passos seguintes
Veja as [FAQ de relatórios do Azure Active Directory](active-directory-reporting-faq.md).

