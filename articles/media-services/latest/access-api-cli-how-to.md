---
title: Aceder a API de serviços de multimédia do Azure - CLI 2.0 | Microsoft Docs
description: Siga os passos para este procedimentos para aceder à API de serviços de suporte de dados do Azure.
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
ms.openlocfilehash: 9295c3f9dfabc8ef7749758e926df443843720a1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639780"
---
# <a name="access-azure-media-services-api-with-cli-20"></a>Acesso Media Services do Azure API com a CLI 2.0
 
Deve utilizar a autenticação principal de serviço do Azure AD para ligar à API de serviços de suporte de dados do Azure. A aplicação tem de pedir um token do Azure AD que tenha os seguintes parâmetros:

* Ponto final de inquilino do Azure AD
* URI do recurso dos Media Services
* URI para serviços de suporte de dados REST do recurso
* Os valores de aplicação do Azure AD: o ID de cliente e o segredo do cliente

Este artigo mostra como utilizar a CLI 2.0 para criar uma aplicação do Azure AD e o serviço principal e obter os valores que são necessárias para aceder a recursos de Media Services do Azure.

## <a name="prerequisites"></a>Pré-requisitos 

Criar uma nova conta de Media Services do Azure, conforme descrito em [este guia de introdução](create-account-cli-quickstart.md).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com) e inicie o **CloudShell** para executar os comandos da CLI, conforme mostrado nos passos seguintes.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Consulte também

[CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
