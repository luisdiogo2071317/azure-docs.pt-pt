---
title: Personalizar as definições de avaliação do Azure Migrate | Documentos da Microsoft
description: Descreve como configurar e executar uma avaliação de migração de VMs de VMware para Azure utilizando o planeador de migração do Azure
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/30/2018
ms.author: raynew
ms.openlocfilehash: d0cfab51b686b5b6eb9617d4424ac3f834de8d6f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241077"
---
# <a name="customize-an-assessment"></a>Personalizar uma avaliação

[O Azure Migrate](migrate-overview.md) cria avaliações com propriedades predefinidas. Depois de criar uma avaliação, pode modificar as propriedades de padrão com as instruções neste artigo.


## <a name="edit-assessment-properties"></a>Editar propriedades de avaliação

1. Na página **Avaliações** página do projeto de migração, selecione a avaliação e clique em **Editar propriedades**.
2. Personalize as propriedades de avaliação com base nos detalhes abaixo:

    **Definição** | **Detalhes** | **Predefinição**
    --- | --- | ---
    **Localização de destino** | A localização do Azure para a qual pretende migrar.<br/><br/> Atualmente, o Azure Migrate suporta 30 regiões, incluindo Alemanha Central, Ásia Oriental, Canadá Central, Coreia do Sul Central, EUA Central, EUA Centro-Norte, EUA Centro-Oeste, EUA Centro-Sul, EUA Leste, EUA Leste 2, EUA Oeste, EUA Oeste 2, Europa do Norte, Europa Ocidental, Índia Central, Oeste da Índia, Leste da Austrália, Leste da China, Leste do Canadá, Leste do Japão, Nordeste da Alemanha, Norte da China, Oeste do Japão, Oeste do Reino Unido, Sudeste Asiático, Sudeste da Austrália, Sul da Coreia do Sul, Sul da Índia, Sul BR, Sul do Reino Unido, US Gov – Arizona, US Gov – Texas e US Gov – Virginia. |  E.U.A. oeste 2 é a localização predefinida.
    **Escalão de preço** | Pode especificar o [escalão de preço (Básico/Standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, gostaria de considerar o escalão Standard. Por outro lado, se tiver um ambiente de Dev/Test, deve considerar o escalão Básico que tem VMs com latência superior e custos reduzidos. | Por predefinição, é utilizado o escalão [Standard](../virtual-machines/windows/sizes-general.md).
    **Tipo de armazenamento** | Pode utilizar esta propriedade para especificar o tipo de discos que pretende alocar no Azure. Para como-no local de dimensionamento, pode especificar o tipo de disco de destino como discos geridos Premium ou discos geridos Standard. Para o dimensionamento baseado no desempenho, pode especificar o tipo de disco de destino como automática de discos geridos Premium ou discos geridos Standard. Quando especificar o tipo de armazenamento como automático, a recomendação de disco é feita com base nos dados de desempenho de discos (IOPS e débito). Por exemplo, se quer atingir um [única instância de VM SLA de 99,9%](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), talvez queira especificar o tipo de armazenamento como discos geridos Premium, que irão garantir que todos os discos na avaliação irão ser recomendados como discos geridos Premium. Note que o Azure Migrate só suporta discos geridos para avaliação de migrações. | O valor predefinido é de discos geridos Premium (com o critério de dimensionamento como *no local dimensionamento*).
    **Instâncias Reservadas** |  Também pode especificar se tem [instâncias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) no Azure e o Azure Migrate fará uma estimativa dos custos em conformidade. Instâncias reservadas não se aplicam ao regiões de soberania (Azure Government, Alemanha e China) e são aplicáveis apenas a oferta pay as you go no Azure Migrate. | O valor predefinido para esta propriedade é instâncias de 3 anos reservados.
    **Critérios de dimensionamento** | Os critérios a serem utilizados pelo Azure Migrate para encontrar VMs de tamanho adequado para o Azure. Pode dimensionar *com base no desempenho* ou dimensionar as VMs *como no local*, sem ter em conta o histórico do desempenho. | O dimensionamento baseado no desempenho é a opção predefinida.
    **Histórico de desempenho** | A duração a considerar para avaliar o desempenho das VMs. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*. | A predefinição é um dia.
    **Utilização de percentil** | O valor de percentil da amostra de desempenho definido para ser considerado para o dimensionamento certo. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*.  | A predefinição é o percentil 95.
    **Série das VMs** | Pode especificar a série das VMs que gostaria de considerar para redimensionamento. Por exemplo, se tiver um ambiente de produção que não pretende migrar para VMs da série A no Azure, poderá excluir a série A da lista ou da série e o redimensionamento será realizado apenas na série selecionada. | Por predefinição, são selecionadas todas as séries VM.
    **Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. | A predefinição é 1,3 x.
    **Oferta** | A [Oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) em que se inscreveu. | [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) é a predefinição.
    **Moeda** | A moeda de faturação. | Predefinição é não utilizados no compromisso E.U.A.
    **Desconto (%)** | Eventuais descontos para uma subscrição específica que receba sobre a oferta do Azure. | A predefinição é 0%.
    **Tempo de atividade de VM** | Se não se pretender que as VMs em execução 24x7 no Azure, pode especificar a duração (número de dias por mês) e o número de horas por dia para que eles estariam em execução e as estimativas de custos serão efetuadas em conformidade. | O valor predefinido é 31 dias por mês e 24 horas por dia.
    **Benefício Híbrido do Azure** | Especifique se tem software assurance e se é elegível para o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, são considerados para as VMs do Windows preços não Microsoft Azure. | A predefinição é Sim.

3. Clique em **guardar** para atualizar a avaliação.


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
