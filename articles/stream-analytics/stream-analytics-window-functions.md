---
title: Introdução às funções de sistema baseado em janelas do Azure Stream Analytics
description: Este artigo descreve as quatro modos de janela funções (em cascata, hopping, a deslizante, sessão) que são utilizadas nas tarefas do Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 2650058e277bc0338c779655ce381be046fb120a
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Introdução às funções de sistema baseado em janelas do Stream Analytics
Em cenários de transmissão em fluxo tempo, executar operações nos dados contidos no windows temporais é um padrão comum. Do Stream Analytics tem suporte nativo para funções de sistema baseado em janelas, permitindo aos programadores para as tarefas de processamento de fluxo complexas de autor sem qualquer esforço.

Existem quatro tipos de temporal windows à sua escolha: [ **em cascata**](https://msdn.microsoft.com/azure/stream-analytics/reference/tumbling-window-azure-stream-analytics), [ **Hopping**](https://msdn.microsoft.com/azure/stream-analytics/reference/hopping-window-azure-stream-analytics), [  **A deslizante**](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics), e [ **sessão** ](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) windows.  Utilize as funções de janela no [ **GROUP BY** ](https://msdn.microsoft.com/azure/stream-analytics/reference/group-by-azure-stream-analytics) cláusula da sintaxe de consulta nas suas tarefas do Stream Analytics.

Todos os o [modos de janela](https://msdn.microsoft.com/azure/stream-analytics/reference/windowing-azure-stream-analytics) operações saída resultados no **final** da janela. A saída da janela serão evento único com base na função de agregação utilizada. O evento de saída serão o carimbo de hora de fim da janela e todas as funções de janela estão definidas com um comprimento fixo. 

![Conceitos de funções de janela do Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Janela em cascata
Em cascata janela funções são utilizadas para segmentar um fluxo de dados em segmentos de tempo distintos e efetuar uma função contra eles, como o exemplo abaixo. As chaves differentiators de uma janela em cascata são que eles repetir, não sobrepõem-se de um evento não é possível pertencem a mais do que uma janela em cascata.

![Janela em cascata do Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Janela de salto
Salto salto de funções de janela reencaminhar na hora por um período fixo. Poderá ser mais fácil considerá-los como janelas em cascata podem sobrepor-se, pelo que os eventos podem pertencer a mais do que um conjunto de resultados de janela Hopping. Para tornar uma janela de Hopping os mesmos como uma janela em cascata, especifique o tamanho de salto deve ser o mesmo que o tamanho da janela. 

![Janela de salto do Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Numa janela deslizante
Funções de janela deslizante, ao contrário do salto windows, ou em cascata produzem uma saída **apenas** quando ocorre um evento. Cada janela vai ter, pelo menos, um evento e a janela continuamente move reencaminhar por um € (epsilon). Como hopping windows, os eventos podem pertencer a mais do que uma janela deslizante.

![Janela deslizante do Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window-preview"></a>Janela de sessão (pré-visualização)
As funções de janela de sessão agrupar os eventos que chegam vezes semelhantes, filtrar períodos de tempo em que não existem dados. Tem três parâmetros principais: tempo limite, duração máxima e a chave de criação de partições (opcional).

![Janela de sessão do Stream Analytics](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

Uma janela de sessão começa quando o primeiro evento ocorre. Se outro evento ocorre dentro do tempo limite especificado a partir do último evento transmissões em, a janela expande para incluir o evento de novo. Caso contrário, se não há eventos ocorrem dentro do tempo limite, a janela ser fechada, o tempo limite.

Se eventos mantém ocorrer dentro do tempo limite especificado, manter alargar a janela de sessão até que a duração máxima for atingida. Os intervalos da verificar se a duração máxima são definidos para ser o mesmo tamanho que a duração máxima especificada. Por exemplo, se a duração máxima é de 10, em seguida, as verificações de se a janela excedem o máximo duração irá acontecer em t = 0, 10, 20, 30, etc.

Quando uma chave de partição é fornecida, os eventos são agrupados em conjunto pela chave e a janela de sessão é aplicada a cada grupo de forma independente. A criação de partições é útil nos casos em que precisa de windows de sessão diferente para diferentes utilizadores ou dispositivos.


## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

