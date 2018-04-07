---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: jeking
ms.custom: include file
ms.openlocfilehash: f5b6e0e74bbab33b9ae6fbacca5c55ea434d3e41
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
Armazenamento georredundante (GRS) foi concebido para fornecer, pelo menos, 99.99999999999999% (16 do 9) durabilidade dos objetos através de um ano fornecido pelo replicar os dados para uma região secundária que é centenas de quilómetros região primária. Se a sua conta do storage tiver GRS ativada, em seguida, os dados são duráveis mesmo em caso de uma falha regional completa ou de um desastre no qual a região primária não é recuperável.

Se optar por para GRS, tem duas opções relacionadas para escolher entre:

* GRS replica os dados para outro centro de dados numa região secundária, mas esse dados estão disponíveis para ser só de leitura se Microsoft inicia uma ativação pós-falha do primário para a região secundária. 
* Armazenamento georredundante com acesso de leitura (RA-GRS) baseia GRS. RA-GRS replica os dados para outro centro de dados numa região secundária e também fornece a opção para ler a partir da região secundária. Com RA-GRS, pode ler a partir do secundário, independentemente se o Microsoft inicia uma ativação pós-falha do primário para o secundário. 

Para uma conta de armazenamento com a GRS ou RA-GRS ativada, todos os dados pela primeira vez é replicado com armazenamento localmente redundante (LRS). Uma atualização primeiro é consolidar para a localização primária e replicados utilizando LRS. A atualização, em seguida, é replicada de forma assíncrona para a região secundária utilizando GRS. Quando os dados são escritos para a localização secundária, também são replicado dentro dessa localização utilizando LRS. 

Regiões primários e secundários gerir réplicas em vários domínios de falhas separada e atualizar domínios dentro de uma unidade de escala de armazenamento. A unidade de escala de armazenamento é a unidade de replicação básica no Centro de dados. A replicação a este nível é fornecida pela LRS; Para obter mais informações, consulte [armazenamento localmente redundante (LRS): redundância de dados de baixo custo de armazenamento do Azure](../articles/storage/common/storage-redundancy-lrs.md).

Tenha em consideração estes pontos quando decidir que opção de replicação a utilizar:

