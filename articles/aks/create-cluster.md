---
title: "Criar um cluster do serviço de contentor do Azure (AKS)"
description: Crie um cluster AKS com a CLI ou o portal do Azure.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3e884bc16680d74801911547045deb48246afccd
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="create-an-azure-container-service-aks-cluster"></a>Criar um cluster do serviço de contentor do Azure (AKS)

Um cluster do serviço de contentor do Azure (AKS) pode ser criado com a CLI do Azure ou o portal do Azure.

## <a name="azure-cli"></a>CLI do Azure

Utilize o [az aks criar] [ az-aks-create] comando para eliminar o cluster AKS.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

As seguintes opções estão disponíveis com o `az aks create` comando.

| Argumento | Descrição | Necessário |
|---|---|:---:|
| `--name` `-n` | Nome do recurso para o cluster gerido. | sim |
| `--resource-group` `-g` | Nome do grupo de recursos do serviço de contentor do Azure. | sim |
| `--admin-username` `-u` | Nome de utilizador para as máquinas de virtuais do Linux.  Predefinição: azureuser. | não |
| ` --client-secret` | O segredo associado ao serviço principal. | não |
| `--dns-name-prefix` `-p` | Prefixo DNS para o endereço de ip público de clusters. | não |
| `--generate-ssh-keys` | Gere ficheiros de chaves públicos e privados SSH se estiverem em falta. | não |
| `--kubernetes-version` `-k` | A versão do Kubernetes a utilizar para criar o cluster, tais como '1.7.9' ou '1.8.2'.  Predefinição: 1.7.7. | não |
| `--no-wait` | Não aguarde a operação de execução longa concluir. | não |
| `--node-count` `-c` | O número predefinido de nós para os conjuntos de nó.  Predefinição: 3. | não |
| `--node-osdisk-size` | O tamanho de osDisk em GB do conjunto de nó Máquina Virtual. | não |
| `--node-vm-size` `-s` | O tamanho da Máquina Virtual.  Predefinição: Standard_D1_v2. | não |
| `--service-principal` | Principal de serviço utilizado para autenticação de cluster. | não |
| `--ssh-key-value` | Ficheiro de chave valor ou chave de ficheiro caminho SSH.  Predefinição: ~ /.ssh/id_rsa.pub. | não |
| `--tags` | Espaço em separado as etiquetas na 'key [= value]' formato. Utilize "para limpar as etiquetas existentes. | não |

## <a name="azure-portal"></a>Portal do Azure

Para instruções sobre como implementar um cluster AKS com o portal do Azure, consulte o serviço de contentor do Azure (AKS) [início rápido de portal do Azure][aks-portal-quickstart]. 

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md