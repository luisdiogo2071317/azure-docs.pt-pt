---
title: "Personalizar as definições de avaliação do Azure migrar | Microsoft Docs"
description: "Descreve como configurar e executar uma avaliação para migrar VMs de VMware para o Azure utilizando o planeador de migração do Azure"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: ce47790f6214864afdba33eb5cbe3a9e49b81cd5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="customize-an-assessment"></a>Personalizar uma avaliação

[Migrar do Azure](migrate-overview.md) cria avaliações com as predefinições. Depois de criar uma avaliação, pode modificar as definições predefinidas utilizando as instruções neste artigo.


## <a name="edit-assessment-values"></a>Editar os valores de avaliação

1. No projeto do Azure migrar **avaliações** página, selecione a avaliação e clique em **editar propriedades**.
2. Modificar as definições de acordo com a tabela abaixo.

    **Definição** | **Detalhes** | **Predefinição**
    --- | --- | ---
    **Localização de destino** | A localização do Azure para a qual pretende migrar. |  E.u. a oeste 2 é a localização predefinida.
    **Redundância do armazenamento** | O tipo de armazenamento que as VMs do Azure vão utilizar após a migração. | Apenas [armazenamento localmente redundante (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replicação é atualmente suportada.
    **Fator de conforto** | Fator de comfort é uma memória intermédia que é utilizada durante a avaliação. Utilizá-la para uma conta para coisas como utilização sazonais, curto histórico de desempenho, provavelmente no futuro aumento da utilização. | Definição predefinida é 1.3 x.
    **Histórico de Perfomance** | Tempo utilizado ao avaliar o histórico de desempenho. | Predefinição é de um mês.
    **Utilização de percentil** | Valor de percentil a considerar para perfomance histórico. | Predefinição é 95%.
    **Escalão de preço** | Pode especificar o [escalão de preço](https://azure.microsoft.com/blog/basic-tier-virtual-machines-2/) para uma VM.  | Por predefinição o [padrão](../virtual-machines/windows/sizes-general.md) camada é utilizada.
    **Oferta** | [Ofertas do Azure](https://azure.microsoft.com/support/legal/offer-details/) que se aplicam. | [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) é a predefinição.
    **Moeda** | Moeda de faturação. | Predefinição é não utilizados no compromisso E.U.A.
    **Desconto (%)** | Quaisquer descontos de subscrição específica receberá por cima de qualquer oferta. | A predefinição é 0%.
    **Benefício de utilização de híbridos do Azure** | Indica se o utilizador estiver inscrito no [benefício de utilização do Azure híbrida](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, os preços de não - do Windows Azure são considerar para VMs do Windows. | A predefinição é Sim.

3. Clique em **guardar** para atualizar a avaliação.


## <a name="next-steps"></a>Passos seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como avaliações são calculadas.
