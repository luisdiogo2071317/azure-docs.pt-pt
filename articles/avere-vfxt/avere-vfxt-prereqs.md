---
title: Avere vFXT pré-requisitos - Azure
description: Pré-requisitos para Avere vFXT para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 9c3301ba16bfaeb7014658a380e287a36a505be8
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299212"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Preparar para criar o vFXT Avere

Este artigo explica as tarefas de pré-requisitos para a criação de um cluster de vFXT Avere.

## <a name="create-a-new-subscription"></a>Criar uma nova subscrição

Comece por criar uma nova subscrição do Azure. Utilize uma subscrição separada para cada projeto de vFXT Avere para permitem-lhe controlar todos os recursos de projeto e despesas facilmente, proteger os outros projetos de limitação durante o aprovisionamento de recursos possível e simplificar a limpeza.  

Para criar uma nova subscrição do Azure no portal do Azure:

* Navegue para o [painel de subscrições](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)
* Clique nas **+ adicionar** botão na parte superior
* Iniciar sessão, se lhe for pedido
* Selecionar uma oferta e siga os passos para criar uma nova subscrição

## <a name="configure-subscription-owner-permissions"></a>Configurar permissões de proprietário da subscrição

Um utilizador com permissões de proprietário da subscrição deve criar o cluster vFXT. São necessárias permissões de proprietário de subscrição para estas ações, entre outras:

* Aceitar os termos de software de vFXT Avere
* Criar a função de acesso de nó de cluster 

Existem duas soluções alternativas, se não pretender conceder acesso de proprietário para os utilizadores que criam o vFXT:

* Um proprietário do grupo de recursos pode criar um cluster, se estas condições são cumpridas:

  * Tem um proprietário da subscrição [aceite os termos de software de vFXT Avere](#accept-software-terms) e [criar a função de acesso de nó de cluster](#create-the-cluster-node-access-role). 
  * Todos os recursos de vFXT Avere tem de ser implementados dentro do grupo de recursos, incluindo:
    * Controlador de cluster
    * Nós de cluster
    * Armazenamento de blobs
    * Elementos de rede
 
* Um utilizador com privilégios de proprietário não pode criar clusters de vFXT ao utilizar o controlo de acesso baseado em funções (RBAC) antes do tempo para atribuir privilégios ao utilizador. Este método fornece permissões significativas a estes utilizadores. [Este artigo](avere-vfxt-non-owner.md) explica como criar uma função de acesso para autorizar não proprietários para criar clusters.

## <a name="quota-for-the-vfxt-cluster"></a>Quota para o cluster vFXT

Tem de ter uma quota suficiente para os seguintes componentes do Azure. Se for necessário, [pedir um aumento de quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> As máquinas virtuais e os componentes SSD listados aqui são para o próprio cluster vFXT. Terá de quota adicional para as VMs e SSD que tenciona utilizar para o farm de computação.  Certifique-se de que a quota estiver ativada para a região onde pretende executar o fluxo de trabalho.

|Componente do Azure|Quota|
|----------|-----------|
|Máquinas virtuais|3 ou mais D16s_v3 ou E32s_v3|
|Armazenamento SSD Premium|200 GB de espaço para SO e 1 a 4 TB de espaço na cache por nó |
|Conta de armazenamento (opcional) |v2|
|Armazenamento de back-end de dados (opcional) |Um novo contentor de BLOBs LRS |

## <a name="accept-software-terms"></a>Aceite os termos de software

> [!NOTE] 
> Este passo não é necessário se o proprietário da subscrição cria o cluster de vFXT Avere.

Durante a criação do cluster, tem de aceitar os termos de serviço para o software de vFXT Avere. Se não for um proprietário da subscrição, tem um proprietário da subscrição de aceitar os termos antes do tempo. Este passo só precisa ser feito uma vez por subscrição.

Para aceitar o software de termos com antecedência: 

1. Abra uma shell de cloud no portal do Azure ou ao navegar para <https://shell.azure.com>. Inicie sessão com o ID da subscrição.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Emita este comando para aceitar os termos de serviço e ativar o acesso programático a vFXT Avere para a imagem de software do Azure: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-access-roles"></a>Criar funções de acesso 

[Controlo de acesso baseado em funções](../role-based-access-control/index.yml) (RBAC) dá nós de cluster e o controlador de cluster vFXT autorização para efetuar tarefas necessárias.

* O controlador de cluster necessita da permissão para criar e modificar as VMs para criar o cluster. 

* VFXT individual, nós têm de fazer coisas como ler propriedades de recursos do Azure e gerir o armazenamento para controlar as definições de interface de rede dos outros nós como parte da operação de normal cluster.

Antes de poder criar o cluster de vFXT Avere, tem de definir uma função personalizada para utilizar com os nós de cluster. 

Para o controlador de cluster, pode aceitar a função predefinida do modelo. O padrão fornece o cluster de privilégios de proprietário de grupo de recursos de controlador. Se preferir para criar uma função personalizada para o controlador, veja [função de acesso de controlador personalizado](avere-vfxt-controller-role.md).

> [!NOTE] 
> Apenas o proprietário da subscrição, ou um utilizador com a função de proprietário ou administrador de acesso de utilizadores, pode criar funções. As funções podem ser criadas antes do tempo.  

### <a name="create-the-cluster-node-access-role"></a>Criar a função de acesso de nó de cluster

<!-- caution - this header is linked to in the template so don't change it unless you can change that -->

Tem de criar a função de nó de cluster antes de poder criar o vFXT Avere para cluster do Azure.

> [!TIP] 
> Usuários internos da Microsoft devem utilizar a função existente com o nome "Operador de tempo de execução do Cluster de Avere" em vez de tentar criar um. 

1. Copie esse arquivo. Adicione o seu ID de subscrição na linha AssignableScopes.

   (A versão atual deste ficheiro é armazenada no repositório como github.com/Azure/Avere [AvereOperator.txt](https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereOperator.txt).)  

   ```json
   {
      "AssignableScopes": [
          "/subscriptions/PUT_YOUR_SUBSCRIPTION_ID_HERE"
      ],
      "Name": "Avere Operator",
      "IsCustom": "true",
      "Description": "Used by the Avere vFXT cluster to manage the cluster",
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

1. Guarde o ficheiro como ``avere-operator.json`` ou um nome de ficheiro fácil de memorizar. 


1. Abra uma shell de nuvem do Azure e inicie sessão com o seu ID de subscrição (descrito [anteriormente neste documento](#accept-software-terms)). Use este comando para criar a função:

   ```bash
   az role definition create --role-definition /avere-operator.json
   ```

O nome da função é utilizado ao criar o cluster. Neste exemplo, o nome é ``avere-operator``.

## <a name="next-step-create-the-vfxt-cluster"></a>Passo seguinte: Criar o cluster vFXT

Depois de concluir estes pré-requisitos, pode saltar para criar o cluster em si. Leia [implementar o cluster vFXT](avere-vfxt-deploy.md) para obter instruções.