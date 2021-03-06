---
title: Dimensionar cluster do Explorador de dados do Azure para acomodar a pedido de alteração
description: Este artigo descreve os passos de escalamento horizontal e dimensionamento num cluster de Explorador de dados do Azure com base no pedido de alteração.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 15ef5282e0a073e870f2ac12b5fc442407535770
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408447"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Gerir o cluster aumentar horizontalmente para acomodar a pedido de alteração

Dimensionar um cluster adequadamente é essencial para o desempenho do Explorador de dados do Azure. Mas a pedido num cluster não é possível prever com precisão de 100%. Um tamanho de cluster estático pode levar a utilização insuficientemente ou utilização excessiva, nenhum dos quais é ideal. Uma abordagem melhor é *dimensionamento* um cluster, adicionar e remover a capacidade com o pedido de alteração. Existem dois fluxos de trabalho para dimensionamento, do aumento vertical e horizontal. Este artigo explica o fluxo de trabalho de escalamento horizontal.

Este artigo mostra-lhe como gerir o cluster Escalamento horizontal, também conhecido como o dimensionamento automático. Dimensionamento automático permite-lhe aumentar horizontalmente automaticamente com base em regras predefinidas e agendas de contagem de instâncias. Determine as definições de dimensionamento automático do seu cluster no portal do Azure, conforme descrito abaixo.

Navegue até ao seu cluster e, em **configurações** selecionar **aumentar horizontalmente**. Sob **configurar**, selecione **ativar o dimensionamento automático**.

![Ativar dimensionamento automático](media/manage-cluster-scaling/enable-autoscale.png)

O gráfico seguinte mostra o fluxo de passos seguintes. Fornecemos mais detalhes abaixo do elemento gráfico.

![Regra de dimensionamento](media/manage-cluster-scaling/scale-rule.png)

1. Sob **nome da definição de dimensionamento automático**, forneça um nome, tal como *Escalamento horizontal: colocar em cache utilização*.

1. Sob **modo de dimensionamento**, selecione **dimensionam com base numa métrica**. Esse modo oferecerá dimensionamento dinâmico; Também pode selecionar **Dimensionar para uma contagem de instâncias específica**.

1. Selecione **adicionar uma regra**.

1. Na **regra de dimensionamento** secção à direita, fornecer valores para cada definição.

    **Critérios**

    | Definição | Valor e descrição |
    | --- | --- | --- |
    | **Agregação de tempo** | Selecione um critério de agregação, como **média**. |
    | **Nome da métrica** | Selecione a métrica que pretende que a operação de dimensionamento seja baseada no, tal como **utilização da Cache**. |
    | **Estatística de intervalo de agregação de tempo** | Escolha entre **médio**, **mínimo**, **máxima**, e **soma**. |
    | **Operador** | Escolha a opção adequada, tais como **maior que ou igual a**. |
    | **Limiar** | Escolha um valor adequado. Por exemplo, 80% de utilização da cache, é um ponto de partida. |
    | **Duração (em minutos)** | Escolha uma quantidade apropriada de tempo para o sistema consultar novamente quando o cálculo de métricas. Comece com o padrão de 10 minutos. |
    |  |  |

    **Ação**

    | Definição | Valor e descrição |
    | --- | --- | --- |
    | **Operação** | Escolha a opção adequada para reduzir ou aumentar horizontalmente. |
    | **Contagem de instâncias** | Escolha o número de nós ou instâncias que pretende adicionar ou remover quando for cumprida uma condição de métrica. |
    | **Repouso (minutos)** | Escolha um intervalo de tempo adequado a aguardar entre as operações de dimensionamento. Comece com o padrão de cinco minutos. |
    |  |  |

1. Selecione **Adicionar**.

1. Na **limites de instância** secção no lado esquerdo, fornecer valores para cada definição.

    | Definição | Valor e descrição |
    | --- | --- | --- |
    | *Mínimo* | O número de instâncias que o cluster não serão dimensionados abaixo, independentemente da utilização. |
    | *Máximo* | O número de instâncias que o cluster não serão dimensionados acima, independentemente da utilização. |
    | *Predefinição* | O número predefinido de instâncias, usada se houver problemas ao ler métricas de recurso. |
    |  |  |

1. Selecione **Guardar**.

Agora que configurou uma operação de escalamento horizontal para o seu cluster do Explorador de dados do Azure. Adicione outra regra de uma operação de dimensionamento. Isto permite ao cluster dimensionar dinamicamente com base nas métricas que especificar.

Também é possível fazer [dimensionamento do cluster se](manage-cluster-scale-up.md) para o dimensionamento apropriado de um cluster.

Se precisar de assistência com problemas de dimensionamento do cluster, abra um pedido de suporte no [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
