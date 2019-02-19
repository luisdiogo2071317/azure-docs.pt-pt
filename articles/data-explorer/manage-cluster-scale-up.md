---
title: Dimensionar cluster do Explorador de dados do Azure para acomodar a pedido de alteração
description: Este artigo descreve os passos para aumentar verticalmente e reduzir verticalmente um cluster do Explorador de dados do Azure com base no pedido de alteração.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 9d265ec7f0ce2030874f38b99b07343f1d4a3f4d
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408651"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Gerir o aumento vertical de cluster para acomodar a pedido de alteração

Dimensionar um cluster adequadamente é essencial para o desempenho do Explorador de dados do Azure. Mas a pedido num cluster não é possível prever com precisão de 100%. Um tamanho de cluster estático pode levar a utilização insuficientemente ou utilização excessiva, nenhum dos quais é ideal. Uma abordagem melhor é *dimensionamento* um cluster, adicionar e remover a capacidade e de CPU com a alteração a pedido. Existem dois fluxos de trabalho para dimensionamento, do aumento vertical e horizontal. Este artigo mostra-lhe como gerir o aumento vertical de cluster.

1. Navegue até ao seu cluster e, em **configurações** selecionar **aumentar verticalmente**.

    É-lhe fornecido uma lista de SKUs disponíveis. Por exemplo, na imagem abaixo há apenas um SKU disponível: D14_V2.

    ![Aumentar verticalmente](media/manage-cluster-scale-up/scale-up.png)

    D13_V2 está desativada uma vez que é o SKU atual do cluster. L8 e L16 estão desativados porque eles não está disponível na região onde está localizado o cluster.

1. Para alterar o SKU, selecione o SKU que pretende e prima a **selecione** botão.

> [!NOTE]
> O processo de dimensionamento pode demorar alguns minutos. Durante esse tempo, o cluster será suspensa.
>
> Reduzir verticalmente, pode prejudicar o desempenho do seu cluster.
>

Agora já realizou uma operação de aumentar ou reduzir verticalmente para o seu cluster do Explorador de dados do Azure. Também é possível fazer [Escalamento horizontal do cluster](manage-cluster-scale-out.md), também conhecido como o dimensionamento automático, para dimensionar de forma dinâmica com base em métricas que especificar.

Se precisar de assistência com problemas de dimensionamento do cluster, abra um pedido de suporte no [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
