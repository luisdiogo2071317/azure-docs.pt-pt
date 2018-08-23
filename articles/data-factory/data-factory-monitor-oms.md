---
title: Monitorizar a fábrica de dados do Azure com o OMS | Documentos da Microsoft
description: Saiba como monitorizar o Azure Data Factory, dados de encaminhamento para o Operations Management Suite (OMS) para análise.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: douglasl
ms.openlocfilehash: fb5da75d0e42dd0700cad008d348ff846d602409
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057093"
---
# <a name="monitor-azure-data-factory-with-operations-management-suite-oms"></a>Monitor do Azure Data Factory com o Operations Management Suite (OMS)

Pode utilizar a integração do Azure Data Factory com o Azure Monitor para encaminhar os dados para o Operations Management Suite (OMS). Esta integração é útil nos seguintes cenários:

1.  Quer escrever consultas complexas num conjunto avançado de métricas, que são publicadas pela fábrica de dados ao OMS. Também pode criar alertas personalizados nestas consultas através do OMS.

2.  Pretende monitorizar em fábricas de dados. Pode encaminhar os dados de diferentes fábricas de dados para um único espaço de trabalho do OMS.

Para obter uma introdução de sete minutos e demonstração desta funcionalidade, veja o vídeo seguinte:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

## <a name="get-started"></a>Introdução

### <a name="configure-diagnostic-settings-and-oms-workspace"></a>Configurar definições de diagnóstico e de área de trabalho do OMS

Ative as definições de diagnóstico para a fábrica de dados.

1.  Selecione **do Azure Monitor** -> **as definições de diagnóstico** -> selecionar a fábrica de dados -> ativar diagnósticos.

    ![Monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Fornece definições de diagnóstico, incluindo a configuração da área de trabalho do OMS.

    ![Monitor-oms-image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-oms-pack-from-azure-marketplace"></a>Instalar o pacote de OMS de análise do Azure Data Factory a partir do Azure Marketplace

![Monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![Monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Clique em **criar** e selecione a área de trabalho do OMS e definições de área de trabalho do OMS.

![Monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

## <a name="monitor-azure-data-factory-metrics-using-oms"></a>Monitorizar métricas de fábrica de dados do Azure com o OMS

Instalar o **do Azure Data Factory Analytics** OMS pacote cria um conjunto de padrão de modos de exibição que permite que as métricas seguintes:

- Execuções de Pipeline do ADF execuções-1) pelo Data Factory

- Execuções de atividades do ADF execuções-2) pelo Data Factory

- Execuções do acionador de execuções do ADF a 3) pelo Data Factory

- Erros de Pipeline do ADF erros-1) os 10 principais pela fábrica de dados

- Execuções de atividades do ADF erros-2) os 10 principais pela fábrica de dados

- Erros de Acionador do ADF erros a 3) os 10 principais pela fábrica de dados

- Execuções de atividades do ADF estatísticas-1) por tipo

- Execuções de Acionador do ADF estatísticas-2) por tipo

- Duração de execuções do Pipeline de máx. de estatísticas do ADF a 3)

![Monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![Monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Pode visualizar as métricas acima, examinar as consultas por trás estas métricas, editar as consultas, criar alertas e assim por diante.

![Monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="next-steps"></a>Passos Seguintes

Ver [monitorizar e gerir pipelines de forma programática](https://docs.microsoft.com/en-us/azure/data-factory/monitor-programmatically) para saber mais sobre monitorização e gestão de pipelines através da execução de scripts.
