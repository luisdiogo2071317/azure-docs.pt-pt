---
title: Disponibilidade de SAP HANA em regiões do Azure | Microsoft Docs
description: Uma descrição geral das considerações de disponibilidade durante a execução de SAP HANA em VMs do Azure em várias regiões do Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: edbd1885dd529e4ccd38f2012d56865a2147f64d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30842276"
---
# <a name="sap-hana-availability-across-azure-regions"></a>Disponibilidade de SAP HANA em regiões do Azure

Este artigo descreve cenários relacionadas com a disponibilidade de SAP HANA em diferentes regiões do Azure. Devido a distância entre regiões do Azure, a configuração de disponibilidade de SAP HANA em várias regiões do Azure envolve considerações especiais.

## <a name="why-deploy-across-multiple-azure-regions"></a>Por que motivo implementar em várias regiões do Azure

Regiões do Azure estão frequentemente separadas pelas distâncias grandes. Consoante a região geopolítica, a distância entre regiões do Azure pode ser centenas de quilómetros, ou até vários milhares quilómetros, tal como nos Estados Unidos. Devido a distância, o tráfego de rede entre os recursos que são implementadas em diferentes regiões Azure dois experiência latência de ida e volta de rede significativo. A latência é significativa excluir intercâmbio de dados síncrona entre duas instâncias de SAP HANA em cargas de trabalho SAP típicas. 

Por outro lado, as organizações têm frequentemente um requisito de distância entre a localização do Centro de dados principal e um datacenter secundário. Um requisito de distância ajuda a fornecer disponibilidade se ocorrer um desastre natural numa localização geográfica mais amplo. Os exemplos incluem os furacões atingiu o Caribbean e na Florida em Setembro e Outubro de 2017. A sua organização poderá ter, pelo menos, um requisito de distância mínima. Para clientes de mais do Azure, requer uma definição de distância mínima de conceção de disponibilidade no [regiões do Azure](https://azure.microsoft.com/regions/). Porque é demasiado grande para utilizar o modo de replicação síncrona HANA a distância entre duas regiões do Azure, os requisitos de RTO e RPO poderão forçar implementar configurações de disponibilidade de uma região e, em seguida, complementar com implementações adicionais num segundo região.

Outro aspeto a ter em consideração neste cenário é a ativação pós-falha e redirecionar o cliente. Pressuposto é que uma ativação pós-falha entre instâncias de SAP HANA em dois diferentes regiões Azure sempre uma ativação pós-falha manual. Porque o modo de replicação da replicação do sistema de SAP HANA está definido como assíncrono, há a possibilidade de que os dados consolidados na primária instância HANA ainda não ainda ficam-lo para a instância HANA secundária. Por conseguinte, ativação pós-falha automática não é uma opção para configurações em que a replicação é assíncrona. Mesmo com ativação pós-falha controlada manualmente, como um exercício de ativação pós-falha, é necessário tomar medidas para se certificar de que todos os dados consolidados no lado primário efetuadas-lo para a instância secundária antes de manualmente mova sobre à região do Azure.
 
Rede Virtual do Azure utiliza um intervalo de endereços IP diferentes. Os endereços IP são implementados na segunda região do Azure. Por isso, a precisar de alterar a configuração de cliente de SAP HANA ou, de preferência, terá de criar os passos para alterar a resolução do nome. Desta forma, os clientes são redirecionados para o endereço IP do servidor do site secundário de novo. Para obter mais informações, consulte o artigo SAP [recuperação da ligação de cliente depois de aquisições](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html).   

## <a name="simple-availability-between-two-azure-regions"></a>Disponibilidade Simple entre duas regiões do Azure

Pode optar por não coloque qualquer configuração de disponibilidade no local numa única região, mas continuará a ter a procura para que a carga de trabalho servida se ocorrer um desastre. Cenários típicos para sistemas como esta são os sistemas de não produção. Embora seja sustentável com o sistema para baixo para metade um dia ou mesmo por dia, não é possível permitir que o sistema fique indisponível de 48 horas ou mais. Para tornar o programa de configuração menos dispendiosos, execute outro sistema que é mesmo menos importante na VM. O outro sistema funciona como um destino. Também pode tamanho da VM na região secundária para ser mais pequenos e optar por não pré-carregar os dados. Uma vez que a ativação pós-falha é manual e envolve muitos mais passos para a pilha de aplicação concluir a ativação pós-falha, o tempo adicional para encerre a VM, redimensioná-la e, em seguida, reiniciar a VM é aceitável.

> [!NOTE]
> Mesmo se não utilizar pré-carregamento de dados no destino de replicação do sistema HANA, precisa de, pelo menos, de 64 GB de memória. Também precisa de memória suficiente para além de 64 GB para manter os dados de rowstore na memória da instância de destino.

![Diagrama de duas VMs através de duas regiões](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> Nesta configuração, não é possível fornecer um RPO = 0, porque o modo de replicação do sistema HANA é assíncrono. Se tiver de fornecer um RPO = 0, esta configuração não está a configuração de eleição.

Poderá ser uma alteração de pequenas que pode efetuar na configuração configurar dados como o pré-carregamento da. No entanto, tendo em conta a natureza manual de ativação pós-falha e o facto das camadas da aplicação também tem de mover para a região segundo, poderá não fazer sentido pré-carregar dados. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Combinar disponibilidade dentro de uma região e em regiões 

Uma combinação de disponibilidade dentro e entre regiões poderá controlada por estes fatores:

- Um requisito de RPO = 0 dentro de uma região do Azure.
- A organização não se encontra disposto ou pode ter operações global afetadas por um catastrophe natural principais que afeta uma região maior. Isto foi o caso de algumas furacões que atingiu o Caribbean ao longo dos últimos anos alguns.
- Normas que as distâncias da pedido entre os sites primários e secundários, que são claramente se para além dos que disponibilidade do Azure podem fornecer zonas.

Nestes casos, pode configurar as chamadas SAP um [configuração de replicação do sistema com várias camadas de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) utilizando a replicação do sistema HANA. A arquitetura seria ter este aspeto:

![Diagrama de três VMs através de duas regiões](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Esta configuração fornece um RPO = 0, com baixa RTO, na região primária. A configuração também fornece decent RPO se está envolvida uma mudança para a região segundo. Os tempos RTO na região segundo são dependentes se os dados é pré-carregado. Muitos clientes utilizam VM na região secundária para executar um sistema de teste. No que utilizar as maiúsculas e minúsculas, os dados não podem ser pré-carregada.

> [!NOTE]
> Uma vez que está a utilizar **logreplay** modo de operação de replicação do sistema HANA que vai da camada 1 à camada 2 (replicação síncrona numa região primária), a replicação entre a camada 2 e camada 3 (replicação para o site secundário) não pode estar em **delta_datashipping** modo de funcionamento. Para obter detalhes sobre algumas restrições e modos de operação, consulte o artigo SAP [modos de operação de replicação do sistema de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Passos Seguintes

Para obter orientações passo a passo sobre como configurar estas configurações no Azure, consulte:

- [Configurar a replicação do sistema de SAP HANA em VMs do Azure](sap-hana-high-availability.md)
- [Elevada disponibilidade para SAP HANA utilizando a replicação do sistema](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
