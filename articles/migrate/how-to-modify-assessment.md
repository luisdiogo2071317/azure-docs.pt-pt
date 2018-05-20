---
title: Personalizar as definições de avaliação do Azure migrar | Microsoft Docs
description: Descreve como configurar e executar uma avaliação para migrar VMs de VMware para o Azure utilizando o planeador de migração do Azure
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: c826453dcbcaf2facfd58daa05b77decda7ae456
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="customize-an-assessment"></a>Personalizar uma avaliação

[Migrar do Azure](migrate-overview.md) cria avaliações com propriedades predefinidas. Depois de criar uma avaliação, pode modificar as propriedades predefinidas utilizando as instruções neste artigo.


## <a name="edit-assessment-properties"></a>Editar propriedades de avaliação

1. Na página **Avaliações** página do projeto de migração, selecione a avaliação e clique em **Editar propriedades**.
2. Modifique as propriedades de acordo com a tabela abaixo:

    **Definição** | **Detalhes** | **Predefinição**
    --- | --- | ---
    **Localização de destino** | A localização do Azure para a qual pretende migrar.<br/><br/> Migrar do Azure suporta atualmente a 30 regiões, incluindo leste da Austrália, Sudeste da Austrália, sul do Brasil, Canadá Central, Canadá leste, Índia Central, EUA Central, leste da China, Norte da China, Ásia Oriental, EUA leste, Datacenters Central, Nordeste da Alemanha, EUA Leste 2, Japão Leste, oeste do Japão, Coreia Central, Coreia Sul, EUA Centro-Norte, Europa do Norte, EUA Centro-Sul, Sudeste asiático, Sul da Índia, sul do RU, RU oeste, E.U.A. us Arizona, GOV us Texas, GOV us Virginia, EUA Centro Oeste, Europa Ocidental, Índia Ocidental, EUA oeste e US2 oeste. |  E.u. a oeste 2 é a localização predefinida.
    **Redundância do armazenamento** | O tipo de redundância de armazenamento que as VMs do Azure vão utilizar após a migração. | O valor predefinido é [Armazenamento Localmente Redundante (LRS)](../storage/common/storage-redundancy-lrs.md). Migrar do Azure só suporta geridos com base em discos avaliações e discos geridos só suportam LRS, por conseguinte, a propriedade atualmente apenas tem a opção de LRS.
    **Critérios de dimensionamento** | Os critérios a serem utilizados pelo Azure Migrate para encontrar VMs de tamanho adequado para o Azure. Pode dimensionar *com base no desempenho* ou dimensionar as VMs *como no local*, sem ter em conta o histórico do desempenho. | O dimensionamento baseado no desempenho é a opção predefinida.
    **Histórico de desempenho** | A duração a considerar para avaliar o desempenho das VMs. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*. | A predefinição é um dia.
    **Utilização de percentil** | O valor de percentil da amostra de desempenho definido para ser considerado para o dimensionamento certo. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*.  | A predefinição é o percentil 95.
    **Série VM** | Pode especificar a série VM que gostaria de considerar para dimensionar. Por exemplo, se tiver um ambiente de produção que não pretende migrar para A série de VMs no Azure, pode excluir série A partir da lista ou série e o dimensionamento do lado direito serão efetuadas apenas na série de selecionado. | Por predefinição, estão selecionadas todas as séries VM.
    **Escalão de preço** | Pode especificar o [escalão de preço (Básico/Standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, deve considerar o escalão Standard, que oferece baixa latência às VMs, mas pode ser mais dispendioso. Por outro lado, se tiver um ambiente de Dev/Test, deve considerar o escalão Básico que tem VMs com latência superior e custos reduzidos. | Por predefinição, é utilizado o escalão [Standard](../virtual-machines/windows/sizes-general.md).
    **Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. | A predefinição é 1,3 x.
    **Oferta** | A [Oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) em que se inscreveu. | [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) é a predefinição.
    **Moeda** | A moeda de faturação. | Predefinição é não utilizados no compromisso E.U.A.
    **Desconto (%)** | Eventuais descontos para uma subscrição específica que receba sobre a oferta do Azure. | A predefinição é 0%.
    **Benefício Híbrido do Azure** | Especifique se tem software assurance e se é elegível para o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, são considerados para as VMs do Windows preços não Microsoft Azure. | A predefinição é Sim.

3. Clique em **guardar** para atualizar a avaliação.


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
