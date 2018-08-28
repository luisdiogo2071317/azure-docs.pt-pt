---
title: Criar um cluster do Azure Kubernetes Service (AKS)
description: Crie um cluster do AKS com a CLI ou o portal do Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6bfe6f9b76693ded79aa9b9d21ddcac4e1a0733e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43110309"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Criar um cluster do Azure Kubernetes Service (AKS)

Um cluster do Azure Kubernetes Service (AKS) pode ser criado com a CLI do Azure ou o portal do Azure.

## <a name="azure-cli"></a>CLI do Azure

Utilize o [criar az aks] [ az-aks-create] comando para criar o cluster do AKS.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

As seguintes opções estão disponíveis com o `az aks create` comando. Consulte a [referência da CLI do Azure] [ az-aks-create] para AKS para obter mais informações sobre cada um desses argumentos.

| Argumento | Descrição | Necessário |
|---|---|:---:|
| `--name` `-n` | Nome do recurso para o cluster gerido. | sim |
| `--resource-group` `-g` | Nome do grupo de recursos do Azure Kubernetes Service. | sim |
| `--admin-username` `-u` | Nome de utilizador para as máquinas de virtuais do Linux.  Predefinição: azureuser. | não |
| `--aad-client-app-id` | (PRÉ-VISUALIZAÇÃO) O ID de um aplicativo de cliente do Azure Active Directory do tipo "Nativo". | não |
| `--aad-server-app-id` | (PRÉ-VISUALIZAÇÃO) O ID de um aplicativo de servidor do Azure Active Directory do tipo "Aplicação/API da Web". | não |
| `--aad-server-app-secret` | (PRÉ-VISUALIZAÇÃO) O segredo de um aplicativo de servidor do Azure Active Directory. | não |
| `--aad-tenant-id` | (PRÉ-VISUALIZAÇÃO) O ID de um inquilino do Azure Active Directory. | não |
| `--admin-username` `-u` | Conta de utilizador para criar as VMs de nó para o acesso SSH.  Predefinição: azureuser. | não |
| ` --client-secret` | O segredo associado ao principal de serviço. | não |
| `--dns-name-prefix` `-p` | Prefixo DNS para o endereço ip público de clusters. | não |
| `--dns-service-ip` | Um endereço IP atribuído para o serviço DNS do Kubernetes. | não |
| `--docker-bridge-address` | Um endereço IP e máscara de rede atribuído para a bridge do Docker. | não |
| `--enable-addons` `-a` | Ative os complementos de Kubernetes numa lista separada por vírgulas. | não |
| `--enable-rbac` `-r` | Ative o controlo de acesso baseado em função do Kubernetes. | não |
| `--generate-ssh-keys` | Gere ficheiros de chaves públicos e privados SSH se estiver em falta. | não |
| `--kubernetes-version` `-k` | A versão do Kubernetes a utilizar para criar o cluster, como '1.7.9' ou '1.9.6'. | não |
| `--location` `-l` | Localização do grupo de recursos criada automaticamente | não |
| `--max-pods` `-m` | O número máximo de pods implementáveis para um nó. | não |
| `--network-plugin` | O rede de Kubernetes Plug-in para utilizar. | não |
| `--no-ssh-key` `-x` | Não utilizar ou crie uma chave SSH local. | não |
| `--no-wait` | Não aguarde a conclusão da operação de longa execução. | não |
| `--node-count` `-c` | O número predefinido de nós para os conjuntos de nó.  Predefinição: 3. | não |
| `--node-osdisk-size` | O tamanho de osDisk em GB do conjunto de nós de Máquina Virtual. | não |
| `--node-vm-size` `-s` | O tamanho da Máquina Virtual.  Predefinição: Standard_D1_v2. | não |
| `--pod-cidr` | Intervalo de IP de notação CIDR partir da qual pretende atribuir o pod IPs quando kubenet é utilizado. | não |
| `--service-cidr` | Intervalo de IP de notação CIDR partir da qual pretende atribuir os IPs do cluster do serviço. | não |
| `--service-principal` | Principal de serviço utilizado para autenticação de cluster. | não |
| `--ssh-key-value` | SSH ficheiro de chave valor ou a chave de caminho do ficheiro.  Predefinição: ~ /.ssh/id_rsa.pub. | não |
| `--tags` | Espaço separados etiquetas em 'key [= value]' formato. Utilização ' para limpar as etiquetas existentes. | não |
| `--vnet-subnet-id` | O ID de uma sub-rede numa VNet existente no qual implementar o cluster. | não |
| `--workspace-resource-id` | O ID de recurso de um registo de análise de área de trabalho existente a utilizar para armazenar dados de monitorização. | não |

## <a name="azure-portal"></a>Portal do Azure

Para obter instruções sobre como implementar um cluster do AKS com o portal do Azure, veja o Azure Kubernetes Service (AKS) [início rápido do portal do Azure][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
