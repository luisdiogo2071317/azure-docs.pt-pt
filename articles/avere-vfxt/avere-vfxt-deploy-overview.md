---
title: Descrição geral da implementação - Avere vFXT para o Azure
description: Visão geral da implantação Avere vFXT para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aa5737d67ea2c9cb8cc7c7098764ae67fc91137d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634468"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT para o Azure - descrição geral da implementação

Este artigo fornece uma descrição geral das etapas necessárias para obter um vFXT Avere para um cluster do Azure backup e em execução.

Na primeira vez que implementar um sistema de vFXT Avere, pode observar que envolve mais passos que implementar a maioria das outras ferramentas do Azure. Ter uma idéia clara do processo de início para concluir irá ajudá-lo a definir o âmbito o esforço necessário. Depois do sistema está em execução, sua capacidade de acelerar as tarefas de computação com base na cloud irá permitir que vale a pena o esforço.

## <a name="deployment-steps"></a>Passos da implementação

Após [planear o seu sistema](avere-vfxt-deploy-plan.md), pode começar a criar o cluster de vFXT Avere. 

Comece por criar um VM, o que é utilizada para criar o cluster vFXT do controlador de cluster.

Depois do cluster de vFXT estiver em execução, desejará sabe como ligar a clientes ao mesmo e, se necessário, como mover seus dados para o novo contentor de armazenamento de Blobs.  

Aqui está uma visão geral de todos os passos.

1. Configurar os pré-requisitos 

   Antes de criar uma VM, tem de criar uma nova subscrição para o projeto de vFXT Avere, configurar a propriedade da subscrição, verifique as quotas e pedir um aumento, se necessário e aceitar os termos de utilização de software de vFXT Avere. Leia [preparar para criar o vFXT Avere](avere-vfxt-prereqs.md) para obter instruções detalhadas.

1. Criar o controlador de cluster

   O *controlador de cluster* é uma VM simple que residem na mesma rede virtual do cluster de vFXT Avere. O controlador cria os nós de vFXT e formulários, o cluster, e também fornece uma interface de linha de comandos para gerir o cluster durante seu ciclo de vida.

   Se configurar seu controlador com um endereço IP público, que também pode servir como um anfitrião de atalhos para ligar para o cluster a partir de fora da vnet.

   Todo o software necessário para criar o cluster vFXT e gerir os respetivos nós é pré-instalado no controlador de cluster.

   Leia [criar a VM do controlador de cluster](avere-vfxt-deploy.md#create-the-cluster-controller-vm) para obter detalhes.

1. Criar uma função de tempo de execução para os nós do cluster 

   O Azure utiliza [controlo de acesso baseado em funções](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) para autorizar o nó de cluster de VMs para executar determinadas tarefas. Por exemplo, os nós do cluster tem de ser capaz de atribuir ou reatribuem endereços IP para outros nós do cluster. Antes de criar o cluster, tem de definir uma função que lhes concede as permissões adequadas.

   Software de pré-instalados do controlador de cluster inclui uma função de protótipo para personalização. Leia [criar a função de acesso de nó de cluster](avere-vfxt-deploy.md#create-the-cluster-node-access-role) para obter instruções.

1. Criar o cluster de vFXT Avere 

   No controlador, edite o script de criação de clusters apropriada e executá-lo para criar o cluster. [Edite o script de implementação](avere-vfxt-deploy.md#edit-the-deployment-script) possua instruções detalhadas. 

1. Configurar o cluster 

   Ligue-se para a interface de configuração de vFXT Avere (painel de controlo de Avere) para personalizar definições do cluster. Escolher para a monitorização de suporte e adicione o seu sistema de armazenamento se estiver a utilizar um centro de dados no local.

   * [Aceder ao cluster vFXT](avere-vfxt-cluster-gui.md)
   * [Ativar o suporte](avere-vfxt-enable-support.md)
   * [Configurar o armazenamento](avere-vfxt-add-storage.md) (se necessário)

1. Clientes de montagem

   Siga as diretrizes [montar o cluster de vFXT Avere](avere-vfxt-mount-clients.md) para saber mais sobre o balanceamento de carga e como máquinas de cliente devem montar o cluster.

1. Adicionar dados (se necessário)

   Como o vFXT Avere é uma cache de cliente multi dimensionável, é a melhor forma de mover dados para um novo contentor de armazenamento de back-end com a estratégia de cópia de ficheiros de vários clientes, vários threads. Leia [mover dados para o cluster vFXT](avere-vfxt-data-ingest.md) para obter detalhes.

## <a name="next-steps"></a>Passos Seguintes

Continuar a [preparar para criar o vFXT Avere](avere-vfxt-prereqs.md) para concluir as tarefas preliminares para implementar o vFXT Avere para o Azure. 