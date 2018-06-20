---
title: Personalizar as definições de avaliação do Azure migrar | Microsoft Docs
description: Descreve como configurar e executar uma avaliação para migrar VMs de VMware para o Azure utilizando o planeador de migração do Azure
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 4e7decc34105b02be51f002e1951145759a9f46e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231530"
---
# <a name="customize-an-assessment"></a>Personalizar uma avaliação

[Migrar do Azure](migrate-overview.md) cria avaliações com propriedades predefinidas. Depois de criar uma avaliação, pode modificar as propriedades predefinidas utilizando as instruções neste artigo.


## <a name="edit-assessment-properties"></a>Editar propriedades de avaliação

1. Na página **Avaliações** página do projeto de migração, selecione a avaliação e clique em **Editar propriedades**.
2. Modifique as propriedades de acordo com a tabela abaixo:

    **Definição** | **Detalhes** | **Predefinição**
    --- | --- | ---
    **Localização de destino** | A localização do Azure para a qual pretende migrar.<br/><br/> Atualmente, o Azure Migrate suporta 30 regiões, incluindo Alemanha Central, Ásia Oriental, Canadá Central, Coreia Central, EUA Central, EUA Centro-Norte, EUA Centro-Oeste, EUA Centro-Sul, EUA Leste, EUA Leste 2, EUA Oeste, EUA Oeste 2, Europa do Norte, Europa Ocidental, Índia Central, Índia Ocidental, Leste da Austrália, Leste da China, Leste do Canadá, Leste do Japão, Nordeste da Alemanha, Norte da China, Oeste do Japão, Oeste do Reino Unido, Sudeste Asiático, Sudeste da Austrália, Sul da Coreia, Sul da Índia, Sul do Brasil, Sul do Reino Unido, US Gov – Arizona, US Gov – Texas e US Gov – Virginia. |  E.u. a oeste 2 é a localização predefinida.
    **Tipo de armazenamento** | Pode especificar o tipo de discos que pretende alocar no Azure. Esta propriedade é aplicável quando o critério de dimensionamento é como no local dimensionamento. Pode especificar o tipo de disco de destino como Premium gerido discos ou Standard discos geridos pelo. Para dimensionamento com base no desempenho, a recomendação de disco é feita automaticamente com base nos dados de desempenho das VMs. Tenha em atenção que só migrar do Azure suporta discos geridos para avaliação de migração. | O valor predefinido é discos Premium gerido (critério de dimensionamento como *tal como no local dimensionamento*).
    **Critérios de dimensionamento** | Os critérios a serem utilizados pelo Azure Migrate para encontrar VMs de tamanho adequado para o Azure. Pode dimensionar *com base no desempenho* ou dimensionar as VMs *como no local*, sem ter em conta o histórico do desempenho. | O dimensionamento baseado no desempenho é a opção predefinida.
    **Histórico de desempenho** | A duração a considerar para avaliar o desempenho das VMs. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*. | A predefinição é um dia.
    **Utilização de percentil** | O valor de percentil da amostra de desempenho definido para ser considerado para o dimensionamento certo. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*.  | A predefinição é o percentil 95.
    **Série das VMs** | Pode especificar a série das VMs que gostaria de considerar para redimensionamento. Por exemplo, se tiver um ambiente de produção que não pretende migrar para VMs da série A no Azure, poderá excluir a série A da lista ou da série e o redimensionamento será realizado apenas na série selecionada. | Por predefinição, estão selecionadas todas as séries VM.
    **Escalão de preço** | Pode especificar o [escalão de preço (Básico/Standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, deve considerar o escalão Standard, que oferece baixa latência às VMs, mas pode ser mais dispendioso. Por outro lado, se tiver um ambiente de Dev/Test, deve considerar o escalão Básico que tem VMs com latência superior e custos reduzidos. | Por predefinição, é utilizado o escalão [Standard](../virtual-machines/windows/sizes-general.md).
    **Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. | A predefinição é 1,3 x.
    **Oferta** | A [Oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) em que se inscreveu. | [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) é a predefinição.
    **Moeda** | A moeda de faturação. | Predefinição é não utilizados no compromisso E.U.A.
    **Desconto (%)** | Eventuais descontos para uma subscrição específica que receba sobre a oferta do Azure. | A predefinição é 0%.
    **Benefício Híbrido do Azure** | Especifique se tem software assurance e se é elegível para o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, são considerados para as VMs do Windows preços não Microsoft Azure. | A predefinição é Sim.

3. Clique em **guardar** para atualizar a avaliação.


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
