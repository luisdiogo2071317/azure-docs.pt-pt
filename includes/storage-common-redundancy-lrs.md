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
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399996"
---
Armazenamento localmente redundante (LRS) foi concebido para proporcionar, pelo menos, 99,999999999% (11 9 s) a durabilidade dos objetos ao longo de um determinado ano ao replicar os seus dados dentro de uma unidade de escala de armazenamento. Uma unidade de escala de armazenamento está alojada num datacenter na região em que criou a conta de armazenamento. Um pedido de escrita para uma conta de armazenamento LRS com êxito devolve apenas depois dos dados foi escritos para todas as réplicas. Essas réplicas cada residem em separam domínios de falha e domínios dentro de uma unidade de escala de armazenamento de atualização.

Uma unidade de escala de armazenamento é uma coleção de racks de nós de armazenamento. Um domínio de falha (FD) é um grupo de nós que representam uma unidade física de falha e pode ser considerado como nós que pertencem ao mesmo rack físico. Um domínio de atualização (UD) é um grupo de nós que são atualizados em conjunto durante o processo de uma atualização do serviço (implementação). As réplicas são distribuídas em UDs e FDs dentro de uma unidade de escala de armazenamento. Esta arquitetura garante que os dados estão disponíveis se uma falha de hardware afeta um bastidor único ou quando nós sejam atualizados durante uma implementação.

LRS é a opção de replicação de custo mais baixa e oferece a durabilidade, pelo menos, em comparação comparada outras opções. Se ocorrer um desastre ao nível do Centro de dados (por exemplo, acionar ou sobrecarregar), todas as réplicas podem ser perdidas ou irrecuperáveis. Para mitigar este risco, a Microsoft recomenda a utilização de armazenamento com redundância de zona (ZRS) ou armazenamento georredundante (GRS).

* Se a aplicação armazena os dados que podem facilmente ser reconstruídos o caso de perda de dados, pode optar por LRS.
* Alguns aplicativos são restritos para replicar dados apenas dentro de um país devido a requisitos de governação de dados. Em alguns casos, as regiões emparelhadas em que os dados são replicados para contas GRS poderão estar em outro país. Para obter mais informações sobre as regiões emparelhadas, consulte [regiões do Azure](https://azure.microsoft.com/regions/).
