---
title: Diagnosticar exceções de runtime com o Azure Application Insights | Microsoft Docs
description: Tutorial para localizar e diagnosticar exceções de runtime na sua aplicação com o Azure Application Insights.
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 115611c5d4eeffb0f0600dd0a792ee9f80247e36
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
ms.locfileid: "27998054"
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Localizar e diagnosticar exceções de runtime com o Azure Application Insights

O Azure Application Insights recolhe telemetria da sua aplicação para ajudar a identificar e diagnosticar exceções de runtime.  Este tutorial guia-o ao longo deste processo com a sua aplicação.  Saiba como:

> [!div class="checklist"]
> * Modificar o seu projeto para ativar o controlo de exceções
> * Identificar exceções de diferentes componentes da aplicação
> * Ver detalhes de uma exceção
> * Transferir um instantâneo da exceção para o Visual Studio para depuração
> * Analisar os detalhes de pedidos falhados com a linguagem de consulta
> * Criar um novo item de trabalho para corrigir o código com erros


## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

- Instale o [2017 do Visual Studio](https://www.visualstudio.com/downloads/) com as seguintes cargas de trabalho:
    - Desenvolvimento ASP.NET e Web
    - Desenvolvimento do Azure
- Transfira e instale o [Visual Studio Snapshot Debugger](http://aka.ms/snapshotdebugger).
- Ative o [Visual Studio Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger).
- Implemente uma aplicação .NET no Azure e [ative o Application Insights SDK](app-insights-asp-net.md). 
- O tutorial controla a identificação de uma exceção na sua aplicação, por isso, modifique o código no seu ambiente de desenvolvimento ou de teste para gerar uma exceção. 

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Analisar falhas
O Application Insights recolhe as falhas ocorridas na sua aplicação e permite-lhe ver a frequência das mesmas em operações diferentes para o ajudar a centrar os seus esforços naquelas cujo impacto é mais significativo.  Pode, em seguida, desagregar os detalhes destas falhas para identificar a raiz do problema.   

1. Selecione **Application Insights** e, em seguida, a sua subscrição.  
2. Para abrir o painel **Falhas**, selecione **Falhas** no menu **Investigar** ou clique no gráfico **Pedidos falhados**.

    ![Pedidos falhados](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

3. O painel **Pedidos falhados** mostra a contagem de pedidos falhados e o número de utilizadores afetados para cada operação da aplicação.  A ordenação destas informações por utilizador permite-lhe identificar as falhas com maior impacto sobre os utilizadores.  Neste exemplo, **GET Employees/Create** e **GET Customers/Details** são candidatos prováveis a serem investigados devido ao número elevado de falhas e utilizadores afetados que apresentam.  Se selecionar uma determinada operação serão mostradas mais informações sobre a mesma no painel à direita.

    ![Painel de pedidos falhados](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

4. Reduza o intervalo de tempo para ampliar o período em que a taxa de falhas mostra um pico.

    ![Janela de pedidos falhados](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

5. Clique em **Ver Detalhes** para ver os detalhes da operação.  Os detalhes incluem um gráfico Gantt que mostra duas dependências falhadas, as quais coletivamente demoraram quase meio segundo a serem concluídas.  Saiba mais sobre como analisar problemas de desempenho ao completar o tutorial [Localizar e diagnosticar problemas de desempenho com o Azure Application Insights](app-insights-tutorial-performance.md).

    ![Detalhes de pedidos falhados](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

6. Os detalhes das operações também mostram um FormatException que parece estar na origem da falha.  Clique na exceção ou na contagem **Três principais tipos de exceção** para ver os detalhes.  Pode ver que o problema deve-se a um código postal inválido.

    ![Detalhes da exceção](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)

> [!NOTE]
Ative a [experiência de pré-visualização](app-insights-previews.md) "Detalhes unificados: Diagnósticos de Transação E2E" para ver toda a telemetria relacionada do lado do servidor, como pedidos, dependências, exceções, rastreios, eventos, entre outros, numa única vista de ecrã inteiro. 

Com a pré-visualização ativada, pode ver o tempo despendido em chamadas de dependência, juntamente com quaisquer falhas ou exceções, numa experiência unificada. Para as transações entre componentes, o gráfico Gantt, juntamente com o painel de detalhes, pode ajudá-lo a diagnosticar rapidamente o componente, a dependência ou a exceção onde reside a raiz do problema. Pode expandir a secção inferior para ver a sequência de tempo de quaisquer rastreios ou eventos recolhidos para a operação de componente selecionada. [Saiba mais sobre a nova experiência](app-insights-transaction-diagnostics.md)  

![Diagnóstico da transação](media/app-insights-tutorial-runtime-exceptions/e2e-transaction-preview.png)

## <a name="identify-failing-code"></a>Identificar o código com falhas
O Snapshot Debugger recolhe instantâneos das exceções mais frequentes na sua aplicação para o ajudar a diagnosticar a raiz do problema na produção.  Pode ver instantâneos de depuração no portal para visualizar a pilha de chamadas e inspecionar as variáveis em cada frame de pilha de chamadas. Em seguida, pode depurar o código fonte ao transferir o instantâneo e abri-lo no Visual Studio 2017.

1. Nas propriedades da exceção, clique em **Abrir instantâneo de depuração**.
2. O painel **Depurar Instantâneo** é aberto com a pilha de chamadas relativas ao pedido.  Clique num dos métodos disponíveis para ver os valores de todas as variáveis locais no momento do pedido.  Tomando como ponto de partida o método principal neste exemplo, é possível ver variáveis locais que não têm qualquer valor.

    ![Depurar o instantâneo](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. A primeira chamada com valores válidos é **ValidZipCode** e podemos ver que foi fornecido um código postal com letras que não é possível converter num número inteiro.  Aparentemente, este é o erro no código que precisa de ser corrigido.

    ![Depurar o instantâneo](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Para transferir este instantâneo para o Visual Studio, onde poderemos localizar o código real que precisa de ser corrigido, clique em **Transferir Instantâneo**.
6. O instantâneo é carregado para o Visual Studio.
7. Agora, pode executar uma sessão de depuração no Visual Studio que identifica rapidamente a linha de código responsável pela exceção.

    ![Exceção no código](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Utilizar dados de análise
Todos os dados recolhidos pelo Application Insights são armazenados no Azure Log Analytics, o qual fornece uma linguagem de consulta avançada que lhe permite analisar os dados de diversas formas.  Podemos utilizar estes dados para analisar os pedidos que geraram a exceção que estamos a pesquisar. 

8. Clique nas informações de CodeLens por cima do código para ver a telemetria fornecida pelo Application Insights.

    ![Código](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Clique em **Analisar o impacto** para abrir o Application Insights Analytics.  Este é povoado com várias consultas que fornecem detalhes sobre os pedidos falhados, tais como os utilizadores, os browsers e as regiões afetados.<br><br>![Análise](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Adicionar item de trabalho
Se ligar o Application Insights a um sistema de controlo, como o Visual Studio Team Services ou o GitHub, pode criar um item de trabalho diretamente a partir do Application Insights.

1. Volte ao painel **Propriedades da Exceção** no Application Insights.
2. Clique em **Novo Item de Trabalho**.
3. O painel **Novo Item de Trabalho** é aberto com os detalhes sobre a exceção já povoados.  Pode adicionar as informações adicionais que quiser antes de o guardar.

    ![Novo Item de Trabalho](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Passos seguintes
Agora que aprendeu a identificar exceções de runtime, avance para o próximo tutorial para saber como identificar e diagnosticar problemas de desempenho.

> [!div class="nextstepaction"]
> [Identificar problemas de desempenho](app-insights-tutorial-performance.md)