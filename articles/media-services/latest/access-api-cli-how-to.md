---
title: Aceder à API de serviços de multimédia do Azure - CLI do Azure | Documentos da Microsoft
description: Siga os passos nesta explicação de procedimento para acessar a API de serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 54a92e56df21b59430ed12f191a9cf7a918e14c9
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612904"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Serviços de multimédia do Azure de acesso à API com a CLI do Azure
 
Deve usar a autenticação do principal de serviço do Azure AD para ligar à API de serviços de multimédia do Azure. Seu aplicativo precisa para pedir um token do Azure AD que tenha os seguintes parâmetros:

* Ponto final de inquilino do Azure AD
* URI do recurso de serviços de multimédia
* URI para serviços de multimédia REST do recurso
* Valores de aplicações do Azure AD: o ID de cliente e o segredo de cliente

Este artigo mostra-lhe como utilizar a CLI do Azure para criar uma aplicação do Azure AD e o serviço principal e obter os valores que são necessárias para aceder aos recursos de serviços de multimédia do Azure.

## <a name="prerequisites"></a>Pré-requisitos 

- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

- [Criar uma conta de Media Services](create-account-cli-how-to.md).

    Lembre-se de que não se esqueça dos valores que utilizou para o nome do grupo de recursos e o nome de conta de serviços de multimédia.

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
