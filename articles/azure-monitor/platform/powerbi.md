---
title: Importar dados do Azure Log Analytics no Power BI | Documentos da Microsoft
description: Power BI é um serviço de análise de negócios com base na cloud da Microsoft que fornece relatórios e visualizações avançadas para análise de diferentes conjuntos de dados.  Este artigo descreve como configurar e importar dados do Log Analytics no Power BI e configurá-lo para atualizar automaticamente.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 89be723b3f5791baf0a1fb161c92f590cc2677e0
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002659"
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Importar dados do Azure Log Analytics no Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) é um serviço de análise de negócios com base na cloud da Microsoft que fornece relatórios e visualizações avançadas para análise de diferentes conjuntos de dados.  Pode importar os resultados de uma pesquisa de registos do Log Analytics para um conjunto de dados do Power BI, para que possa tirar partido das respetivas funcionalidades, como combinar dados de diferentes origens e partilhar relatórios na web e em dispositivos móveis.

## <a name="overview"></a>Descrição geral
Para importar dados a partir de uma área de trabalho do Log Analytics no Power BI, crie um conjunto de dados no Power BI com base numa consulta de pesquisa de registos no Log Analytics.  A consulta é executada sempre que o conjunto de dados é atualizado.  Em seguida, pode criar relatórios do Power BI que utilizam dados do conjunto de dados.  Para criar o conjunto de dados no Power BI, exportar a sua consulta do Log Analytics para [linguagem do Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx).  Em seguida, utilize esta opção para criar uma consulta no Power BI Desktop e, em seguida, publique-o no Power BI como um conjunto de dados.  Os detalhes deste processo são descritos abaixo.

![O log Analytics para o Power BI](media/powerbi/overview.png)

## <a name="export-query"></a>Consulta de exportação
Comece por criar um [pesquisa de registos](../../azure-monitor/log-query/log-query-overview.md) que devolve os dados do Log Analytics que deseja preencher o conjunto de dados do Power BI.  Em seguida, exportar essa consulta para [linguagem do Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx) que podem ser utilizadas pelo Power BI Desktop.

1. Crie a pesquisa de registos no Log Analytics para extrair os dados para o conjunto de dados.
2. Se estiver a utilizar o portal de pesquisa de registos, clique em **Power BI**.  Se estiver a utilizar o portal do Analytics, selecione **exportar** > **Power BI consulta (M)**.  As duas opções exportar a consulta para um ficheiro de texto chamado **PowerBIQuery.txt**. 

    ![Pesquisa de registos de exportação](media/powerbi/export-logsearch.png) ![Pesquisa de registos de exportação](media/powerbi/export-analytics.png)

3. Abra o ficheiro de texto e copie os conteúdos.

## <a name="import-query-into-power-bi-desktop"></a>Importar consulta para o Power BI Desktop
O ambiente de trabalho do Power BI é um aplicativo de desktop permite-lhe criar conjuntos de dados e relatórios que podem ser publicados para o Power BI.  Também pode utilizar para criar uma consulta usando a linguagem do Power Query exportada a partir do Log Analytics. 

1. Instale [Power BI Desktop](https://powerbi.microsoft.com/desktop/) se não o tiver e, em seguida, abra a aplicação.
2. Selecione **obter dados** > **consulta em branco** para abrir uma nova consulta.  Em seguida, selecione **Editor avançado** e cole o conteúdo do ficheiro exportado para a consulta. Clique em **Concluído**.

    ![Consulta do Power BI Desktop](media/powerbi/desktop-new-query.png)

5. A consulta é executada, e seus resultados são apresentados.  Poderá ser-lhe pedido para as credenciais para ligar ao Azure.  
6. Escreva um nome descritivo para a consulta.  A predefinição é **Consulta1**. Clique em **fechar e aplicar** para adicionar o conjunto de dados ao relatório.

    ![Nome do Power BI Desktop](media/powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Publicar no Power BI
Ao publicar no Power BI, um conjunto de dados e um relatório serão criados.  Se criar um relatório no Power BI Desktop, em seguida, isso será publicado com os seus dados.  Caso contrário, em seguida, será criado um relatório em branco.  Pode modificar o relatório no Power BI ou crie um novo com base no conjunto de dados.

8. Crie um relatório com base nos seus dados.  Uso [documentação do Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-report-view) se não estiver familiarizado com ele.  Quando estiver pronto para enviar para o Power BI, clique em **publicar**.  Quando lhe for pedido, selecione um destino na sua conta do Power BI.  A menos que tenha um destino específico em mente, utilize **minha área de trabalho**.

    ![Publicar do Power BI Desktop](media/powerbi/desktop-publish.png)

3. Quando a publicação estiver concluída, clique em **abrir no Power BI** para abrir o Power BI com o novo conjunto de dados.


### <a name="configure-scheduled-refresh"></a>Configurar a atualização agendada
O conjunto de dados criado no Power BI terão os mesmos dados que viu anteriormente no Power BI Desktop.  Terá de atualizar o conjunto de dados periodicamente para executar novamente a consulta e preenchê-lo com os dados mais recentes do Log Analytics.  

1. Clique na área de trabalho em que carregou o relatório e selecione o **conjuntos de dados** menu. Selecione o menu de contexto junto ao seu novo conjunto de dados e selecione **definições**. Sob **credenciais da origem de dados** deve ter uma mensagem que as credenciais são inválidas.  Isso é porque ainda não forneceu credenciais ainda para o conjunto de dados a utilizar quando ele atualiza os seus dados.  Clique em **editar credenciais** e especifique as credenciais com acesso ao Log Analytics.

    ![Agenda do Power BI](media/powerbi/powerbi-schedule.png)

5. Sob **atualização agendada** ative a opção para **manter os seus dados atualizados**.  Opcionalmente, pode alterar o **frequência de atualização** e uma ou mais vezes específicos para executar a atualização.

    ![Atualização do Power BI](media/powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [pesquisas de registos](../../azure-monitor/log-query/log-query-overview.md) para criar consultas que podem ser exportadas para o Power BI.
* Saiba mais sobre [Power BI](https://powerbi.microsoft.com) para criar visualizações com base em exportações do Log Analytics.