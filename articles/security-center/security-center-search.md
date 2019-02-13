---
title: Pesquisa do Centro de segurança do Azure | Documentos da Microsoft
description: Saiba como o Centro de segurança do Azure utiliza a pesquisa do Log Analytics para recuperar e analisar os seus dados de segurança.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: rkarlin
ms.openlocfilehash: c02a9f61a4a8b88f8b6c4d861f1a6cbe904ad70d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110545"
---
# <a name="azure-security-center-search"></a>Pesquisa do Centro de segurança do Azure
Centro de segurança do Azure utiliza [de pesquisa do Log Analytics](../log-analytics/log-analytics-log-searches.md) para recuperar e analisar os seus dados de segurança. O log Analytics inclui uma linguagem de consulta para obter e consolidar os dados rapidamente. No Centro de segurança, pode aproveitar a pesquisa do Log Analytics para criar consultas e analisar os dados recolhidos.

Pesquisa está disponível no escalão gratuito e escalão Standard do Centro de segurança.  Os dados disponíveis em suas pesquisas de registo estão dependentes de nível da camada aplicado à sua área de trabalho.  Consulte o Centro de segurança [página de preços](../security-center/security-center-pricing.md) para obter mais informações.


> [!NOTE]
> Centro de segurança não-guardar dados de segurança para uma área de trabalho sob o escalão gratuito. Pode enviar uma variedade de registos para uma área de trabalho sob o escalão gratuito e pesquisa nesses dados, mas os resultados da pesquisa não incluem dados do Centro de segurança. Centro de segurança apenas guarda dados numa área de trabalho sob o escalão Standard.
>
>

## <a name="access-search"></a>Pesquisa de acesso
1. No menu principal do Centro de segurança, selecione **pesquisa**.

  ![Selecione a pesquisa de registos][1]

2. Centro de segurança apresenta uma lista de todas as áreas de trabalho em suas subscrições do Azure. Selecione uma área de trabalho. (Se tiver apenas uma área de trabalho, este Seletor de área de trabalho não serão apresentados.)

  ![Selecione uma área de trabalho][2]

3. **Pesquisa de registos** abre. Para consultar mais dados na área de trabalho selecionado, introduza esta consulta de exemplo:

  SecurityEvent | onde EventID = = 4625 | resumir count () by TargetAccount

  Resultado mostra todas as contas que falharam ao início de sessão (evento 4625).

  ![Resultados da pesquisa][3]

Ver [linguagem de consulta do Log Analytics](../log-analytics/log-analytics-search-reference.md) para obter mais informações sobre como consultar os dados na área de trabalho selecionada.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como acessar a pesquisa no Centro de segurança. Centro de segurança utiliza a pesquisa do Log Analytics. Para saber mais sobre a pesquisa do Log Analytics, consulte:

- [O que é o Log Analytics?](../log-analytics/log-analytics-overview.md) – Descrição geral no Log Analytics
- [Compreender a pesquisa do Log Analytics](../log-analytics/log-analytics-log-search-new.md) - descreve como as pesquisas de registos são usadas no Log Analytics e fornece os conceitos que devem ser compreendidos antes de criar uma pesquisa de registos
- [Encontrar os dados com pesquisas de registos no Log Analytics](../log-analytics/log-analytics-log-searches.md) – Tutorial sobre como utilizar a pesquisa de registos
- [Referência de pesquisa do log Analytics](../log-analytics/log-analytics-search-reference.md) – descreve a linguagem de consulta do Log Analytics

Para saber mais sobre o Centro de segurança, veja:

- [Descrição geral do Centro de segurança](security-center-intro.md) – capacidades do Centro de segurança descreve de principais

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
