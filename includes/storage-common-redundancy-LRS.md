---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 58d81bd4c1ce4b3af91a335039f62df08b340576
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
Armazenamento localmente redundante (LRS) foi concebido para fornecer, pelo menos, 99.999999999% (11 da 9) durabilidade dos objetos através de um ano fornecido pelo replicar os dados dentro de uma unidade de escala de armazenamento. Uma unidade de escala de armazenamento está alojada num centro de dados na região na qual criou a conta de armazenamento. Um pedido de escrita para uma conta de armazenamento LRS devolve com êxito apenas depois dos dados foram escritos na todas as réplicas. Estas réplicas, cada um reside em domínios de falhas de separar e domínios dentro de uma unidade de escala de armazenamento de atualização.

Uma unidade de escala de armazenamento é uma coleção de bastidores de nós de armazenamento. Um domínio de falhas (DF) é um grupo de nós que representam uma unidade física de falha e pode ser considerada como nós que pertencem ao mesmo bastidor físico. Um domínio de atualização (UD) é um grupo de nós que sejam atualizados em conjunto durante o processo de uma atualização do serviço (implementação). As réplicas são distribuídas por UDs e FDs dentro de uma unidade de escala de armazenamento. Esta arquitetura garante que os seus dados estão disponíveis se uma falha de hardware afeta um bastidor único ou quando nós sejam atualizados durante uma implementação.

LRS é a opção de replicação de custo mais baixa e oferece as características de durabilidade, pelo menos, em comparação comparada outras opções. Se ocorrer um desastre ao nível do Centro de dados (por exemplo, fire ou sobrecarregar), todas as réplicas podem ser perdido ou irrecuperáveis. Para mitigar este risco, a Microsoft recomenda a utilização de armazenamento com redundância de zona (ZRS) ou armazenamento georredundante (GRS).

* Se a aplicação armazena os dados que podem ser facilmente é reconstruir se ocorrer perda de dados, poderá optar por LRS.
* Algumas aplicações estão limitadas a replicar dados apenas dentro de um país devido a requisitos de governação de dados. Em alguns casos, as regiões emparelhadas em que os dados são replicados para contas GRS poderão estar em noutro país. Para obter mais informações sobre regiões emparelhadas, consulte [regiões do Azure](https://azure.microsoft.com/regions/).
