---
title: Visualizar e resolver problemas relacionados com tarefas do Azure Stream Analytics
description: Este artigo descreve como visualizar uma tarefa de Stream Analytics, utilizando a funcionalidade de diagrama de diagnóstico para efetuar a resolução de problemas de self-service.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 523802f1f9a1dda19c5b6a66da7bc26fee851bd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualizar e resolver problemas relacionados com tarefas do Stream Analytics
No Stream Analytics, tal como acontece com outras tecnologias baseado na nuvem, resolução de problemas é por vezes, necessário a razão pela qual uma tarefa não produzir o resultado esperado (ou qualquer saída para esse fim). Com este conceito em mente, o Stream Analytics fornece a capacidade para visualizar uma tarefa de transmissão em fluxo. Isto também é útil como uma ferramenta de modelação e tem o benefício do lado desses necessitando de documentação do seu trabalho.

No painel de visualização, as entradas estão visíveis, bem como a consulta a ser executado e, em seguida, todas as saídas configuradas. Problemas de conetividade ou a configuração podem tornar-se mais aparente e também pode ser útil ver uma representação visual da configuração.

## <a name="using-the-diagnosis-diagram-tool"></a>Utilizar a ferramenta de diagrama de diagnóstico
Para aceder a este visualizador, basta clicar no botão "Diagrama de diagnóstico" na área da tarefa de Stream Analytics "Definições".

![stream-analytics-troubleshoot-visualization-diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Cada entrada e saída são o estado de cor codificada para indicar atual desse componente, conforme mostrado da seguinte forma.

![stream-analytics-troubleshoot-visualization-input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Quando o utilizador pretende ver passos da consulta intermédio para compreender os padrões de fluxo de dados dentro de uma tarefa, a ferramenta de visualização fornece uma vista de divisão da consulta para os passos de componente e a sequência de fluxo. Clicar em cada passo de consulta mostra a secção correspondente numa consulta painel conforme ilustrado a editar. 

![stream-analytics-troubleshoot-visualization-intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

