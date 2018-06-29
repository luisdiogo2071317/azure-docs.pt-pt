---
title: Criar um cluster do serviço de Kubernetes do Azure (AKS)
description: Crie um cluster AKS com a CLI ou o portal do Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 304f3807a70179e4aab2ede80dc08a1aa85a2e51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098911"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Criar um cluster do serviço de Kubernetes do Azure (AKS)

Um cluster do serviço de Kubernetes do Azure (AKS) pode ser criado com a CLI do Azure ou o portal do Azure.

## <a name="azure-cli"></a>CLI do Azure

Utilize o [az aks criar] [ az-aks-create] comando para criar o cluster AKS.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

As seguintes opções estão disponíveis com o `az aks create` comando. Consulte o [referência da CLI do Azure] [ az-aks-create] para AKS para obter mais informações sobre cada um destes argumentos.

| Argumento | Descrição | Necessário |
|---|---|:---:|
| `--name` `-n` | Nome do recurso para o cluster gerido. | sim |
| `--resource-group` `-g` | Nome do grupo de recursos do Azure Kubernetes serviço. | sim |
| `--admin-username` `-u` | Nome de utilizador para as máquinas de virtuais do Linux.  Predefinição: azureuser. | não |
| `--aad-client-app-id` | (PRÉ-VISUALIZAÇÃO) O ID de uma aplicação de cliente do Azure Active Directory do tipo "Nativo". | não |
| `--aad-server-app-id` | (PRÉ-VISUALIZAÇÃO) O ID de uma aplicação de servidor do Azure Active Directory do tipo "Aplicação Web/API". | não |
| `--aad-server-app-secret` | (PRÉ-VISUALIZAÇÃO) O segredo de uma aplicação de servidor do Azure Active Directory. | não |
| `--aad-tenant-id` | (PRÉ-VISUALIZAÇÃO) O ID de um inquilino do Azure Active Directory. | não |
| `--admin-username` `-u` | Conta de utilizador a criar num nó de VMs para o acesso SSH.  Predefinição: azureuser. | não |
| ` --client-secret` | O segredo associado ao principal de serviço. | não |
| `--dns-name-prefix` `-p` | Prefixo DNS para o endereço de ip público de clusters. | não |
| `--dns-service-ip` | Um endereço IP atribuído para o serviço Kubernetes DNS. | não |
| `--docker-bridge-address` | Um endereço IP e máscara de rede atribuído à bridge de Docker. | não |
| `--enable-addons` `-a` | Ative o addons Kubernetes numa lista separada por vírgulas. | não |
| `--enable-rbac` `-r` | Ative o controlo de acesso baseado em funções de Kubernetes. | não |
| `--generate-ssh-keys` | Gere ficheiros de chaves públicos e privados SSH se estiverem em falta. | não |
| `--kubernetes-version` `-k` | A versão do Kubernetes a utilizar para criar o cluster, tais como '1.7.9' ou '1.9.6'. | não |
| `--locaton` `-l` | Localização para o grupo de recursos criados automaticamente | não |
| `--max-pods` `-m` | O número máximo de pods implementáveis para um nó. | não |
| `--network-plugin` | O rede Kubernetes Plug-in para utilizar. | não |
| `--no-ssh-key` `-x` | Não utilizar ou criar uma chave SSH local. | não |
| `--no-wait` | Não aguarde a operação de execução longa concluir. | não |
| `--node-count` `-c` | O número predefinido de nós para os conjuntos de nó.  Predefinição: 3. | não |
| `--node-osdisk-size` | O tamanho de osDisk em GB do conjunto de nó Máquina Virtual. | não |
| `--node-vm-size` `-s` | O tamanho da Máquina Virtual.  Predefinição: Standard_D1_v2. | não |
| `--pod-cidr` | Intervalo de IP de notação CIDR partir do qual atribui pod IPs quando kubenet é utilizado. | não |
| `--service-cidr` | Intervalo de IP de notação CIDR partir do qual atribui o cluster do serviço de IPs. | não |
| `--service-principal` | Principal de serviço utilizado para autenticação de cluster. | não |
| `--ssh-key-value` | Ficheiro de chave valor ou chave de ficheiro caminho SSH.  Predefinição: ~ /.ssh/id_rsa.pub. | não |
| `--tags` | Espaço em separado as etiquetas na 'key [= value]' formato. Utilize "para limpar as etiquetas existentes. | não |
| `--vnet-subnet-id` | O ID de uma sub-rede uma VNet existente para o qual pretende implementar o cluster. | não |
| `--workspace-resource-id` | O ID de recurso de um registo de análise de área de trabalho existente a utilizar para armazenar dados de monitorização. | não |

## <a name="azure-portal"></a>Portal do Azure

Para instruções sobre como implementar um cluster AKS com o portal do Azure, consulte o serviço de Kubernetes do Azure (AKS) [início rápido de portal do Azure][aks-portal-quickstart].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
