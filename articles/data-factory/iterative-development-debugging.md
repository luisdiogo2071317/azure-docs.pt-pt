---
title: Desenvolvimento iterativo e depuração no Azure Data Factory | Microsoft Docs
description: Saiba como desenvolver e depurar pipelines de fábrica de dados iteratively no portal do Azure.
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.date: 04/04/2018
ms.topic: article
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.openlocfilehash: adb5ffd65d4b2b591abba5d4b3481b7ca08de579
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Desenvolvimento iterativo e depuração com o Azure Data Factory

O Azure Data Factory permite-lhe iteratively desenvolver e depurar pipelines de fábrica de dados.

## <a name="iterative-debugging-features"></a>Funcionalidades de depuração interativo
Criar pipelines e teste for executado utilizando o **depurar** capacidade na tela pipeline sem ter de escrever uma única linha de código.

![Depurar capacidade na tela pipeline](media/iterative-development-debugging/iterative-development-image1.png)

Ver os resultados de teste é executado no **saída** janela da tela pipeline.

![Janela de saída da tela pipeline](media/iterative-development-debugging/iterative-development-image2.png)

Depois de um teste executar for bem sucedida, adicionar mais atividades para o pipeline e continuar a depuração de forma iterativa. Também pode **Cancelar** um teste executar enquanto esta se encontra em curso.

![Cancelar um execução do teste](media/iterative-development-debugging/iterative-development-image3.png)

Quando testa é executado, não tem de publicar as alterações à fábrica de dados antes de selecionar **depurar**. Esta ação é útil em cenários em que pretenda Certifique-se de que as alterações a funcionar conforme esperado antes de atualizar o fluxo de trabalho de fábrica de dados.

## <a name="more-info-about-debugging"></a>Obter mais informações sobre depuração

1. O teste é executado iniciado com o **depurar** capacidade não estão disponíveis na lista de **Monitor** separador. Pode apenas Consulte executa accionada com **acionador agora**, **agenda**, ou **janela em cascata** aciona no **Monitor** separador. Pode ver o último teste executar iniciada com o **depurar** capacidade no **saída** janela da tela pipeline.

2. Selecionar **depurar** executa efetivamente o pipeline. Por isso, por exemplo, se o pipeline contém uma atividade de cópia, a execução do teste copia dados a partir da origem para destino. Como resultado, recomendamos a utilização de pastas de teste nas atividades de cópia e outras atividades quando a depuração. Depois de ter debugged pipeline, mude para as pastas reais que pretende utilizar nas operações normais.

## <a name="setting-breakpoints-for-debugging"></a>A definição de pontos de interrupção para depuração

Fábrica de dados também lhe permite até chegar uma atividade específica no seu tela do pipeline de depuração. Apenas colocar um ponto de interrupção na atividade até que pretende testar e selecione **depurar**. Fábrica de dados garante que o teste é executado apenas até que a atividade de ponto de interrupção na tela do pipeline. Isto *depurar até* funcionalidade é útil quando não pretende testar o pipeline de todo, mas apenas um subconjunto das atividades dentro do pipeline.

![Pontos de interrupção na tela pipeline](media/iterative-development-debugging/iterative-development-image4.png)

## <a name="next-steps"></a>Passos Seguintes
[Integração contínua e a implementação no Azure Data Factory](continuous-integration-deployment.md)
