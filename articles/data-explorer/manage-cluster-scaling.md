---
title: Dimensionar cluster do Explorador de dados do Azure para acomodar a pedido de alteração
description: Este artigo descreve os passos de escalamento horizontal e dimensionamento num cluster de Explorador de dados do Azure com base no pedido de alteração.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 59a6a94e2906413423a4ae03a7c1c115b2ec0cc0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047925"
---
# <a name="manage-cluster-scaling-to-accommodate-changing-demand"></a>Gerir o dimensionamento do cluster para acomodar a pedido de alteração

Dimensionar um cluster adequadamente é essencial para o desempenho do Explorador de dados do Azure. Mas a pedido num cluster não é possível prever com precisão de 100%. Um tamanho de cluster estático pode levar a utilização insuficientemente ou utilização excessiva, nenhum dos quais é ideal. Uma abordagem melhor é *dimensionamento* um cluster, adicionar e remover a capacidade com o pedido de alteração. Este artigo mostra-lhe como gerir o dimensionamento do cluster.

Navegue até ao seu cluster e, em **configurações** selecionar **aumentar horizontalmente**. Sob **configurar**, selecione **ativar o dimensionamento automático**.

![Ativar dimensionamento automático](media/manage-cluster-scaling/enable-autoscale.png)

O gráfico seguinte mostra o fluxo de passos seguintes. Fornecemos mais detalhes abaixo do elemento gráfico.

![Regra de dimensionamento](media/manage-cluster-scaling/scale-rule.png)

1. Sob **nome da definição de dimensionamento automático**, forneça um nome, tal como *Escalamento horizontal: colocar em cache utilização*.

1. Sob **modo de dimensionamento**, selecione **dimensionam com base numa métrica**. Esse modo oferecerá dimensionamento dinâmico; Também pode selecionar **Dimensionar para uma contagem de instâncias específica**.

1. Selecione **adicionar uma regra**.

1. Na **regra de dimensionamento** secção à direita, fornecer valores para cada definição.

    | Definição | Valor e descrição |
    | --- | --- | --- |
    | **Agregação de tempo** | Selecione um critério de agregação, como **média**. |
    | **Nome da métrica** | Selecione a métrica que pretende que a operação de dimensionamento seja baseada no, tal como **utilização da Cache**. |
    | **Estatística de intervalo de agregação de tempo** | Escolha entre **médio**, **mínimo**, **máxima**, e **soma**. |
    | **Operador** | Escolha a opção adequada, tais como **maior que ou igual a**. |
    | **Limiar** | Escolha um valor adequado. Por exemplo, 80% de utilização da cache, é um ponto de partida. |
    | **Duração** | Escolha uma quantidade apropriada de tempo para o sistema consultar novamente quando o cálculo de métricas. Comece com a predefinição de dez minutos. |
    | **Operação** | Escolha a opção adequada para reduzir horizontalmente ou aumentar horizontalmente. |
    | **Contagem de instâncias** | Escolha o número de nós ou instâncias que pretende adicionar ou remover quando for cumprida uma condição de métrica. |
    | **Repouso (minutos)** | Escolha um intervalo de tempo adequado a aguardar entre as operações de dimensionamento. Comece com o padrão de cinco minutos. |
    |  |  |

1. Selecione **Adicionar**.

1. Na **limites de instância** secção no lado esquerdo, fornecer valores para cada definição.

    | Definição | Valor e descrição |
    | --- | --- | --- |
    | *Mínimo* | Este é o número de instâncias de que o cluster não será escalado abaixo, independentemente da utilização. |
    | *Máximo* | Este é o número de instâncias de que o cluster não será escalado acima, independentemente da utilização. |
    | *Predefinição* | O número predefinido de instâncias, usada se houver um problema ao ler métricas de recurso. |
    |  |  |

1. Selecione **Guardar**.

Agora que configurou uma operação de escalamento horizontal para o seu cluster do Explorador de dados do Azure. Adicione outra regra de uma operação de dimensionamento. Isto permite ao cluster dimensionar dinamicamente com base em métricas de utilização que especificar.

Se precisar de assistência com problemas de dimensionamento do cluster, abra um pedido de suporte no [portal do Azure](https://portal.azure.com).