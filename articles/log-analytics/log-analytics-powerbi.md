---
title: Importar dados de análise de registos do Azure para o Power BI | Microsoft Docs
description: Power BI é um serviço de análise de negócio baseada na nuvem da Microsoft que fornece visualizações otimizadas e relatórios para análise dos diferentes conjuntos de dados.  Este artigo descreve como configurar e importar dados de análise de registos para o Power BI e configurá-lo para atualizar automaticamente.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: bwren
ms.openlocfilehash: 725828c2acc5ac4bb53c5e6af14d20578a3d3652
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Importar dados de análise de registos do Azure para o Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) é um serviço de análise de negócio baseada na nuvem da Microsoft que fornece visualizações otimizadas e relatórios para análise dos diferentes conjuntos de dados.  Pode importar os resultados da pesquisa de análise de registos de registo para um conjunto de dados do Power BI, pelo que pode tirar partido das respetivas funcionalidades como combinar dados de diferentes origens e partilha relatórios na web e dispositivos móveis.

## <a name="overview"></a>Descrição geral
Para importar dados a partir de uma área de trabalho de análise de registos para o Power BI, crie um conjunto de dados no Power BI com base numa consulta de pesquisa de registo na análise de registos.  A consulta é executada sempre que o conjunto de dados está atualizado.  Em seguida, pode criar relatórios do Power BI que utilizam dados do conjunto de dados.  Para criar o conjunto de dados no Power BI, exportar a consulta de análise de registos para [idioma de consulta de energia (M)](https://msdn.microsoft.com/library/mt807488.aspx).  Em seguida, utilize esta opção para criar uma consulta no Power BI Desktop e, em seguida, publicá-lo no Power BI como um conjunto de dados.  Os detalhes para este processo são descritos abaixo.

![Análise de registos para o Power BI](media/log-analytics-powerbi/overview.png)

## <a name="export-query"></a>Consulta de exportação
Comece por criar um [pesquisa registo](log-analytics-log-search-new.md) que devolve os dados de análise de registos que pretende povoar o conjunto de dados do Power BI.  Em seguida, exportar essa consulta para [idioma de consulta de energia (M)](https://msdn.microsoft.com/library/mt807488.aspx) que pode ser utilizado pelo Power BI Desktop.

1. Crie a pesquisa de registo na análise de registos para extrair os dados para o conjunto de dados.
2. Se estiver a utilizar o portal de pesquisa de registo, clique em **Power BI**.  Se estiver a utilizar o portal da análise, selecione **exportar** > **Power BI consulta (M)**.  A consulta ambas estas opções de exportação para um ficheiro de texto chamado **PowerBIQuery.txt**. 

    ![Pesquisa de registo de exportação](media/log-analytics-powerbi/export-logsearch.png) ![Pesquisa de registo de exportação](media/log-analytics-powerbi/export-analytics.png)

3. Abra o ficheiro de texto e copie o respetivo conteúdo.

## <a name="import-query-into-power-bi-desktop"></a>Importar consulta para o Power BI Desktop
Ambiente de trabalho do Power BI é uma aplicação de ambiente de trabalho que permite-lhe criar conjuntos de dados e relatórios que podem ser publicados para o Power BI.  Também pode utilizar para criar uma consulta utilizando a linguagem de Power Query exportada a partir da análise de registos. 

1. Instalar [Power BI Desktop](https://powerbi.microsoft.com/desktop/) se não tiver já-lo e, em seguida, abra a aplicação.
2. Selecione **obter dados** > **consulta em branco** para abrir uma nova consulta.  Em seguida, selecione **avançadas Editor** e cole o conteúdo do ficheiro exportado para a consulta. Clique em **Concluído**.

    ![Consulta do Power BI Desktop](media/log-analytics-powerbi/desktop-new-query.png)

5. A consulta é executada, e os respetivos resultados são apresentados.  Poderão ser-lhe solicitadas as credenciais ligar ao Azure.  
6. Escreva um nome descritivo para a consulta.  A predefinição é **Query1**. Clique em **fechar e aplicar** para adicionar o conjunto de dados ao relatório.

    ![Nome do Power BI Desktop](media/log-analytics-powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Publicar para o Power BI
Quando publica para o Power BI, serão criados um conjunto de dados e um relatório.  Se criar um relatório no Power BI Desktop, em seguida, este será publicado com os seus dados.  Caso contrário, em seguida, irá criar um relatório em branco.  Pode modificar o relatório no Power BI ou crie uma nova com base no conjunto de dados.

8. Crie um relatório com base nos seus dados.  Utilize [documentação do Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-report-view) se não estiver familiarizado com o mesmo.  Quando estiver pronto para enviar ao Power BI, clique em **publicar**.  Quando lhe for pedido, selecione um destino na sua conta do Power BI.  A menos que tenha um destino específico em mente, utilize **minha área de trabalho**.

    ![Publicar o Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Quando a publicação estiver concluída, clique em **abrir no Power BI** para abrir o Power BI com o novo conjunto de dados.


### <a name="configure-scheduled-refresh"></a>Configurar a atualização agendada
O conjunto de dados criado no Power BI terão os mesmos dados que vimos anteriormente no Power BI Desktop.  Terá de atualizar o conjunto de dados periodicamente para executar a consulta novamente e preenchê-lo com os dados mais recentes da análise de registos.  

1. Clique na área de trabalho onde que carregou o relatório e selecione o **conjuntos de dados** menu. Selecione o menu de contexto junto ao seu novo conjunto de dados e selecione **definições**. Em **as credenciais da origem de dados** deve ter uma mensagem que as credenciais são inválidas.  Isto acontece porque ainda não forneceu as credenciais ainda para o conjunto de dados a utilizar quando se atualiza os dados.  Clique em **editar credenciais** e especificar credenciais com acesso a análise de registos.

    ![Agendamento do Power BI](media/log-analytics-powerbi/powerbi-schedule.png)

5. Em **atualização agendada** ative a opção para **manter os seus dados atualizados**.  Opcionalmente, pode alterar o **atualizar frequência** e um ou mais horas específicas ao executar a atualização.

    ![Atualização do Power BI](media/log-analytics-powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [pesquisas de registo](log-analytics-log-searches.md) para criar consultas que podem ser exportadas para o Power BI.
* Saiba mais sobre [Power BI](http://powerbi.microsoft.com) criar visualizações baseadas nos exportações de análise de registos.
