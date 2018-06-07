---
title: Implementar um Cluster de Kubernetes à pilha do Azure | Microsoft Docs
description: Saiba como implementar um Cluster de Kubernetes à pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 43c0b7c87f9ee1cd33da3d617747c11dc120e51a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823627"
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Implementar um cluster de Kubernetes à pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

> [!Note]  
> O Kubernetes de serviços de contentor do Azure (ACS) na pilha do Azure está em pré-visualização privada. O operador de pilha do Azure terá de pedir acesso para o item do Kubernetes Marketplace necessário para desempenhar as instruções neste artigo.

O seguinte artigo observa através de um modelo de solução do Azure Resource Manager para implementar e aprovisionar os recursos para Kubernetes numa operação única e coordenada. Irá precisar para recolher as informações necessárias sobre a instalação de pilha do Azure, gerar o modelo e, em seguida, implementar a sua nuvem.

## <a name="kubernetes-and-containers"></a>Kubernetes e contentores

Pode instalar Kubernetes através de modelos Azure Resource Manager gerados pelo motor de serviços de contentor do Azure (ACS) na pilha do Azure. [Kubernetes](https://kubernetes.io) é um sistema de open source para automatizar a implementação, dimensionamento e gestão de aplicações nos contentores. A [contentor](https://www.docker.com/what-container) está contido numa imagem, semelhante a uma VM. Ao contrário de uma VM, a imagem de contentor inclui apenas os recursos tem de executar uma aplicação, tais como o código, o tempo de execução para executar o código, bibliotecas específicas e definições.

Pode utilizar Kubernetes para:

- Desenvolva aplicações extremamente dimensionáveis, pode ser atualizadas, que podem ser implementadas em segundos. 
- Simplificar a estrutura da sua aplicação e melhorar a fiabilidade por aplicações Helm diferentes. [Helm](https://github.com/kubernetes/helm) é uma ferramenta de empacotamento de open source que o ajuda a instalar e gerir o ciclo de vida das aplicações Kubernetes.
- Monitorizar e diagnosticar o estado de funcionamento das suas aplicações com uma escala e facilmente a funcionalidade de atualização.

## <a name="prerequisites"></a>Pré-requisitos 

Para começar, certifique-se de que tem as permissões corretas e de que a pilha do Azure está pronta.

1. Certifique-se de que pode criar aplicações no seu inquilino do Azure Active Directory (Azure AD). Tem as permissões para a implementação de Kubernetes.

    Para obter instruções sobre a verificar as suas permissões, consulte [permissões de verificação do Azure Active Directory](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

2. Gere um público e privado par de chaves SSH para iniciar sessão para a VM com Linux na pilha do Azure. Terá da chave pública ao criar o cluster.

    Para obter instruções sobre como gerar uma chave, consulte [geração de chave SSH](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

3. Certifique-se de que tem uma subscrição válida no seu portal inquilino de pilha do Azure e se tem suficiente IP público endereços disponíveis para adicionar novas aplicações.

    O cluster não é possível implementar uma pilha de Azure **administrador** subscrição. Tem de utilizar uma subscrição de utilizador * *. 

## <a name="create-a-service-principal-in-azure-ad"></a>Criar um principal de serviço no Azure AD

1. Iniciar sessão para o global [portal do Azure](http://portal.azure.com).
2. Certifique-se de que iniciou sessão com o inquilino do Azure AD associado com a instância de pilha do Azure.
3. Crie uma aplicação do Azure AD.

    a. Selecione **do Azure Active Directory** > **+ registos de aplicação** > **novo registo de aplicação**.

    b. Introduza um **nome** da aplicação.

    c. Selecione **aplicação Web / API**.

    d. Introduza `http://localhost` para o **URL de início de sessão**.

    c. Clique em **Criar**.

4. Anote o **ID da aplicação**. É necessário o ID quando criar o cluster. O ID é referenciado como **ID de cliente de Principal de serviço**.

5. Selecione **definições** > **chaves**.

    a. Introduza o **Descrição**.

    b. Selecione **nunca expira** para **expira**.

    c. Selecione **Guardar**. Certifique-tenha em atenção a cadeia de chave. Terá a cadeia de chave ao criar o cluster. A chave é referenciada como o **segredo do cliente de Principal de serviço**.



## <a name="give-the-service-principal-access"></a>Conceder o acesso de principal de serviço

Conceda o acesso de principal de serviço à sua subscrição para que o principal pode criar recursos.

1.  Iniciar sessão para o [do portal de administração](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **subscrições de utilizador** > **+ adicionar**.

3. Selecione a subscrição que criou.

4. Selecione **(IAM) do controlo de acesso** > selecione **+ adicionar**.

5. Selecione o **proprietário** função.

6. Selecione o nome de aplicação criado para o seu serviço principal. Poderá ter de escrever o nome na caixa de pesquisa.

7. Clique em **Guardar**.

## <a name="deploy-a-kubernetes-cluster"></a>Implementar um Cluster de Kubernetes

1. Abra o [portal do Azure pilha](https://portal.local.azurestack.external).

2. Selecione **+ nova** > **computação** > **Kubernetes Cluster**. Clique em **Criar**.

    ![Implementar Modelo de Solução](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

3. Selecione **Noções básicas** na criar Kubernetes Cluster.

    ![Implementar Modelo de Solução](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

2. Introduza o **o nome de utilizador do Linux VM Admin**. Nome de utilizador para as máquinas de virtuais do Linux que fazem parte do Kubernetes cluster e DVM.

3. Introduza o **chave pública SSH** utilizadas para autorização para todos os computadores Linux criado como parte do Kubernetes cluster e DVM.

4. Introduza o **prefixo de DNS de perfil do mestre** que seja exclusivo para a região. Tem de ser um nome exclusivo para a região, tal como `k8s-12345`. Tentar escolhido, mesmo que o grupo de recursos nome como melhor prática.

    > [!Note]  
    > Para cada cluster, utilize um prefixo de DNS novos e únicos perfil principal.

5. Introduza o **contagem de perfil de agrupamento de agentes**. A contagem contém o número de agentes no cluster. Pode ser de 1 a 4.

6. Introduza o **Principal de serviço ClientId** é utilizada pelo fornecedor de nuvem do Azure de Kubernetes.

7. Introduza o **segredo do cliente de Principal de serviço** que criou ao criar a aplicação principal de serviço.

8. Introduza o **versão do fornecedor de nuvem Kubernetes Azure**. Esta é a versão do fornecedor do Kubernetes Azure. Pilha do Azure disponibiliza uma compilação Kubernetes personalizada para cada versão de pilha do Azure.

9. Selecione o **subscrição** ID.

10. Introduza o nome de um novo grupo de recursos ou selecione um grupo de recursos existente. O nome do recurso tem de ser alfanumérico e minúsculas.

11. Selecione o **localização** do grupo de recursos. Esta é a região que escolher para a instalação de pilha do Azure.

### <a name="specify-the-azure-stack-settings"></a>Especifique as definições de pilha do Azure

1. Selecione o **definições de carimbo de pilha do Azure**.

    ![Implementar Modelo de Solução](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings.png)

2. Introduza o **inquilino Arm Endpoint**. Este é o ponto final do Azure Resource Manager para ligar ao criar o grupo de recursos para o cluster Kubernetes. Terá de obter o ponto final do operador de pilha do Azure para um sistema integrado. Para o Kit de desenvolvimento a pilha de Azure (ASDK), pode utilizar `https://management.local.azurestack.external`.

3. Introduza o **ID do inquilino** para o inquilino. Se necessitar de ajuda para localizar este valor, consulte o artigo [obter ID de inquilino](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

## <a name="connect-to-your-cluster"></a>Ligar ao cluster

Agora está pronto para ligar ao cluster. O mestre de podem ser encontrado no seu grupo de recursos do cluster e tem o nome `k8s-master-<sequence-of-numbers>`. Utilize um cliente SSH para ligar ao principal. No mestre, pode utilizar **kubectl**, o cliente de linha de comandos Kubernetes para gerir o cluster. Para obter instruções, consulte [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Também pode encontrar o **Helm** útil para instalar e implementar aplicações para o cluster de Gestor de pacotes. Para obter instruções sobre como instalar e utilizar Helm com o cluster, consulte [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Passos Seguintes

[Adicionar um Cluster de Kubernetes no Marketplace (para o operador de pilha do Azure)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes no Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
