---
title: Melhores práticas ao escolher um ID de série de tempo no Azure Time Series Insights | Documentos da Microsoft
description: Noções básicas sobre melhores práticas ao escolher um ID de série de tempo no Azure Time Series Insights
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: a7fd1ff4a0fe96724af0c43019afe53666ab81d7
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856358"
---
# <a name="best-practices-when-choosing-a-time-series-id"></a>Melhores práticas ao escolher um ID de série de tempo

Este documento aborda a chave de partição do Azure Time Series Insights (pré-visualização), o **ID de série de tempo**e as práticas recomendadas para escolher um.

## <a name="choose-a-time-series-id"></a>Escolha um ID de série de tempo

A opção do **ID de série de tempo** é como a seleção de uma chave de partição para uma base de dados. Portanto, é uma decisão importante que deve ser feita em tempo de design. Não é possível atualizar um ambiente existente do TSI (pré-visualização) para utilizar um diferente **ID de série de tempo**. Em outras palavras, depois de criar um ambiente com um **ID de série de tempo**, a política não pode ser alterada, pois é uma propriedade imutável.

> [!IMPORTANT]
> O **ID de série de tempo** é imutável e diferencia maiúsculas de minúsculas (ele não é possível alterar uma vez definida).

Com isso em mente, selecionando o adequado **ID de série de tempo** é fundamental. Considere as seguintes propriedades ao selecionar uma **ID de série de tempo**:

> [!div class="checklist"]
> * Escolha um nome de propriedade que tem uma vasta gama de valores e tem até mesmo os padrões de acesso. É recomendável ter uma chave de partição com muitos valores distintos (por exemplo, centenas ou milhares). Para muitos clientes, este será algo parecido com **ID do dispositivo**, **ID de Sensor**, etc.  
> * Deve ser exclusivo no nível de nó folha da sua [modelo de série de tempo](./time-series-insights-update-tsm.md).
> * R **ID de série de tempo** cadeia de caracteres de nome de propriedade pode ter até 128 carateres e **ID de série de tempo** valores de propriedade podem ser até 1024 carateres.
> * Se algumas exclusivo **ID de série de tempo** valores de propriedade estão em falta, são tratadas como valores nulos, o que faça parte da restrição de exclusividade.

Além disso, pode selecionar até **três** propriedades chave (3), como seu **ID de série de tempo**.

  > [!NOTE]
  > Sua **três** propriedades chave (3) tem de ser cadeias de caracteres.

Os seguintes cenários descrevem selecionar mais do que uma propriedade de chave como sua **ID de série de tempo**:  

* Cenário 1:

  * Terá de frotas de legado de ativos que cada um tem chaves exclusivas. 
  * Por exemplo, uma frota seja identificada exclusivamente pela propriedade **deviceId** e outra em que é a propriedade exclusiva **objectId**.  Em ambos os frotas de, a propriedade de outros frota única não está presente. Neste exemplo, deverá selecionar duas chaves, **deviceId**, e **objectId** como chaves exclusivas. 
  * Aceitamos valores nulos, e a falta de presença de uma propriedade no payload do evento que irão contar como um `null` valor.  Isso também seria a forma adequada para lidar com o envio de dados para duas origens de eventos diferentes em que os dados em cada origem de evento tinham um diferentes exclusivo **ID de série de tempo**.

* Cenário 2:

  * Precisa de várias propriedades para mostrar a exclusividade na mesma frota de ativos. Por exemplo, digamos que um fabricante de construção inteligente e implementação de sensores em cada sala. Em cada sala, normalmente têm os mesmos valores para **sensorId**, incluindo **sensor1**, **sensor2**, **sensor3**e assim por diante.
  * Além disso, tem a sobreposição de números de andar e espaço em todos os sites na propriedade **flrRm** que contêm valores como `1a`, `2b`, `3a`e assim por diante.
  * Finalmente, tem uma propriedade **localização** que contém valores como `Redmond`, `Barcelona`, `Tokyo`e assim por diante. Para criar a exclusividade, seria designar todas as três essas propriedades como sua **ID de série de tempo** as chaves – **sensorId**, **flrRm**, e **localização**.

## <a name="next-steps"></a>Passos Seguintes

Leia mais sobre [do Azure Time Series Insights (pré-visualização) modelos de série de tempo](./time-series-insights-update-tsm.md).

Planear a sua [ambiente do Azure Time Series Insights (pré-visualização)](./time-series-insights-update-plan.md).