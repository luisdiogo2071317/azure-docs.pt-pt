---
title: Avere vFXT pré-requisitos - Azure
description: Pré-requisitos para Avere vFXT para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 823bf50a54ff43fa95f7136c137e3d8f3303c3e0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634381"
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
* Permitir que o nó de controlador de cluster gerir grupos de recursos e redes virtuais 
* Permitir que o controlador de cluster Criar e modificar os nós do cluster 

Existem duas soluções alternativas, se não pretender conceder acesso de proprietário para os utilizadores que criam o vFXT:

* Um proprietário do grupo de recursos pode criar um cluster, se estas condições são cumpridas:

  * Tem um proprietário da subscrição [aceite os termos de software de vFXT Avere](#accept-software-terms-in-advance) e [criar a função de acesso de nó de cluster](avere-vfxt-deploy.md#create-the-cluster-node-access-role).
  * Todos os recursos de vFXT Avere tem de ser implementados dentro do grupo de recursos, incluindo:
    * Controlador de cluster
    * Nós de cluster
    * Armazenamento de blobs
    * Elementos de rede
 
* Um utilizador com privilégios de proprietário não pode criar clusters de vFXT se uma função de um acesso adicional é criada e atribuída ao utilizador antes do tempo. No entanto, esta função concede-lhe significativas permissões para esses usuários. [Este artigo](avere-vfxt-non-owner.md) explica como autorizar não proprietários para criar clusters.

## <a name="quota-for-the-vfxt-cluster"></a>Quota para o cluster vFXT

Tem de ter uma quota suficiente para os seguintes componentes do Azure. Se for necessário, [pedir um aumento de quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> As máquinas virtuais e os componentes SSD listados aqui são para o próprio cluster vFXT. Terá de quota adicional para as VMs e SSD que tenciona utilizar para o farm de computação.  Certifique-se de que a quota estiver ativada para a região onde pretende executar o fluxo de trabalho.

|Componente do Azure|Quota|
|----------|-----------|
|Máquinas virtuais|3 ou mais D16s_v3 ou E32s_v3|
|Armazenamento SSD Premium|Espaço de SO de 200 GB e 1 TB de espaço da cache de 4 TB por nó |
|Conta de armazenamento (opcional) |v2|
|Armazenamento de back-end de dados (opcional) |Um novo contentor de BLOBs LRS |

## <a name="accept-software-terms-in-advance"></a>Aceitar termos com antecedência de software

> [!NOTE] 
> Este passo não é necessário se o proprietário da subscrição cria o cluster de vFXT Avere.

Antes de poder criar um cluster, tem de aceitar os termos de serviço para o software de vFXT Avere. Se não for um proprietário da subscrição, tem um proprietário da subscrição de aceitar os termos antes do tempo. Este passo só precisa ser feito uma vez por subscrição.

Para aceitar o software de termos com antecedência: 

1. Abra uma shell de cloud no portal do Azure ou ao navegar para <https://shell.azure.com>. Inicie sessão com o ID da subscrição.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Emita este comando para aceitar os termos de serviço e ativar o acesso programático a vFXT Avere para imagens de software do Azure: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-node:latest
   ```

## <a name="next-step-create-the-vfxt-cluster"></a>Passo seguinte: criar o cluster vFXT

Depois de concluir estes pré-requisitos, pode saltar para criar o cluster em si. Leia [implementar o cluster vFXT](avere-vfxt-deploy.md) para obter instruções.