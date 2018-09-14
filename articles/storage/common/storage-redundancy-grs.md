---
title: Armazenamento georredundante (GRS) de durabilidade de inter-regional no armazenamento do Azure | Documentos da Microsoft
description: Armazenamento georredundante (GRS) replica os dados entre duas regiões que distam centenas de quilómetros de distância. GRS protege contra falhas de hardware no Centro de dados, bem como de desastres regionais.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 03/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: be3d0d32e60e23b2b2d7d414d2297b86dec62f1d
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576838"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Armazenamento georredundante (GRS): replicação de inter-regional do armazenamento do Azure
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Armazenamento georredundante com acesso de leitura
Armazenamento georredundante com acesso de leitura (RA-GRS) maximiza a disponibilidade para a sua conta de armazenamento. RA-GRS fornece acesso só de leitura aos dados na localização secundária, além de georreplicação entre duas regiões.

Quando ativar o acesso só de leitura aos seus dados na região secundária, os dados estão disponíveis num ponto final secundário, bem como no ponto final primário para a sua conta de armazenamento. O ponto final secundário é semelhante para o ponto final primário, mas acrescenta o sufixo `–secondary` ao nome da conta. Por exemplo, se o seu ponto final primário para o serviço Blob `myaccount.blob.core.windows.net`, em seguida, o ponto final secundário é `myaccount-secondary.blob.core.windows.net`. As chaves de acesso para a sua conta de armazenamento são os mesmos para os pontos de extremidade primários e secundários.

Algumas considerações a ter em conta ao utilizar o RA-GRS:

* A aplicação tem de gerir o ponto final está interagindo com ao utilizar o RA-GRS.
* Uma vez que a replicação assíncrona envolve um atraso, as alterações que ainda não tem sido replicadas para a região secundária podem ser perdidas se não não possível recuperar os dados da região primária, por exemplo em caso de desastre regional.
* Pode verificar a hora da última sincronização da sua conta de armazenamento. Hora da última sincronização é um valor de data/hora GMT. Todas as escritas primárias antes da hora da última sincronização foi escritas com êxito para a localização secundária, que significa que eles estão disponíveis a serem lidos a partir da localização secundária. Primário escreve após a hora da última sincronização pode ou não estar disponível para leituras ainda. Pode consultar este valor usando o [portal do Azure](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), ou a partir de uma das bibliotecas de cliente do armazenamento do Azure.
* Se o Microsoft inicie a ativação pós-falha para a região secundária, que será leu e acesso de escrita a esses dados após a ativação pós-falha foi concluída. Para obter mais informações, consulte [orientações sobre a recuperação após desastre](storage-disaster-recovery-guidance.md).
* Para obter informações sobre como mudar para a região secundária, consulte [o que fazer se ocorrer uma falha de armazenamento do Azure](storage-disaster-recovery-guidance.md).
* Destina-se o RA-GRS para fins de elevada disponibilidade. Para obter orientações de escalabilidade, reveja os [lista de verificação de desempenho](storage-performance-checklist.md).
* Para obter sugestões sobre como estruturar para elevada disponibilidade com RA-GRS, consulte [criação altamente disponíveis de aplicativos com o armazenamento RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>O que é o RPO e RTO com GRS?
**Objetivo de ponto de recuperação (RPO):** no GRS e RA-GRS, o armazenamento de forma assíncrona service geo-replica os dados dos principais para a localização secundária. Em caso de desastre regional principais na região primária, a Microsoft efetua uma ativação pós-falha para a região secundária. Se ocorrer uma ativação pós-falha, as alterações recentes que não tenham ainda sido georreplicado poderão perder-se. O número de minutos de potenciais dados perdidos é referido como o RPO e indica o ponto no tempo para que os dados podem ser recuperados. Normalmente, o armazenamento do Azure tem um RPO de menos de 15 minutos, embora não existe atualmente nenhum SLA no georreplicação quanto tempo demora.

**Objetivo de tempo de recuperação (RTO):** o RTO é uma medida de tempo que demora para efetuar a ativação pós-falha e obter a conta de armazenamento online. A hora a realizar a ativação pós-falha inclui as seguintes ações:

   * O tempo que a Microsoft requer para determinar se os dados podem ser recuperados na localização principal, ou se uma ativação pós-falha é necessária.
   * O tempo para efetuar a ativação pós-falha da conta do storage, alterando as entradas de DNS primárias para apontar para a localização secundária.

   A Microsoft leva a responsabilidade de preservar os seus dados muito a sério. Se houver alguma chance de recuperação de dados na região primária, a Microsoft irá atrasar a ativação pós-falha e se concentrar em recuperar seus dados. Uma versão futura do serviço permitirá que acionar uma ativação pós-falha ao nível da conta para que pode controlar o RTO por conta própria.

## <a name="paired-regions"></a>Regiões emparelhadas 
Quando criar uma conta de armazenamento, selecione a região primária para a conta. Região secundária associada é determinado com base na região principal e não pode ser alterado. Para obter informações atualizadas sobre as regiões suportadas pelo Azure, consulte [Business continuidade e recuperação após desastre (BCDR): regiões emparelhadas do Azure](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Consulte também
- [Replicação do Armazenamento do Azure](storage-redundancy.md)
- [Armazenamento localmente redundante (LRS): redundância de dados de baixo custo do armazenamento do Azure](storage-redundancy-lrs.md)
- [Armazenamento com redundância de zona (ZRS): aplicações de armazenamento do Azure de elevada disponibilidade](storage-redundancy-zrs.md)