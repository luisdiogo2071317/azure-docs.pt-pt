---
title: Implementar uma aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila que utiliza o volume de ficheiros do Azure | Documentos da Microsoft
description: Saiba como implementar uma aplicação que utiliza o volume de ficheiros do Azure para a malha de recursos de infraestrutura de serviço com a CLI do Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: fb9740efaa9a1033d6602180f5750f6fb550c048
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076251"
---
# <a name="deploy-a-service-fabric-mesh-application-that-uses-the-azure-files-volume"></a>Implementar uma aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila que utiliza o volume de ficheiros do Azure
Este exemplo ilustra a utilização de volumes de armazenamento num contentor em execução no Azure Service Fabric em malha. Como parte deste exemplo:

- Criar uma partilha de ficheiros com [ficheiros do Azure](/azure/storage/files/storage-files-introduction) 
- Referência que partilham como um volume para uma instância de contentor que iremos implementar
  - Quando o contentor é iniciado, monta a que a partilha como um local específico dentro do contentor
- O código em execução no interior do contentor escreve um arquivo de texto a essa localização
- Certifique-se de que o ficheiro é escrito corretamente na partilha que efetua uma cópia do volume

## <a name="example-json-templates"></a>Modelos JSON de exemplo

Linux: [https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json](https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json)

Windows: [https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json](https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json)

## <a name="create-the-azure-files-file-share"></a>Criar a partilha de ficheiros de ficheiros do Azure

Siga as instruções no [documentação de ficheiros do Azure](/azure/storage/files/storage-how-to-create-file-share) para criar uma partilha de ficheiros para a aplicação a utilizar.

## <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric de malha
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir estes passos. Se optar por instalar e utilizar a CLI localmente, tem de instalar a CLI do Azure versão 2.0.35 ou posterior. Executar `az --version` para localizar a versão. Para instalar ou atualizar para a versão mais recente da CLI, veja [instalar CLI 2.0 do Azure] [azure-cli-install].

Instale o módulo de extensão de CLI de malha do Azure Service Fabric. Para a pré-visualização, CLI do Azure Service Fabric de malha é escrita como uma extensão da CLI do Azure.

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Azure e definir a subscrição.

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-resource-group"></a>Criar grupo de recursos
Criar um grupo de recursos (RG) para implementar neste exemplo, ou pode utilizar um grupo de recursos existente e ignorar este passo. A pré-visualização está disponível apenas nas `eastus` localização.

```azurecli-interactive
az group create --name <resourceGroupName> --location eastus
```

## <a name="deploy-the-template"></a>Implementar o modelo
Crie a aplicação e os recursos relacionados com um dos seguintes comandos.

Para Linux:

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json
```

Para Windows:

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json
```

Siga as indicações para introduzir o nome da partilha de ficheiros, o nome e a chave de conta para a partilha de ficheiros do Azure que fornece o volume. Num minuto mais ou menos, o comando deverá devolver com `"provisioningState": "Succeeded"`.

O parâmetro de palavra-passe no modelo é de `string` tipo para facilidade de utilização. Será apresentado no ecrã com texto não criptografado e o estado de implementação.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Certifique-se de que o aplicativo é capaz de usar o volume
O aplicativo cria um ficheiro denominado _data.txt_ na partilha de ficheiros (se não existir já). O conteúdo deste ficheiro é um número que é incrementado a cada 30 segundos pela aplicação. Para verificar que o exemplo funciona corretamente, abra a _data.txt_ periodicamente de ficheiros e certifique-se de que o número está a ser atualizado.

O ficheiro pode ser transferido através de qualquer ferramenta que permite a navegação de uma partilha de ficheiros de ficheiros do Azure. O [Explorador de armazenamento do Microsoft Azure](https://azure.microsoft.com/features/storage-explorer/) é um exemplo de uma ferramenta desse tipo.

## <a name="next-steps"></a>Passos Seguintes

- Ver a aplicação de exemplo do volume de ficheiros do Azure no [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/azurefiles-volume).
- Para saber mais sobre o modelo de recursos do Service Fabric, veja [modelo de recursos de malha do Service Fabric](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).
