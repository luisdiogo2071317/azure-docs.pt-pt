---
title: Início rápido - criar um registo privado do Docker no Azure - PowerShell
description: Aprenda rapidamente a criar um registo de contentor do Docker no Azure com o PowerShell.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b8ff8e671d51a148177e66b30225dd7536a48028
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299748"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Início rápido: Criar um registo de contentor privado com o Azure PowerShell

O Azure Container Registry é um serviço de registo de contentor do Docker privado e gerido que serve para criar, armazenar e fornecer imagens de contentor do Docker. Neste guia de início rápido, irá aprender a criar um registo de contentor do Azure com o PowerShell. Em seguida, utilize os comandos do Docker para enviar por push uma imagem de contentor para o registro e finalmente extrair e executar a imagem a partir do registo.

## <a name="prerequisites"></a>Pré-requisitos

Este guia de início rápido requer a versão 5.7.0 ou posterior do módulo Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para determinar a versão instalada. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/azurerm/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

Também tem de ter o Docker instalado localmente. O Docker fornece pacotes para os sistemas [macOS][docker-mac], [Windows][docker-windows] e [Linux][docker-linux].

Uma vez que o Azure Cloud Shell não inclui todos os componentes do Docker necessários (o daemon `dockerd`), não é possível utilizar o Cloud Shell para este guia de início rápido.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando [Connect-AzureRmAccount][Connect-AzureRmAccount] e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Depois de efetuar a autenticação no Azure, crie um grupo de recursos com [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Criar registo de contentor

Em seguida, crie um registo de contentor no novo grupo de recursos com o comando [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry].

O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. O exemplo seguinte cria um registo com o nome "myContainerRegistry007". Substitua *myContainerRegistry007* no comando seguinte e execute-o para criar o registo:

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

Neste início rápido de criar uma *básica* registo, o que é uma opção com otimização de custos para os desenvolvedores a aprender sobre o Azure Container Registry. Para obter detalhes sobre os escalões de serviço disponíveis, consulte [SKUs do registo de contentor][container-registry-skus].

## <a name="log-in-to-registry"></a>Iniciar sessão no registo

Antes de enviar e solicitar imagens de contentor, tem de iniciar sessão no registo. Em cenários de produção deve usar uma identidade individual ou um principal de serviço para acesso ao registo de contentor, mas para manter este início rápido em breve, permitir que o utilizador de administrador no seu registo com o [Get-AzureRmContainerRegistryCredential] [ Get-AzureRmContainerRegistryCredential] comando:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Em seguida, execute o [docker login][docker-login] para iniciar sessão:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

O comando devolve `Login Succeeded` depois de estar concluído.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Depois de concluir a trabalhar com os recursos que criou neste início rápido, utilize o [Remove-AzureRmResourceGroup] [ Remove-AzureRmResourceGroup] comando para remover o grupo de recursos, o registo de contentor e o contentor imagens armazenadas:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou um Azure Container Registry com o Azure PowerShell, enviou uma imagem de contentor e extraídos e executou a imagem do registo. Avance para os tutoriais de registo de contentor do Azure para uma análise mais profunda ACR.

> [!div class="nextstepaction"]
> [Tutoriais de registo de contentor do Azure][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md