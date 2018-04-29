---
title: Compreender as entradas para o Azure Stream Analytics
description: Este artigo descreve o conceito de entradas de uma tarefa do Azure Stream Analytics, comparando a entrada de transmissão em fluxo para entrada de dados de referência.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 926821e2ba9912ae0140f11c9fe9a2d504609a1e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Compreender as entradas para o Azure Stream Analytics

Tarefas do Stream Analytics do Azure ligar a uma ou mais entradas de dados. Cada entrada define uma ligação a uma origem de dados existente. Do Stream Analytics aceita entrada de dados de vários tipos de origens de eventos, incluindo o armazenamento de BLOBs, o IoT Hub e Hubs de eventos. As entradas são referenciadas por nome na consulta SQL transmissão em fluxo que escreve para cada tarefa. A consulta, pode associar várias entradas para se misturem dados ou comparam os dados de transmissão em fluxo com uma pesquisa de dados de referência e passar os resultados para saídas. 

Do Stream Analytics tem integração de primeira classe de três tipos de recursos como entradas:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub/) 
- [Armazenamento de Blobs do Azure](https://azure.microsoft.com/services/storage/blobs/) 

Estes recursos entrados podem em direto na mesma subscrição do Azure como a tarefa de Stream Analytics, ou a partir de uma subscrição diferente.

Pode utilizar o [portal do Azure](stream-analytics-quick-create-portal.md#configure-input-to-the-job), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/New-AzureRmStreamAnalyticsInput), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), e [Visual Studio](stream-analytics-tools-for-visual-studio.md)para criar, editar e testar a entradas da tarefa de Stream Analytics.

## <a name="stream-and-reference-inputs"></a>Entradas de fluxo e de referência
Como os dados são forçados a uma origem de dados, tem consumidas pela tarefa de Stream Analytics e processados em tempo real. Entradas são divididas em dois tipos: entradas de dados de referência e entradas de fluxo de dados.

### <a name="data-stream-input"></a>Entrada de fluxo de dados
Um fluxo de dados é uma sequência de eventos unbounded ao longo do tempo. Tarefas do Stream Analytics tem de incluir entradas de fluxo de dados de pelo menos um. Os Event Hubs, o IoT Hub e o armazenamento de BLOBs são suportados como origens de entrada de fluxo de dados. Os Event Hubs são utilizados para recolher fluxos de eventos de vários dispositivos e serviços. Estes fluxos podem incluir feeds de atividade de redes sociais, as cotações compromissos informações ou dados de sensores. Os Hubs IoT estão otimizados para recolher dados de dispositivos ligados em cenários Internet das coisas (IoT).  O blob storage pode ser utilizado como uma origem de entrada para a ingestão de dados do volume como um fluxo, tais como ficheiros de registo.  

Para obter mais informações sobre as entradas de dados de transmissão em fluxo, consulte [transmitir dados como entrada para o Stream Analytics](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Entrada de dados de referência
Stream Analytics suporta também conhecida como de entrada *referência a dados*. Dados de referência estão completamente estático ou alterações lentamente. Normalmente, é utilizado para efetuar a correlação e pesquisas. Por exemplo, poderá associar dados na entrada de fluxo de dados para dados em dados de referência, sucederia com executaria uma associação SQL para procurar valores estáticos. Armazenamento de Blobs do Azure está atualmente a única origem de entrada suportada para dados de referência. Os blobs de origem de dados de referência estão limitados a 100 MB de tamanho.

Para mais informações sobre as entradas de dados de referência, consulte [utilizar dados de referência para pesquisas com no Stream Analytics](stream-analytics-use-reference-data.md)

Este artigo é um passo de [percurso de aprendizagem do Stream Analytics](/documentation/learning-paths/stream-analytics/).

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Início rápido: Criar uma tarefa de Stream Analytics, utilizando o portal do Azure](stream-analytics-quick-create-portal.md)