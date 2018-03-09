---
title: "SAP HANA disponibilidade em regiões do Azure | Microsoft Docs"
description: "Descreve a descrição geral das considerações de disponibilidade ao executar SPA HANA em VMs do Azure"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 052394884f85d527caa88ff6c9464af669f47bb5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-across-azure-regions"></a>SAP HANA disponibilidade em regiões do Azure
Neste artigo cenários em torno de disponibilidade de SAP HANA em diferentes regiões do Azure são descritos e abordados. Tendo em conta o facto de que regiões do Azure separadas têm maior distância entre eles, existem considerações especiais que são apresentadas neste artigo.

## <a name="motivation-to-deploy-across-multiple-azure-regions"></a>Motivação para implementar em várias regiões do Azure
Diferentes regiões do Azure são separadas por uma distância maior. Depende de região geopolítica, isto pode ser centenas de quilómetros ou mesmo várias milhares quilómetros, tal como nos Estados Unidos. Devido a distância entre as diferentes regiões do Azure, de rede tráfego entre os recursos implementados na latência de ida e volta diferentes regiões Azure duas experiências de rede significativo. Significativas suficiente excluir intercâmbio de dados síncrona entre duas instâncias de SAP HANA sob carga de trabalho SAP normal. No outro lado, é, muitas vezes, é confrontadas com o facto de que não há requisitos definidos na distância entre o seu datacenter primário e um datacenter secundário para fornecer disponibilidade em caso de desastre natural atingir uma vasta área. Por exemplo, Furacões atingiu a área Caribbean e na Florida em Setembro e Outubro de 2017. Ou, pelo menos, um requisito de distância mínima. Na maioria dos cenários de cliente, esta definição de distância mínima requer a estruturar para disponibilidade no [regiões do Azure](https://azure.microsoft.com/regions/). Uma vez que a distância é demasiado grande entre duas regiões do Azure para utilizar o modo de replicação síncrona do HANA, os requisitos de RTO e RPO poderão exigir que para implementar as configurações de disponibilidade dentro de uma região e, em seguida, complementar com implementações adicionais num segundo região.

Outro aspeto ser considerados nestas configurações é a ativação pós-falha e o cliente de redirecionamento. Pressuposto é que uma ativação pós-falha entre instâncias de SAP HANA em dois diferentes regiões Azure sempre uma ativação pós-falha manual. Uma vez que o modo de replicação de replicação do sistema de SAP HANA está definido como assíncrono, há um potencial que dados consolidados na primária instância HANA não fez com que ainda para a instância HANA secundária. Por conseguinte, não é possível aceitar ativação pós-falha automática para configurações em que a replicação é assíncrona. Mesmo com manual controlado ativação pós-falha, como um exercício de ativação pós-falha, é necessário tomar medidas para se certificar de que todos os dados consolidados no lado primário efetuada-lo para a instância secundária antes de mover manualmente através à região do Azure.
 
Uma vez que funcionam com um intervalo de endereços IP diferentes nas VNets do Azure, que são implementados na região do Azure segundo, um tem de ser alterado na configuração de clientes de SAP HANA ou forma melhor tem de colocar os passos no local para alterar a resolução do nome. Por isso, secundário que os clientes estão a obter redirecionados para o novo do endereço IP do servidor. O artigo SAP [recuperação da ligação de cliente depois de aquisições](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html) entrar em obter mais detalhes.   

## <a name="simple-availability-between-two-azure-regions"></a>Disponibilidade Simple entre duas regiões do Azure
Neste cenário, decidiu não coloque qualquer configuração de disponibilidade no local numa única região. Mas ter a procura para que a carga de trabalho servida em caso de desastre. Cenários típicos para sistemas como o que são os sistemas de não produção. Embora possa suportar para que o sistema para baixo para metade um dia ou mesmo por dia, não é possível permitir que o sistema não esteja disponível de 48 horas ou mais. Para efetuar o programa de configuração menos dispendiosos, executar outro sistema que é o mesmo menos importante período VM que funciona como um destino ou tamanho da VM na região secundária mais pequeno e optar por não pré-carregar os dados. Uma vez que a ativação pós-falha vai ser manual e envolve muitos mais passos para ativação pós-falha, bem como a pilha de aplicação completa, o tempo adicional para colocar para baixo a VM, redimensioná-la e iniciar novamente a VM é aceitável.

> [!NOTE]
> Mesmo sem dados pré-carregar no destino de replicação do sistema HANA, precisa de, pelo menos, 64 GB de memória e esse suficiente memória para manter os dados de rowstore na memória da instância de destino.

![Duas VMs através de duas regiões](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> Nesta configuração, não é possível fornecer um RPO = 0, uma vez que o modo de replicação do sistema HANA é assíncrono. Se tiver de fornecer um RPO = 0, esta configuração não é a configuração de eleição.

Pode ser uma alteração de pequenos na configuração para configurar a pré-carregamento de dados. No entanto fornecido a natureza manual de ativação pós-falha e o facto de que precisam de camadas de aplicação para mover para a região segundo bem, não poderá fazer sentido para a pré-carregar dados. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Combinar disponibilidade dentro de uma região e em regiões 
Uma combinação de disponibilidade dentro e entre regiões pode ser controlada por:

- Requisito de RPO = 0 dentro de uma região do Azure.
- Não pretendo ou pode ter operações global da empresa sejam afetados por um catastrophe natural principais que afeta a uma região maior. Tal como estava as maiúsculas e minúsculas por alguns furacões que atingiu o Caribbean ao longo dos últimos anos alguns.
- Podem fornecer regulamentos que as distâncias da pedido entre o site primário e secundário que ultrapassam claramente que zonas de disponibilidade do Azure.

 
Nestes casos, teria de configurar as chamadas SAP um [configuração de replicação de sistema do SAP HANA Multitier](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) com replicação do sistema HANA. A arquitetura deverá ter o seguinte aspeto:

![três VMs através de duas regiões](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Esta configuração fornece um RPO = 0 com pequenas vezes RTO na região primária e fornece adicionalmente descend RPO em caso de uma mudança através da região segundo. Os tempos RTO na região segundo são dependentes se configurar pré-carregar de dados ou não. Em muitos casos de cliente, a VM com a região secundária é utilizada para executar um sistema de teste. Como resultado de que a utilização de dados não podem ser previamente carregados.

> [!NOTE]
> Uma vez que está a utilizar o modo de operação de logreplay para replicação do sistema HANA que vai da camada 1 para a camada 2 (replicação síncrona numa região primária), a replicação entre a camada 2 e 3 camada (replicação do site secundário) não pode estar em delta_datashipping operação modo. Detalhes de algumas restrições e modos de operação estão documentados pelo SAP no [modos de operação de replicação do sistema de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Passos Seguintes
Se necessitar de orientação passo a passo sobre como configurar este tipo de uma configuração no Azure, leia os artigos:

- [Configurar a replicação do sistema de SAP HANA em VMs do Azure](sap-hana-high-availability.md)
- [O SAP no Azure – parte 4 – elevada disponibilidade para SAP HANA utilizando a replicação do sistema](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
