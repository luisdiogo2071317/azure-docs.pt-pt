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
ms.openlocfilehash: 9f95be8a9d75065deedd3bd7c92907145e966494
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913059"
---
# <a name="scaling-media-processing"></a>Dimensionar processamento de multimédia

Serviços de multimédia do Azure permite-lhe dimensionar o processamento de multimédia na sua conta através da gestão de unidades reservadas de multimédia (MRUs). Para uma visão geral detalhada, consulte [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

Este artigo mostra como usar [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) Dimensionar MRUs.

> [!NOTE]
> Para a análise de áudio e tarefas de análise de vídeo que são acionados por serviços de multimédia v3 ou Video Indexer, recomenda-se elevada para aprovisionar a sua conta com 10 MRUs de S3. <br/>Se precisar de mais de 10 S3 MRUs, abra um pedido de suporte através do [portal do Azure](https://portal.azure.com/).

## <a name="prerequisites"></a>Pré-requisitos 

+ Instale a [CLI do Azure]( /cli/azure/install-azure-cli). Este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. 

    Pode também utilizar o [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).
+ [Criar uma conta de Media Services](create-account-cli-how-to.md).

## <a name="scale-media-reserved-units-with-cli"></a>Unidades com a CLI reservadas de multimédia de dimensionamento

1. Execute o comando `login`. Execute este comando, se estiver a utilizar o Azure cloud shell ou a shell CLI local.

    ```azurecli
    az login
    ```
    
    Se a CLI conseguir abrir o seu browser predefinido, executa essa ação e carrega uma página de início de sessão. Caso contrário, terá de abrir uma página do browser e siga as instruções na linha de comandos para introduzir um código de autorização depois de navegar para [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) no seu browser.
2. Execute o comando `mru`.

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
