---
title: Capacidade de armazenamento de planeamento para o Azure Stack | Documentos da Microsoft
description: Saiba mais sobre o planeamento de implementações do Azure Stack a capacidade de armazenamento.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 09/18/2018
ms.openlocfilehash: 5d9d01a482483d030569a4dcad03c9ecef7cffc0
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245155"
---
# <a name="azure-stack-storage-capacity-planning"></a>Planeamento de capacidade de armazenamento de pilha do Azure
As secções seguintes fornecem Azure Stack capacidade de armazenamento informações de planeamento para auxiliar no planejamento das necessidades de armazenamento as soluções.

## <a name="uses-and-organization-of-storage-capacity"></a>Utiliza e a organização da capacidade de armazenamento
A configuração hiperconvergida do Azure Stack permite a partilha de dispositivos de armazenamento físicos. As três divisões principais de armazenamento disponível são entre a infraestrutura de armazenamento temporário de máquinas virtuais inquilinas e o armazenamento de blobs, tabelas e filas, os serviços de armazenamento consistente do Azure (ACS) de segurança.

## <a name="spaces-direct-cache-and-capacity-tiers"></a>Cache e níveis de capacidade de espaços direto
Há capacidade de armazenamento utilizada para o sistema operativo, o registo local, despejos e outro armazenamento temporário de infraestrutura necessidades. Esta capacidade de armazenamento local é separar (dispositivos e a capacidade) dos dispositivos de armazenamento colocados sob a gestão da configuração de espaços de armazenamento direto. O restante dos dispositivos de armazenamento é colocado num único conjunto de capacidade de armazenamento, independentemente do número de servidores em que a unidade de escala. Estes dispositivos são de dois tipos: Cache e a capacidade.  Os dispositivos de Cache são apenas esse – Cache. Espaços direto irá consumir estes dispositivos para repetição de escrita e colocação em cache de leitura. As capacidades desses dispositivos de Cache, embora utilizado, não são consolidadas a capacidade de formatado, "visível" dos formatado-discos virtuais. Os dispositivos de capacidade são utilizados para esta finalidade e fornecem a "localização principal" de dados geridos pelo espaços de armazenamento.

Todas as capacidade de armazenamento é alocada e gerenciada diretamente pela infraestrutura do Azure Stack. O operador tem de tomar decisões sobre a configuração, alocação, ou lidar com opções quando se trata de expansão de capacidade. Essas decisões de design se tornaram-se para se alinhar com os requisitos de solução e são automatizados durante a instalação/implantação inicial ou durante a expansão de capacidade. Detalhes sobre resiliência, capacidade de reserva para recompilações e outros detalhes têm sido considerados como parte do design. 

Operadores podem escolher entre um flash todos ou uma configuração de armazenamento híbrido:

![Planeamento de capacidade de armazenamento do Azure](media/azure-stack-capacity-planning/storage.png)

Na configuração de todos os flash, o cache é NVMe com uma opção de SATA SSD ou NVMe para capacidade. Numa configuração híbrida, o cache é uma escolha entre NVMe e SATA SSD enquanto a capacidade é HDD.

Segue-se um breve resumo sobre os espaços de armazenamento direto e a configuração de armazenamento do Azure Stack:
- Um agrupamento de espaços de armazenamento por unidade de escala (todos os dispositivos de armazenamento são configurados dentro de um conjunto único)
- Discos virtuais são criados como um espelho de copiar três para melhor desempenho e resiliência
- Cada disco virtual é formatado como um sistema de ficheiros ReFS
- Capacidade do disco virtual é calculada e alocada na maneira a deixar a quantidade de um dispositivo de capacidade de capacidade de dados não alocada no conjunto. Este é o equivalente de uma unidade de capacidade por servidor.
- Cada sistema de ficheiros ReFS terão o BitLocker ativado para encriptação de dados em repouso. 

Os-discos virtuais criados automaticamente e as suas capacidades são os seguintes:




|Name|Cálculo de capacidade|Descrição|
|-----|-----|-----|
|Dispositivo de arranque/local|Mínimo de 340 GB<sup>1</sup>|Armazenamento de servidor individual para imagens do sistema operativo e as VMs de infraestrutura "local"|
|Infraestrutura|3,5 TB|Todas as utilizações de infraestrutura do Azure Stack|
|VmTemp|Veja a seguir<sup>2</sup>|Máquinas virtuais inquilinas têm um disco temporário anexado e os dados estão armazenados nestes discos virtuais|
|ACS|Veja a seguir <sup>3</sup>|Capacidade de armazenamento consistente do Azure para blobs, tabelas e filas de manutenção|

<sup>1</sup> capacidade de armazenamento mínimo necessária de parceiro de solução do Azure Stack.

<sup>2</sup> é calculado o tamanho de disco virtual utilizado para discos temporários da Máquina Virtual de inquilino como um rácio de memória física do servidor. Conforme indicado nas tabelas abaixo para os tamanhos de VM de IaaS do Azure, o disco temporário é um rácio de memória física atribuída à máquina virtual. A alocação feita para o armazenamento de "disco temporário" no Azure Stack será efetuada numa maneira de capturar a maioria dos casos de utilização, mas pode não ser capaz de atender a todas as necessidades de armazenamento de disco temporário. O rácio escolhido, é preciso considerar disponibilizar armazenamento temporário enquanto está a consumir não a maioria da capacidade de armazenamento da solução para apenas a capacidade de disco temporário. Um disco de armazenamento temporário é criado por servidor em que a unidade de escala. A capacidade de armazenamento temporário não irá aumentar para além de 10% da capacidade de armazenamento geral disponível no agrupamento de armazenamento de unidade de escala. O cálculo é algo semelhante ao seguinte exemplo:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup> -discos virtuais criados para uso pelo ACS são uma divisão simple da capacidade restante. Como observado, todos os-discos virtuais são um espelho de três vias e valor de uma unidade de capacidade de capacidade para cada servidor é não alocado. Os vários-discos virtuais enumerados acima são alocados em primeiro lugar e a capacidade restante, em seguida, é utilizada para os ACS-discos virtuais.

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre a folha de cálculo de planeamento da capacidade do Azure Stack](capacity-planning-spreadsheet.md)
