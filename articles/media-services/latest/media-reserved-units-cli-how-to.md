---
title: Utilizar a CLI para dimensionar unidades reservadas de multimédia - Azure | Documentos da Microsoft
description: Este tópico mostra como utilizar a CLI para dimensionar processamento de multimédia com serviços de multimédia do Azure.
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
ms.date: 01/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b40ab6bcc2f718eda85ff64d69a6689e12d60ab8
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55094840"
---
# <a name="scaling-media-processing"></a>Dimensionar processamento de multimédia

Serviços de multimédia do Azure permite-lhe dimensionar o processamento de multimédia na sua conta através da gestão de unidades reservadas de multimédia (MRUs). Para uma visão geral detalhada, consulte [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

Este artigo mostra como usar [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) Dimensionar MRUs.

> [!NOTE]
> Para a análise de áudio e tarefas de análise de vídeo que são acionados por serviços de multimédia v3 ou Video Indexer, recomenda-se elevada para aprovisionar a sua conta com 10 MRUs de S3. <br/>Se precisar de mais de 10 S3 MRUs, abra um pedido de suporte através do [portal do Azure](https://portal.azure.com/).

## <a name="prerequisites"></a>Pré-requisitos 

[Criar uma conta de Media Services](create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>Unidades com a CLI reservadas de multimédia de dimensionamento

Execute o comando `mru`.

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
