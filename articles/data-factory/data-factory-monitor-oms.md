---
title: Monitorizar Azure Data Factory com o OMS | Microsoft Docs
description: Saiba como monitorizar dados de encaminhamento para o Operations Management Suite (OMS) para análise do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: douglasl
ms.openlocfilehash: 4275a4ddcee51d58949b5bd83e4a898cb3dbb389
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304424"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Monitor do Azure Data Factory no Operations Management Suite (OMS)

Pode utilizar a integração do Azure Data Factory com a monitorização do Azure para dados da rota para o Operations Management Suite (OMS). Esta integração é útil nos seguintes cenários:

1.  Pretender escrever consultas complexas num conjunto avançado de métricas que são publicadas pela fábrica de dados ao OMS. Também pode criar alertas personalizados nestas consultas através do OMS.

2.  Pretende monitorizar nas fábricas de dados. Pode encaminhar os dados de várias fábricas de dados para uma única área de trabalho do OMS.

## <a name="get-started"></a>Introdução

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Configurar definições de diagnóstico e área de trabalho do OMS

Ative as definições de diagnóstico para a fábrica de dados.

1.  Selecione **Azure Monitor** -> **as definições de diagnóstico** -> selecione a fábrica de dados -> ativar diagnósticos.

    ![Monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Fornece definições de diagnóstico, incluindo a configuração da área de trabalho OMS.

    ![Monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Instalar o pacote de análise de fábrica de dados do Azure OMS do Azure Marketplace

![Monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![Monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Clique em **criar** e selecione a área de trabalho OMS e definições da área de trabalho do OMS.

![Monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Monitorizar as métricas fábrica de dados do Azure com o OMS

Instalar o **análise do Azure Data Factory** OMS pacote cria um conjunto predefinido de vistas que lhe permite as métricas seguintes:

- Execução de Pipeline do ADF executa-1) pela fábrica de dados

- Execuções de atividade ADF executa-2) pela fábrica de dados

- Execuções de Acionador ADF executa-3) pela fábrica de dados

- Erros de Pipeline de 10 principais ADF erros-1) por fábrica de dados

- Execuções de atividade ADF erros-2) 10 principais pela fábrica de dados

- Erros de Acionador de 10 principais ADF erros-3) pela fábrica de dados

- Execuções de atividade ADF estatísticas-1) por tipo

- Execuções de Acionador ADF estatísticas-2) por tipo

- Pipeline de máx. do ADF estatísticas-3) é executado duração

![Monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![Monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Pode visualizar as métricas acima, observe as consultas atrás estas métricas, edite as consultas, criar alertas e assim sucessivamente.

![Monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Passos Seguintes

Consulte [monitorizar e gerir pipelines programaticamente](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) para saber mais sobre como monitorizar e gerir pipelines através da execução de scripts.
