---
title: Migrar VMs do Azure para discos geridos | Documentos da Microsoft
description: Migre máquinas virtuais do Azure criadas com discos não geridos nas contas de armazenamento para utilizar os Managed Disks.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 4aefc8c033383125a803eb0c8a38e2f3de119540
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55467922"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Migrar VMs do Azure para discos geridos no Azure

Managed Disks do Azure simplifica a gestão de armazenamento, eliminando a necessidade de gerir separadamente contas de armazenamento.  Também pode migrar as VMs do Azure existentes para os Managed Disks para beneficiar de confiabilidade de VMs num conjunto de disponibilidade. Ele garante que os discos de VMs diferentes no conjunto de disponibilidade é suficientemente isolados uns dos outros para evitar único ponto de falhas. Coloca automaticamente discos de VMs diferentes no conjunto de disponibilidade em unidades de escala de armazenamento diferentes (carimbos de data /) que limita o impacto das falhas de unidade de escala de armazenamento únicas provocadas por hardware e de falhas de software.
Com base nas suas necessidades, pode escolher entre dois tipos de opções de armazenamento:

- [Premium Managed Disks](premium-storage.md) são a unidade de estado sólido (SSD) com base em mídias de armazenamento que fornecem elevado desempenho, suporte de disco de baixa latência para máquinas virtuais que executam cargas de trabalho de e/S intensivas. Pode tirar partido da velocidade e o desempenho destes discos ao migrar para Premium Managed Disks.

- [Standard Managed Disks](standard-storage.md) utilizar suportes de dados de armazenamento de unidade de disco rígido (HDD) com base e são mais adequadas para programação/teste e outras cargas de trabalho de acesso pouco frequente menos sensíveis à variabilidade de desempenho.

Pode migrar para Managed Disks nos seguintes cenários:

| Migre...                                            | Hiperligação para a documentação                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Converter VMs autônomo e VMs no conjunto de disponibilidade para discos geridos   | [Converter VMs para utilizar discos geridos](convert-unmanaged-to-managed-disks.md) |
| Uma VM única de clássico para Resource Manager em discos geridos     | [Criar uma VM a partir de um VHD clássico](create-vm-specialized-portal.md)  | 
| Todas as VMs numa vNet da implementação clássica para Resource Manager em discos geridos     | [Migrar recursos de IaaS da implementação clássica para Resource Manager](migration-classic-resource-manager-ps.md) e, em seguida, [converter uma VM de discos não geridos para managed disks](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-the-conversion-to-managed-disks"></a>Plano para a conversão para o Managed Disks

Esta secção ajuda-o a tomar a melhor decisão em tipos VM e disco.


## <a name="location"></a>Localização

Escolha uma localização onde o Managed Disks do Azure estão disponíveis. Se estiver a mover para o Premium Managed Disks, certifique-se também que o armazenamento Premium está disponível na região em que pretende mover para. Ver [serviços do Azure por região](https://azure.microsoft.com/regions/#services) para obter informações atualizadas sobre localizações disponíveis.

## <a name="vm-sizes"></a>Tamanhos de VM

Se estiver a migrar para Premium Managed Disks, terá de atualizar o tamanho da VM para o armazenamento Premium com capacidade de tamanho disponível na região onde está localizada a VM. Reveja os tamanhos VM que são compatíveis com o armazenamento Premium. As especificações de tamanho de VM do Azure estão listadas na [tamanhos de máquinas virtuais](sizes.md).
Reveja as características de desempenho de máquinas virtuais que funcionam com o armazenamento Premium e escolha o tamanho VM mais adequado que melhor se adequa aos sua carga de trabalho. Certifique-se de que existe largura de banda suficiente disponível na sua VM para direcionar o tráfego de disco.

## <a name="disk-sizes"></a>Tamanhos de disco

**Premium Managed Disks**

Existem sete tipos de discos geridos premium, que podem ser utilizados com a VM e cada uma tem específicos IOPs e débito limites. Leve em consideração estes limites ao escolher o tipo de disco Premium para a sua VM com base nas necessidades da sua aplicação em termos de capacidade, desempenho, escalabilidade e cargas de pico.

| Tipo de discos Premium  | P4    | P6    | P10   | P15   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Tamanho do disco           | 32 GB| 64 GB| 128 GB| 256 GB|512 GB | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPs por disco       | 120   | 240   | 500   | 1100  |2300              | 5000              | 7500              | 7500              | 
| Débito por disco | 25 MB por segundo  | 50 MB por segundo  | 100 MB por segundo | 125 MB por segundo |150 MB por segundo | 200 MB por segundo | 250 MB por segundo | 250 MB por segundo |

**Discos geridos Standard**

Existem sete tipos de discos geridos padrão que podem ser utilizados com a VM. Cada um deles tem capacidade diferente, mas têm a mesma IOPS e limites de débito. Escolha o tipo de discos geridos Standard com base nas necessidades de capacidade do seu aplicativo.

| Tipo de Disco Standard  | S4               | S6               | S10              | S15              | S20              | S30              | S40              | S50              | 
|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------| 
| Tamanho do disco           | 30 GB            | 64 GB            | 128 GB           | 256 GB           |512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPs por disco       | 500              | 500              | 500              | 500              |500              | 500              | 500             | 500              | 
| Débito por disco | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo |60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 

## <a name="disk-caching-policy"></a>Política de colocação em cache do disco

**Premium Managed Disks**

Por predefinição, é a política de colocação em cache *só de leitura* em todos os discos de dados de Premium, e *leitura-escrita* para o disco de sistema operativo Premium ligados à VM. Esta definição de configuração é recomendada para alcançar o desempenho ideal para a IOs sua aplicação. Para discos de dados de escrita intensiva ou só de escrita (por exemplo, ficheiros de registo do SQL Server), desative o cache em disco para que pode obter um melhor desempenho do aplicativo.

## <a name="pricing"></a>Preços

Reveja os [os preços dos discos geridos](https://azure.microsoft.com/pricing/details/managed-disks/). Os preços dos Premium Managed Disks são mesmo que os discos não geridos Premium. Mas os preços dos discos geridos Standard são diferente de discos não geridos Standard.



## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [Managed Disks](managed-disks-overview.md)