* Armazenamento com redundância de zona (ZRS) fornece elevada disponibilidade com replicação síncrona e pode ser uma melhor opção para alguns cenários que GRS ou RA-GRS. Para obter mais informações sobre o ZRS, consulte [ZRS](../articles/storage/common/storage-redundancy-zrs.md).
* Porque a replicação assíncrona envolve um atraso, na eventualidade de ocorrer um desastre regional é possível que as alterações que ainda não tem sido replicadas para a região secundária serão perdidas se não não possível recuperar os dados da região primária.
* Com a GRS, a réplica não está disponível, a menos que a Microsoft inicia a ativação pós-falha para a região secundária. Se o Microsoft inicie uma ativação pós-falha para a região secundária, será leu e acesso de escrita para que os dados após a ativação pós-falha foi concluída. Para obter mais informações, consulte [orientações de recuperação após desastre](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Se a aplicação tem de ler a partir da região secundária, ative o RA-GRS.

## <a name="read-access-geo-redundant-storage"></a>Armazenamento georredundante com acesso de leitura

Armazenamento georredundante com acesso de leitura (RA-GRS) maximiza a disponibilidade para a sua conta de armazenamento. RA-GRS fornece acesso só de leitura aos dados na localização secundária, além de georreplicação em duas regiões.

Quando ativar o acesso só de leitura aos seus dados na região secundária, os dados estão disponíveis um ponto final secundário, bem como o ponto final principal para a sua conta de armazenamento. O ponto final secundário é semelhante para o ponto final principal, mas acrescenta o sufixo `–secondary` ao nome da conta. Por exemplo, se o ponto final principal para o serviço Blob é `myaccount.blob.core.windows.net`, em seguida, o ponto final secundário é `myaccount-secondary.blob.core.windows.net`. As chaves de acesso para a sua conta de armazenamento são as mesmas para os pontos de finais primários e secundários.

Algumas considerações a lembrar ao utilizar o RA-GRS:

* A aplicação tem de gerir o ponto final de interagir com ao utilizar o RA-GRS.
* Uma vez que a replicação assíncrona envolve um atraso, as alterações que ainda não tem sido replicadas para a região secundária podem ser perdidas se não não possível recuperar os dados da região primária, por exemplo na eventualidade de ocorrer um desastre regional.
* Pode verificar a hora da última sincronização da sua conta de armazenamento. Hora da última sincronização é um valor de data/hora GMT. Todas as escritas primárias antes da hora da última sincronização tem sido escrita com êxito na localização secundária, o que significa que estão disponíveis para ler a partir da localização secundária. Escreve principal após a hora da última sincronização pode ou não estar disponível para leituras ainda. Pode consultar este valor a utilizar o [portal do Azure](https://portal.azure.com/), [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), ou uma das bibliotecas de cliente do Storage do Azure.
* Se o Microsoft inicia a ativação pós-falha para a região secundária, será leu e acesso de escrita para que os dados após a ativação pós-falha foi concluída. Para obter mais informações, consulte [orientações de recuperação após desastre](../articles/storage/common/storage-disaster-recovery-guidance.md).
* Para obter informações sobre como mudar para a região secundária, consulte [o que fazer se ocorrer uma falha de armazenamento do Azure](../articles/storage/common/storage-disaster-recovery-guidance.md).
* RA-GRS destina-se a fins de elevada disponibilidade. Para obter orientações de escalabilidade, reveja o [lista de verificação de desempenho](../articles/storage/common/storage-performance-checklist.md).
* Para sugestões sobre como conceber para elevada disponibilidade com RA-GRS, consulte [conceber altamente disponível aplicações utilizando o armazenamento RA-GRS](../articles/storage/common/storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Qual é o RPO e RTO com a GRS?
**Objetivo de ponto de recuperação (RPO):** no GRS e RA-GRS, o armazenamento no modo assíncrono service georreplicação-são replicados os dados a partir de principal para a localização secundária. Em caso de desastre grave regional na região primária, a Microsoft efetua uma ativação pós-falha para a região secundária. Se uma ativação pós-falha acontecer, as alterações recentes à que não ainda foram georreplicação poderão perder-se. O número de minutos de potenciais dados perdidos é referido como o RPO e indica o ponto no tempo para que os dados podem ser recuperados. Normalmente, o Storage do Azure tem um RPO de menos de 15 minutos, apesar de não existe atualmente nenhum SLA no georreplicação quanto tempo demora.

**Objetivo de tempo de recuperação (RTO):** o RTO é uma medida do tempo que demora para efetuar a ativação pós-falha e obter a conta de armazenamento online. O tempo para efetuar a ativação pós-falha inclui as seguintes ações:

   * A hora exige o Microsoft para determinar se os dados podem ser recuperados na localização principal ou se uma ativação pós-falha é necessária.
   * O tempo para efetuar a ativação pós-falha da conta do storage alterando as entradas DNS principais para apontar para a localização secundária.

   Microsoft assume a responsabilidade de preservar os seus dados muito a sério. Se não houver qualquer possibilidade de recuperar os dados na região primária, o Microsoft atrasar a ativação pós-falha e concentrar-se na recuperar os dados. Versão futura do serviço permitirá acionar uma ativação pós-falha ao nível de uma conta para que pode controlar o RTO por si.

## <a name="paired-regions"></a>Regiões emparelhadas 

Quando cria uma conta de armazenamento, selecione a região primária para a conta. A região secundária emparelhada é determinada com base na região primária e não pode ser alterada. Para obter informações atualizadas sobre regiões suportadas pelo Azure, consulte [recuperação de continuidade e desastre de negócio (BCDR): Azure emparelhado regiões](../articles/best-practices-availability-paired-regions.md).
