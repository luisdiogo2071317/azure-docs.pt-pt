---
title: incluir ficheiro
description: incluir ficheiro
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 4c4a5b66fe35da01a3661715e17a9fda20bc2411
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43184714"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Criar um cluster do Kubernetes ativado para os Espaços de Programador do Azure

1. Inicie sessão no portal do Azure em http://portal.azure.com.
1. Escolha **Criar um recurso** > procure **Kubernetes** > selecione **Serviço Kubernetes** > **Criar**.

   Conclua os passos seguintes, descritos abaixo de cada um dos títulos do formulário para criar o cluster de AKS.

    - **DETALHES DO PROJETO**: selecione uma subscrição do Azure e um grupo de recursos, novo ou existente, do Azure.
    - **DETALHES DO CLUSTER**: introduza um nome, região (atualmente, tem de escolher EUALeste, EUACentral, EuropaOcidental, EUAOcidental2, CanadaCentral ou CanadaLeste), a versão e o prefixo de nome de DNS para o cluster do AKS.
    - **DIMENSIONAMENTO**: selecione um tamanho da VM para os nós de agente do AKS e o número de nós. Se está a começar de utilizar os Espaços de Programador do Azure, basta um nó para explorar todas as funcionalidades. A contagem de nós pode ser facilmente ajustada em qualquer altura depois de o cluster ser implementado. Note que o tamanho da VM não pode ser alterado após a criação de um cluster de AKS. No entanto, depois de um cluster de AKS ser implementado, pode criar facilmente um novo cluster de AKS com VMs maiores e utilizar Espaços de Programador para voltar a implementar para esse cluster maior, se precisar de aumentar verticalmente.

   Certifique-se de que escolhe a versão 1.9.6 ou posterior do Kubernetes.

   ![Definições de configuração do Kubernetes](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Selecione **Seguinte: Autenticação** quando terminar.

1. Escolha a definição pretendida para controlo de acesso baseado em funções (RBAC). Os espaços de desenvolvimento do Azure suportam clusters com RBAC ativado ou desativado.

    ![Definição de RBAC](../media/common/k8s-RBAC.PNG)

1. Certifique-se de que o Encaminhamento de Aplicações Http está ativado.

   ![Ativar Encaminhamento de Aplicações Http](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!Note]
    > Para habilitar o [Encaminhamento de Aplicações Http](/azure/aks/http-application-routing) num cluster existente, utilize o comando: `az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing`

1. Selecione **Rever + criar** e, em seguida, selecione **Criar** quando terminar.
