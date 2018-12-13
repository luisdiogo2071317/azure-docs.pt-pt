---
title: Automatizar processos de Log Analytics do Azure com o Microsoft Flow
description: Saiba como pode utilizar o Microsoft Flow para rapidamente automatizar processos repetíveis ao utilizar o conector Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
ms.service: log-analytics
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 63f9417a65fbe3141907dc7d7d64da06eec7e93f
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882846"
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a>Automatizar processos de Log Analytics com o conector para o Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) permite-lhe criar fluxos de trabalho automatizados através de centenas de ações para uma variedade de serviços. Saída de uma ação pode ser utilizada como entrada para outro, permitindo que crie a integração entre diferentes serviços.  O conector Azure Log Analytics para o Microsoft Flow permite-lhe criar fluxos de trabalho que incluem dados obtidos através de pesquisas de registos no Log Analytics.

Por exemplo, pode utilizar o Microsoft Flow a utilizar dados do Log Analytics numa notificação de e-mail do Office 365, criar um bug no Azure DevOps ou publicar uma mensagem Slack.  Pode acionar um fluxo de trabalho por uma agenda simples ou de alguma ação num serviço ligado, como quando é recebido um e-mail ou um tweet.  

O tutorial neste artigo mostra-lhe como criar um fluxo que envia automaticamente os resultados de uma pesquisa de registos do Log Analytics por e-mail, apenas um exemplo de como pode utilizar o Log Analytics no Microsoft Flow. 


## <a name="step-1-create-a-flow"></a>Passo 1: Criar um fluxo
1. Inicie sessão no [Microsoft Flow](https://flow.microsoft.com)e selecione **meus fluxos**.
2. Clique em **+ criar do zero**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Passo 2: Criar um acionador para o fluxo
1. Clique em **procurar entre centenas de conectores e acionadores**.
2. Tipo **agenda** na caixa de pesquisa.
3. Selecione **agenda**e, em seguida, selecione **Schedule - Recurrence**.
4. Na **frequência** selecione **dia** e, no **intervalo** , digite **1**.<br><br>![Caixa de diálogo de Acionador do Microsoft Flow](media/flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Passo 3: Adicionar uma ação do Log Analytics
1. Clique em **+ novo passo**e, em seguida, clique em **adicionar uma ação**.
2. Procure **do Log Analytics**.
3. Clique em **Log Analytics do Azure – executar a consulta e visualizar os resultados**.<br><br>![Execute a janela de consulta do log Analytics](media/flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>Passo 4: Configurar a ação do Log Analytics

1. Especifique os detalhes para a área de trabalho incluindo o ID de subscrição, grupo de recursos e o nome de área de trabalho.
2. Adicione a seguinte consulta do Log Analytics para o **consulta** janela.  Isso é apenas uma consulta de exemplo e pode substituir com qualquer outro que retorna dados.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
```

2. Selecione **tabela HTML** para o **tipo de gráfico**.<br><br>![Ação de análise de registo](media/flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>Passo 5: Configurar o fluxo para enviar e-mail

1. Clique em **novo passo**e, em seguida, clique em **+ adicionar uma ação**.
2. Procure **Outlook do Office 365**.
3. Clique em **Office 365 Outlook – enviar um e-mail**.<br><br>![Janela de seleção do Outlook do Office 365](media/flow-tutorial/flow04.png)

4. Especifique o endereço de e-mail de um destinatário no **para** janela e um assunto para o e-mail na **assunto**.
5. Clique em qualquer ponto do **corpo** caixa.  R **conteúdo dinâmico** é aberta a janela com valores de ações anteriores.  
6. Selecione **corpo**.  Isso é que os resultados da consulta na ação do Log Analytics.
6. Clique em **Mostrar opções avançadas**.
7. Na **é HTML** caixa, selecione **Sim**.<br><br>![Janela de configuração de e-mail do Office 365](media/flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Passo 6: Guardar e testar o fluxo
1. Na **nome do fluxo** caixa, adicione um nome para o fluxo e, em seguida, clique em **criar fluxo**.<br><br>![Guardar fluxo](media/flow-tutorial/flow06.png)
2. O fluxo está agora criado e será executado após um dia em que é a agenda que especificou. 
3. Para testar imediatamente o fluxo, clique em **executar agora** e, em seguida **executar fluxo**.<br><br>![Executar fluxo](media/flow-tutorial/flow07.png)
3. Quando o fluxo estiver concluída, verifique o correio do destinatário que especificou.  Deve ter recebido um e-mail com um corpo de forma semelhante ao seguinte:<br><br>![E-mail de exemplo](media/flow-tutorial/flow08.png)


## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [pesquisas de registos no Log Analytics](../../azure-monitor/log-query/log-query-overview.md).
- Saiba mais sobre [Microsoft Flow](https://ms.flow.microsoft.com).



