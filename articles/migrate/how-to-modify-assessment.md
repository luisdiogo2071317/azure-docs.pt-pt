---
title: "Personalizar as definições de avaliação do Azure migrar | Microsoft Docs"
description: "Descreve como configurar e executar uma avaliação para migrar VMs de VMware para o Azure utilizando o planeador de migração do Azure"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 02/26/2018
ms.author: raynew
ms.openlocfilehash: efb4ad59d25a0c1209e4f0f6cd406c2f0d48159c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="customize-an-assessment"></a>Personalizar uma avaliação

[Migrar do Azure](migrate-overview.md) cria avaliações com propriedades predefinidas. Depois de criar uma avaliação, pode modificar as propriedades predefinidas utilizando as instruções neste artigo.


## <a name="edit-assessment-properties"></a>Editar propriedades de avaliação

1. No **avaliações** página do projeto de migração, selecione a avaliação e clique em **editar propriedades**.
2. Modificar as propriedades de acordo com a tabela seguinte:

    **Definição** | **Detalhes** | **Predefinição**
    --- | --- | ---
    **Localização de destino** | A localização do Azure para a qual pretende migrar.<br/><br/> Migrar do Azure suporta atualmente a 30 regiões, incluindo leste da Austrália, Sudeste da Austrália, sul do Brasil, Canadá Central, Canadá leste, Índia Central, EUA Central, leste da China, Norte da China, Ásia Oriental, EUA leste, Datacenters Central, Nordeste da Alemanha, EUA Leste 2, Japão Leste, oeste do Japão, Coreia Central, Coreia Sul, EUA Centro-Norte, Europa do Norte, EUA Centro-Sul, Sudeste asiático, Sul da Índia, sul do RU, RU oeste, EUA Centro Oeste, Europa Ocidental, Índia Ocidental, EUA oeste e US2 oeste. |  E.u. a oeste 2 é a localização predefinida.
    **Redundância do armazenamento** | O tipo de redundância de armazenamento que irá utilizar as VMs do Azure após a migração. | [Armazenamento localmente redundante (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) é o valor predefinido. Migrar do Azure só suporta geridos com base em discos avaliações e discos geridos só suportam LRS, por conseguinte, a propriedade atualmente apenas tem a opção de LRS. 
    **Critério de dimensionamento** | Os critérios a serem utilizados pelo Azure Migrate para encontrar VMs de tamanho adequado para o Azure. Pode fazê-lo ou efetue *com base no desempenho* dimensionamento ou redimensionar VMs *como no local*, sem considerar o histórico de desempenho. | Com base no desempenho dimensionamento é a opção predefinida.
    **Histórico de desempenho** | A duração a considerar para avaliar o desempenho das VMs. Esta propriedade só é aplicável quando o critério de dimensionamento é *com base no desempenho dimensionamento*. | Predefinição é um dia.
    **Utilização de percentil** | O valor de percentil da amostra desempenho definido para ser considerado de dimensionar. Esta propriedade só é aplicável quando o critério de dimensionamento é *com base no desempenho dimensionamento*.  | Predefinição é percentil 95th.
    **Escalão de preço** | Pode especificar o [escalão de preço (Básico/Standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, deve considerar o escalão Standard, que oferece baixa latência às VMs, mas pode ser mais dispendioso. Por outro lado, se tiver um ambiente de Dev/Test, deve considerar o escalão Básico que tem VMs com latência superior e custos reduzidos. | Por predefinição, é utilizado o escalão [Standard](../virtual-machines/windows/sizes-general.md).
    **Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. | Definição predefinida é 1.3 x.
    **Oferta** | [Oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) inscritos para. | [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) é a predefinição.
    **Moeda** | Moeda de faturação. | Predefinição é não utilizados no compromisso E.U.A.
    **Desconto (%)** | Quaisquer descontos de subscrição específica recebe sobre a oferta do Azure. | A predefinição é 0%.
    **Benefício de híbridos do Azure** | Especifique se têm software assurance e são elegíveis para [Azure híbrida benefício](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, os preços de não - do Windows Azure são considerados para VMs do Windows. | A predefinição é Sim.

3. Clique em **guardar** para atualizar a avaliação.


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
