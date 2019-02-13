---
title: Criar uma conta de serviços de multimédia com a CLI do Azure - Azure | Documentos da Microsoft
description: Siga os passos deste início rápido para criar uma conta dos Serviços de Multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: ce022e22cc98dd218dff4840f686464233f99c72
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56176355"
---
# <a name="create-an-azure-media-services-account"></a>Criar uma conta dos Azure Media Services

Para começar a encriptar, codificação, analisar, gerir e transmissão em fluxo conteúdo de multimédia do Azure, terá de criar uma conta de Media Services. No momento, criar uma conta de Media Services, também é criar um armazenamento associado da conta (ou utilize um já existente).  

> [!NOTE]
> A conta de Media Services e todas as contas de armazenamento associado tem de ser na mesma subscrição do Azure. É vivamente recomendado para utilizar contas de armazenamento na mesma localização que a conta de serviços de multimédia para evitar custos de saída de latência e dados adicionais.

Este artigo descreve os passos para criar uma nova conta de Media Services do Azure com a CLI do Azure.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Uma subscrição ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="set-the-azure-subscription"></a>Definir a subscrição do Azure

No comando seguinte, forneça o ID da subscrição do Azure que quer utilizar na conta dos Serviços de Multimédia. Pode ver uma lista de subscrições a que tem acesso ao navegar até [Subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

