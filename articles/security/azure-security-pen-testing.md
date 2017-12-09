---
title: Teste de caneta | Microsoft Docs
description: "O artigo fornece uma descrição geral de penetração testar o processo de (pentest) e como executar pentest contra as suas aplicações em execução na infraestrutura do Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2017
ms.author: yurid
ms.openlocfilehash: dd5c874406ec184a2526a9eb0843cd6f3b6b3aa1
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="pen-testing"></a>Teste de caneta
Uma das vantagens da utilização do Azure para implementação e teste de aplicações é que pode obter rapidamente ambientes criados.  Não tem de se preocupar com requisitioning, aquisição, "racking e empilhamento" hardware os seus próprios no local.

Esta é uma grande – mas ainda tem de certificar-se de que efetua a segurança normal diligence devida. Uma das coisas que precisa de fazer é penetração teste as aplicações que implementa no Azure.

Já poderá saber que a Microsoft efetua [penetração testes do nosso ambiente do Azure](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Isto ajuda a unidade melhoramentos do Azure. 

Iremos não o caneta testar a aplicação por si, mas compreendemos que irá pretender e terá de executar as suas próprias aplicações de teste de caneta. É uma boa coisa, porque quando melhorar a segurança das suas aplicações, de ajudar a tornar a todo o ecossistema do Azure mais seguros.

O que fazer?

A partir do dia 15 de Junho de 2017, Microsoft já não necessita de aprovação anterior ao realizar uma penetração testes relativamente aos recursos do Azure. Os clientes que pretendem formally documento futuros penetração testar ações de envolvimento com o Microsoft Azure são encorajados a preencher a [formulário de notificação de teste do Azure Service penetração](https://portal.msrc.microsoft.com/engage/pentest). Esta é apenas relacionados com o processo para o Microsoft Azure e não é aplicável a qualquer outro serviço de nuvem da Microsoft. 

>[!IMPORTANT] 
>Enquanto notificar Microsoft a respeito de caneta atividades de teste já não é necessário clientes tem ainda está em conformidade com a [Microsoft Cloud Unified penetração testar regras do Engagement](https://technet.microsoft.com/en-us/mt784683). 

Testes padrão, que pode realizar incluem:

* Os testes no seus pontos finais para descobrir o [10 vulnerabilidades de principais de aplicação do abra Web projeto segurança (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Fuzz testar](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) dos seus pontos finais
* [Análise de porta](https://en.wikipedia.org/wiki/Port_scanner) dos seus pontos finais

É de um tipo de teste que não é possível executar quaisquer tipos de [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) ataque. Isto inclui a iniciar um ataque DoS próprio ou efetuar testes relacionados que podem determinar, demonstrar ou simular qualquer tipo de um ataque.

## <a name="next-steps"></a>Passos seguintes

- Pronto para começar a caneta irá testar as suas aplicações alojadas no Microsoft Azure? Se Sim, em seguida, vá na ativação pós-falha para o [descrição geral do teste penetração](https://technet.microsoft.com/library/mt784683.aspx) página (e clique em criar uma pedido de teste na parte inferior da página. 