---
title: Noções sobre faturação do Azure CDN | Microsoft Docs
description: Estas FAQ descreve como funciona a faturação da CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: v-deasim
ms.openlocfilehash: 218c493c772dc8fd212efaf60a0599fa2e896411
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32163519"
---
# <a name="understanding-azure-cdn-billing"></a>Noções sobre faturação da CDN do Azure

Estas FAQ descreve a estrutura de faturação para o conteúdo alojado pela rede de entrega de conteúdos do Azure (CDN).

## <a name="what-is-a-billing-region"></a>O que é uma região de faturação?
Uma região de faturação é uma área geográfica utilizada para determinar a velocidade a que é cobrada de fornecimento de objetos da CDN do Azure. As zonas de faturação atuais e os respetivos regiões são os seguintes:

- Zona 1: América do Norte, Europa, Médio Oriente e África

- Zona 2: Ásia-Pacífico (incluindo Japão)

- Zona 3: América do Sul

- Zona 4: Austrália e Nova Zelândia

- Zona 5: Índia

Para obter informações sobre regiões de ponto de presença (POP), consulte [localizações de POP do CDN do Azure por região](https://docs.microsoft.com/azure/cdn/cdn-pop-locations). Por exemplo, um POP localizado em México é a região da América do Norte e, por isso, está incluído na zona 1. 

Para obter informações sobre os preços da CDN do Azure, consulte [preços de rede de entrega de conteúdos](https://azure.microsoft.com/is-is/pricing/details/cdn/).

## <a name="how-are-delivery-charges-calculated-by-region"></a>Como são calculados os custos de entrega por região?
A região de faturação da CDN do Azure é com base na localização do servidor de origem, a entrega de conteúdo para o utilizador final. O destino (localização física) do cliente não é considerado a região de faturação.

Por exemplo, se um utilizador localizado em México emite um pedido e este pedido é servido por um servidor localizado no POP dos Estados Unidos devido a condições de peering ou tráfego, a região de faturação serão dos Estados Unidos.

## <a name="what-is-a-billable-azure-cdn-transaction"></a>O que é uma transação sujeito a faturação do CDN do Azure?
Qualquer pedido de HTTP (S) que termina no CDN é um evento de sujeito a faturação, que inclui todos os tipos de resposta: com êxito, falha ou outro. No entanto, as respostas diferentes poderão gerar quantidades de tráfego diferentes. Por exemplo, *304 não modificado* e outras respostas só de cabeçalho geram tráfego reduzida, porque são uma pequena resposta de cabeçalho; do mesmo modo, as respostas de erro (por exemplo, *404 não encontrado*) são facturável mas cobrado um custo pequeno devido ao payload de resposta muito pequena.

## <a name="what-other-azure-costs-are-associated-with-azure-cdn-use"></a>Que outros custos do Azure estão associados a utilização da CDN do Azure?
Também utilizar a CDN do Azure implica alguns custos de utilização nos serviços de utilizado como origem para os objetos. Estes custos são, normalmente, uma pequena fração do custo de utilização geral do CDN.

Se estiver a utilizar armazenamento de Blobs do Azure como origem para o conteúdo, também pode implicar os seguinte custos de armazenamento de cache preenche:

- Real GB utilizado: O armazenamento real dos seus objetos de origem.

- As transferências em GB: A quantidade de dados transferidos para preencher as caches da CDN.

- Transações: conforme necessário para preencher a cache.

Para mais informações sobre faturação de armazenamento do Azure, consulte [compreender Azure armazenamento faturação – largura de banda, as transações e a capacidade](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Se estiver a utilizar *alojado entrega do serviço*, será cobrado o custo da seguinte forma:

- Tempo de computação do Azure: as instâncias de computação que atuam como a origem.

- Transferência de computação do Azure: os dados transferidos a partir de instâncias de computação para preencher as caches da CDN do Azure.

Se o cliente utiliza os pedidos de intervalo de bytes (independentemente do serviço de origem), aplicam as seguintes considerações:

- A *pedido de intervalo de bytes* é uma transação facturável na CDN. Quando um cliente emite um pedido de intervalo de bytes, este pedido é para um subconjunto (intervalo) do objeto. A CDN responde com apenas uma parte parcial do conteúdo que é pedido. Esta resposta parcial é uma transação sujeito a faturação e a quantidade de transferência está limitada ao tamanho da resposta de intervalo (mais cabeçalhos).

- Quando um pedido chega para apenas uma parte de um objeto (especificando um cabeçalho de intervalo de bytes), a CDN poderá obter todo o objeto para a sua cache. Como resultado, apesar da transação sujeito a faturação da CDN destina-se uma resposta parcial, a transação sujeito a faturação da origem poderá envolver o respetivo tamanho total do objeto.

## <a name="how-much-transfer-activity-occurs-to-support-the-cache"></a>Ocorre quanto atividade de transferência para a cache de suportar?
Sempre que precisar de um POP do CDN para preencher a cache, faz um pedido para a origem para o objeto que está a ser colocados em cache. Como resultado, a origem implica uma transação facturável em cada falha de acerto na cache. O número de pedidos sem êxito de cache depende de vários fatores:

- O conteúdo como colocáveis é: se o conteúdo tem elevada TTL (time-to-live) / expiração, os valores e é acedidos com frequência, pelo que esta permanece populares na cache, em seguida, a vasta maioria da carga é processado pela CDN. Um rácio de boa-acertos na cache de típico é bem que mais de 90%, que significa que menos de 10% de pedidos de cliente tem de voltar a origem, para uma falha de acerto na cache ou o objeto de atualizar.

- O número de nós tem de carregar o objeto: sempre que um nó carrega um objeto de origem, implica uma transação sujeito a faturação. Como resultado, o conteúdo mais global (acedido a partir de mais nós) resulta em mais facturável transações.

- Influência TTL: um valor de TTL superior para um objeto significa tem de ser obtido com menos frequência da origem. Isto também significa que os clientes, tais como navegadores, podem colocar em cache o objeto superior, pode reduzir as transações para a CDN.

## <a name="how-do-i-manage-my-costs-most-effectively"></a>Como gerir os meus os custos de forma mais eficaz?
Defina o valor de TTL mais longo possíveis no seu conteúdo. 
