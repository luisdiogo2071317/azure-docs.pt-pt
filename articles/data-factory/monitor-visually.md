---
title: Monitorizar visualmente fábricas de dados do Azure | Documentos da Microsoft
description: Saiba como monitorizar visualmente fábricas de dados do Azure
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: shlo
ms.openlocfilehash: 4b3828e1857d17a128de346449d5cf2041709e50
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041080"
---
# <a name="visually-monitor-azure-data-factories"></a>Monitorizar visualmente fábricas de dados do Azure
O Azure Data Factory é um serviço de integração de dados baseado na cloud que lhe permite criar fluxos de trabalho condicionados por dados na cloud para orquestrar e automatizar o movimento e a transformação de dados. Com o Azure Data Factory, poderá criar e agendar fluxos de trabalho condicionados por dados (denominados pipelines) que são capazes de ingerir dados provenientes de diferentes arquivos de dados, processar/transformar os dados através de serviços de computação, tais como o Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics e Azure Machine Learning, e publicar os dados de saída em arquivos de dados como o Azure SQL Data Warehouse para serem consumidos por aplicações de business intelligence (BI).
Neste início rápido, irá aprender a monitorizar visualmente pipelines do data factory v2 sem ter de escrever uma única linha de código.
Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="monitor-data-factory-v2-pipelines"></a>Monitorizar pipelines do data factory v2

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. Inicie sessão para o [portal do Azure](https://portal.azure.com/).
3. Navegue para o painel de fábrica de dados criada no portal do Azure e clique no mosaico 'Monitorizar e gerir'. Este procedimento inicia a experiência de monitorização visual do ADF v2.

## <a name="list-view-monitoring"></a>Monitorização de vista de lista

Monitorizar o pipeline e execuções de atividades com uma interface do modo de exibição de lista simples. Todas as execuções são apresentadas no fuso horário local do browser. Pode alterar o fuso horário e todos os campos de data e hora mudarão para esse fuso horário.  

#### <a name="monitoring-pipeline-runs"></a>Monitorizar execuções de Pipeline
Vista de lista que mostra cada execução de pipeline dos seus pipelines da fábrica de dados v2. Colunas incluídas:

| **Nome da coluna** | **Descrição** |
| --- | --- |
| Nome do pipeline | Nome do pipeline. |
| Ações | Única ação disponível para ver as execuções de atividade. |
| Início da execução | Início da execução de pipeline data hora (MM/DD/AAAA HH: mm: SS AM/PM) |
| Duração | Duração de execução (hh: mm:) |
| Acionada Por | Acionador manual, o acionador de agenda |
| Estado | Não foi possível, foi concluída com êxito, está em curso |
| Parâmetros | Parâmetros (nome, pares de valor) de execução de pipeline |
| Erro | Execução de pipeline erro (if/any) |
| ID de Execução | ID de execução do pipeline |

![Monitorizar execuções de pipeline](media/monitor-visually/pipeline-runs.png)

#### <a name="monitoring-activity-runs"></a>Execuções de atividades de monitorização
Vista de lista que mostra as execuções de atividades que correspondem a cada execução de pipeline. Clique em **atividade executa** ícone sob a **'Actions'** coluna para ver a atividade é executada para cada execução de pipeline. Colunas incluídas:

| **Nome da coluna** | **Descrição** |
| --- | --- |
| Nome da Actividade | Nome da atividade no pipeline. |
| Tipo de Actividade | Tipo de atividade, ou seja, o cópia, HDInsightSpark, HDInsightHive etc. |
| Início da execução | Início de execução da atividade tempo data (MM/DD/AAAA HH: mm: SS AM/PM) |
| Duração | Duração de execução (hh: mm:) |
| Estado | Não foi possível, foi concluída com êxito, está em curso |
| Input | Matriz JSON que descrevem as entradas de atividade |
| Saída | Matriz JSON que descrevem as saídas de atividade |
| Erro | Erro de execução (if/any) de atividade |

![Monitorização de execuções de atividade](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> Tem de clicar **'Atualizar'** ícone na parte superior para atualizar a lista de pipeline e execuções de atividades. A atualização automática não é suportada atualmente.
>

![Atualizar](media/monitor-visually/refresh.png)

## <a name="features"></a>Funcionalidades

#### <a name="select-a-data-factory-to-monitor"></a>Selecione uma fábrica de dados para monitorizar
Paire o rato sobre o **fábrica de dados** ícone no canto superior esquerdo. Clique no ícone de seta "para" para ver uma lista de fábricas de dados e de subscrições do azure que pode monitorizar.

![Selecionar fábrica de dados](media/monitor-visually/select-datafactory.png)

#### <a name="rich-ordering-and-filtering"></a>Ordenação e filtragem avançadas

Execuções de pipelines de encomenda ordem descendente/ascendente por início da execução e filtre execuções por colunas seguintes:

| **Nome da coluna** | **Descrição** |
| --- | --- |
| Nome do pipeline | Nome do pipeline. As opções incluem filtros rápidos para 'últimas 24 horas', 'Semana passada', "últimos 30 dias" ou selecione uma hora de datas personalizadas. |
| Início da execução | Início de execução de pipeline data hora |
| Estado de execução | Filtro é executado por Estado, ou seja, com êxito, falha, em curso |

![Filtro](media/monitor-visually/filter.png)

#### <a name="addremove-columns-in-list-view"></a>Adicionar/remover colunas na vista de lista
Clique com o botão direito do rato no cabeçalho de vista de lista e escolha as colunas que pretende são apresentadas na vista de lista

![Colunas](media/monitor-visually/columns.png)

#### <a name="reorder-column-widths-in-list-view"></a>Reordenar a largura das colunas na vista de lista
Aumentar e diminuir a largura das colunas na vista de lista por simplesmente passar o rato sobre o cabeçalho de coluna

#### <a name="user-properties"></a>Propriedades do utilizador

Pode promover qualquer propriedade de atividade do pipeline como uma propriedade de utilizador para que o torna-se uma entidade de monitorização. Por exemplo, pode promover a **origem** e **destino** propriedades da atividade de cópia no seu pipeline como propriedades de utilizador. Também pode selecionar **de geração automática** para gerar o **origem** e **destino** propriedades de utilizador para uma atividade de cópia.

![Criar propriedades de utilizador](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Apenas pode promover propriedades de atividade do pipeline até 5 como propriedades de utilizador.

Depois de criar as propriedades de utilizador, em seguida, pode monitorizá-las nas vistas de lista de monitorização. Se a origem para a atividade de cópia é um nome de tabela, pode monitorizar o nome da tabela de origem como uma coluna na atividade é executada a vista de lista.

![Lista sem propriedades de utilizador de execuções de atividades](media/monitor-visually/monitor-user-properties-image2.png)

![Adicionar colunas para as propriedades do utilizador a lista de execuções de atividade](media/monitor-visually/monitor-user-properties-image3.png)

![Lista de execuções de atividades com colunas para as propriedades do utilizador](media/monitor-visually/monitor-user-properties-image4.png)

#### <a name="guided-tours"></a>Tours guiados
Clique no "ícone de informações" no canto inferior esquerdo e clique em "Tours guiados" para obter instruções passo a passo sobre como monitorizar as pipeline e execuções de atividades.

![Tours guiados](media/monitor-visually/guided-tours.png)

#### <a name="feedback"></a>Comentários
Clique no ícone de "Comentários", para nos dar comentários sobre vários recursos ou de quaisquer problemas que talvez esteja enfrentando.

![Comentários](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Passos Seguintes

Ver [monitorizar e gerir pipelines de forma programática](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) artigo para saber como monitorizar e gerir pipelines
