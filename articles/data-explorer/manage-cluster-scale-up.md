---
title: Dimensionar cluster do Explorador de dados do Azure para acomodar a pedido de alteração
description: Este artigo descreve os passos de escalamento horizontal e dimensionamento num cluster de Explorador de dados do Azure com base no pedido de alteração.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 213a49d87d5e9f801bb17604a322c231a4e3dee2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735817"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Gerir o cluster de dimensionamento até acomodar a demanda de alteração

Dimensionar um cluster adequadamente é essencial para o desempenho do Explorador de dados do Azure. Mas a pedido num cluster não é possível prever com precisão de 100%. Um tamanho de cluster estático pode levar a utilização insuficientemente ou utilização excessiva, nenhum dos quais é ideal. Uma abordagem melhor é *dimensionamento* um cluster, adicionar e remover a capacidade e de CPU com a alteração a pedido. Este artigo mostra-lhe como gerir o dimensionamento do cluster cópia de segurança.

Navegue até ao seu cluster e, em **configurações** selecionar **aumentar verticalmente**.

Poderia, em seguida, a chance a lista de SKUs que estão disponíveis. Pode escolher entre a lista de cartões ativados. Por exemplo no abaixo figura é apenas um SKU que pode ser selecionado do D14_vs.

![Aumentar verticalmente](media/manage-cluster-scale-up/scale-up.png)

D13_v2 está desativada porque este é o SKU atual do cluster. L8 e L16 estão desativados porque eles não está disponível na região onde está o cluster.

Para alterar o SKU de um clique de apenas de SKU, gostaria de utilizar e clique nas **selecione** botão.

[!NOTE] O processo de dimensionamento pode demorar alguns minutos e, durante esse período o cluster será suspensa. Tenha em atenção que a reduzir verticalmente pode prejudicar o desempenho do seu cluster.

Se precisar de assistência com problemas de dimensionamento do cluster, abra um pedido de suporte no [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).