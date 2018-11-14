---
title: Criar uma conta de Media Services do Azure com a CLI do Azure | Documentos da Microsoft
description: Siga os passos deste início rápido para criar uma conta dos Serviços de Multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: cb49b642137517c0ceef7d2fa01994a554db1f4e
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613164"
---
# <a name="create-an-azure-media-services-account"></a>Criar uma conta dos Azure Media Services

Para começar a encriptar, codificação, analisar, gerir e transmissão em fluxo conteúdo de multimédia do Azure, terá de criar uma conta de Media Services. No momento, criar uma conta de Media Services, também é criar um armazenamento associado da conta (ou utilize um já existente) na mesma região geográfica que a conta de Media Services.

Este artigo descreve os passos para criar uma nova conta de Media Services do Azure com a CLI do Azure.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

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

