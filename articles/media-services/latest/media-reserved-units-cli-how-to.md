---
title: Dimensionar o suporte de dados reservados unidades - Azure | Documentos da Microsoft
description: Este tópico é uma visão geral do dimensionamento processamento de multimédia com os serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: dd587e5fc2082d1e496fbc05d5b25cf6692413bc
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713066"
---
# <a name="scaling-media-processing"></a>Dimensionar processamento de multimédia

Serviços de multimédia do Azure permite-lhe dimensionar o processamento de multimédia na sua conta através da gestão de unidades reservadas de multimédia (MRUs). Para uma visão geral detalhada, consulte [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

Este artigo mostra como usar [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) Dimensionar MRUs.

> [!NOTE]
> Para a análise de áudio e tarefas de análise de vídeo que são acionados por serviços de multimédia v3 ou Video Indexer, recomenda-se elevada para aprovisionar a sua conta com 10 MRUs de S3. <br/>Se precisar de mais de 10 S3 MRUs, abra um pedido de suporte através do [portal do Azure](https://portal.azure.com/).

## <a name="prerequisites"></a>Pré-requisitos 

- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

- [Criar uma conta de Media Services](create-account-cli-how-to.md).

## <a name="scale-media-reserved-units-with-cli"></a>Unidades com a CLI reservadas de multimédia de dimensionamento

O seguinte procedimento [mru de contas do az ams](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) comando conjuntos de unidades reservadas de multimédia "amsaccount" conta utilizando o **contagem** e **tipo** parâmetros.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Faturação

Cobrado consoante o número, o tipo e o período de tempo que MRUs são aprovisionados na sua conta. Aplicam-se custos se ou não executar quaisquer tarefas. Para obter uma explicação detalhada, consulte a secção de FAQ do [preços de serviços de multimédia](https://azure.microsoft.com/pricing/details/media-services/) página.   

## <a name="next-step"></a>Passo seguinte

[Analisar vídeos](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
