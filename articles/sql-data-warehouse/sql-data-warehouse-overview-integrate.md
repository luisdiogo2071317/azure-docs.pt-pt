---
title: Criar soluções integradas com o SQL Data Warehouse | Microsoft Docs
description: 'Ferramentas e parceiros com soluções que se integram com o SQL Data Warehouse. '
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: f198a99fc03a079be77c7f8167580bb7b758579e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>Integrar a outros serviços com o SQL Data Warehouse
Para além do respetiva funcionalidades principais, o SQL Data Warehouse permite aos utilizadores integrar com muitos dos outros serviços no Azure. Alguns destes serviços incluem:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

O SQL Data Warehouse continua a integrar com mais serviços através do Azure e muito mais [parceiros de integração](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
Integração do Power BI permite-lhe combinar a capacidade de computação do SQL Data Warehouse com os relatórios dinâmicos e visualização do Power BI. Integração do Power BI atualmente inclui:

* **Direcionar Connect**: um mais avançadas de ligação com a lógica pushdown no armazém de dados do SQL Server. Pushdown fornece uma análise mais rápido numa escala maior.
* **Abrir no Power BI**: O botão 'Abrir no Power BI' passa a informação de instância para Power BI para uma forma simplifed ligar.

Para obter mais informações, consulte [integrar com o Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md), ou o [documentação do Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx).

## <a name="azure-data-factory"></a>Azure Data Factory
O Azure Data Factory fornece aos utilizadores uma plataforma gerida para criar extrair complexa e carregar pipelines. Integração do SQL Data Warehouse com o Azure Data Factory inclui:

* **Procedimentos armazenados**: orquestrar a execução de procedimentos armazenados no armazém de dados do SQL Server.
* **Cópia**: Utilize ADF para mover dados para o SQL Data Warehouse. Esta operação pode utilizar o mecanismo de movimento de dados padrão do ADF ou PolyBase nos bastidores. 

Para obter mais informações, consulte [integrar com o Azure Data Factory](sql-data-warehouse-get-started-visualize-with-power-bi.md).

## <a name="azure-machine-learning"></a>Azure Machine Learning
O Azure Machine Learning é um serviço de análise completamente gerido, que lhe permite criar modelos intricate utilizando um grande conjunto de ferramentas preditivos. Armazém de dados do SQL Server é suportado como uma origem e de destino para estes modelos com a seguinte funcionalidade:

* **Ler dados:** unidade modelos com dimensionamento, com o T-SQL no armazém de dados do SQL Server.
* **Escrita de dados:** consolidação é alterado de qualquer modelo de volta para o SQL Data Warehouse.

Para obter mais informações, consulte [integrar com o Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
O Azure Stream Analytics é uma infraestrutura complexa, completamente gerida para processamento e consumir dados de eventos gerados a partir de Hub de eventos do Azure.  Integração com o SQL Data Warehouse permite a transmissão em fluxo de dados de forma eficaz processar e armazenar em conjunto com a dados relacionais, ativar a análise mais aprofundada, mais avançada.  

* **Resultado da tarefa:** enviar o resultado de tarefas do Stream Analytics diretamente ao SQL Data Warehouse.

Para obter mais informações, consulte [integrar com o Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).

## <a name="next-steps"></a>Passos Seguintes
Para integrar com a SQL Database do Azure, consulte [consulta elástica de configurar a base de dados do SQL](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)

