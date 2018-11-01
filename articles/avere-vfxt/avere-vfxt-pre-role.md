---
title: Criar função de Avere com nenhum controlador - Avere vFXT para o Azure
description: Método para criar a função RBAC necessária sem uma VM do controlador de cluster
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 547a0e0ac5254408a4064d627ec4c1e7c354a433
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634531"
---
# <a name="create-the-avere-vfxt-cluster-runtime-access-role-without-a-controller"></a>Criar a função de acesso de tempo de execução do Avere vFXT cluster sem um controlador

Este documento mostra um método para criar a função de acesso de nó de cluster a partir da linha de comandos, antes de criar a VM do controlador de cluster. 

Para criá-lo a partir do controlador de cluster, leia [criar a função de acesso de nó de cluster](avere-vfxt-deploy.md#create-the-cluster-node-access-role). A imagem de controlador inclui um arquivo de protótipo de função. Pode atualizar o ficheiro com o seu ID de subscrição e utilizá-lo para definir a função localmente no controlador de VM.

## <a name="create-an-azure-rbac-role"></a>Criar uma função RBAC do Azure

Utiliza o sistema de vFXT Avere [controlo de acesso baseado em funções](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) para autorizar os nós do cluster vFXT para executar tarefas necessárias.  

Como parte da operação de cluster normal vFXT, vFXT individual, nós têm de fazer coisas como ler propriedades de recursos do Azure, gerir o armazenamento e controlar as definições de interface de rede dos outros nós. 

Esta função é utilizada para os nós de vFXT apenas, não para a VM do controlador de cluster.  

Se pretender criar a função antes do controlador, siga estes passos: 

1. Abra o Azure Cloud Shell no portal do Azure ou navegue até [ https://shell.azure.com ](https://shell.azure.com).

1. Utilize o comando da CLI do Azure para mudar para a sua subscrição vFXT:

   ```az account set --subscription YOUR_SUBSCRIPTION_ID```

1. Utilize estes comandos para transferir a definição de função a partir da imagem do marketplace e editá-lo. 

   ```bash
   wget -O- https://averedistribution.blob.core.windows.net/public/vfxtdistdoc.tgz | tar zxf - avere-cluster.json
   vi avere-cluster.json
   ``` 

4. Edite o ficheiro para incluir o seu ID de subscrição e eliminar a linha acima dela. Guarde o ficheiro como ``avere-cluster.json``.

   ![Consola do editor de texto, que mostra o ID de subscrição e o "remover esta linha" selecionados para eliminação](media/avere-vfxt-edit-role.png)

5. Crie a função:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Quando criar o cluster, forneça o nome da função (nesse caso, `avere-cluster`). O script de criação do cluster atribui a função para os nós de cluster recém-criado. 

## <a name="sample-cluster-node-runtime-role-definition"></a>Definição de função de tempo de execução de nó de cluster de exemplo

> [!IMPORTANT] 
> Esta definição de exemplo foi efetuada a partir de uma versão anterior à disponibilidade geral do produto. Se a versão que obtém com a distribuição de produto atual for diferente, use essa versão.

```json

{
    "AssignableScopes": [
        "/subscriptions/YOUR_SUBSCRIPTION_ID_GOES_HERE"
    ],
    "Name": "avere-cluster",
    "IsCustom": "true",
    "Description": "Avere cluster runtime role",
    "NotActions": [],
    "Actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
    ],
    "DataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
    ]
}

```