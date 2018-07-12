---
title: Aceder à API de serviços de multimédia do Azure - CLI do Azure | Documentos da Microsoft
description: Siga os passos nesta explicação de procedimento para acessar a API de serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: e20cac5f1063589bdbfee0f384ac6af5a39811ed
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724030"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Serviços de multimédia do Azure de acesso à API com a CLI do Azure
 
Deve usar a autenticação do principal de serviço do Azure AD para ligar à API de serviços de multimédia do Azure. Seu aplicativo precisa para pedir um token do Azure AD que tenha os seguintes parâmetros:

* Ponto final de inquilino do Azure AD
* URI do recurso de serviços de multimédia
* URI para serviços de multimédia REST do recurso
* Valores de aplicações do Azure AD: o ID de cliente e o segredo de cliente

Este artigo mostra-lhe como utilizar a CLI do Azure para criar uma aplicação do Azure AD e o serviço principal e obter os valores que são necessárias para aceder aos recursos de serviços de multimédia do Azure.

## <a name="prerequisites"></a>Pré-requisitos 

Criar uma nova conta de Media Services do Azure, conforme descrito em [este guia de introdução](create-account-cli-quickstart.md).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com) e inicie o **CloudShell** para executar os comandos da CLI, conforme mostrado nos passos seguintes.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI do Azure](/cli/azure/install-azure-cli). 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
