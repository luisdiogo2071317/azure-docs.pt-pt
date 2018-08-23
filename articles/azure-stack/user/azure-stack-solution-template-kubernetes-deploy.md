---
title: Implementar um Cluster de Kubernetes no Azure Stack | Documentos da Microsoft
description: Saiba como implementar um Cluster de Kubernetes no Azure Stack.
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
ms.date: 08/09/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: f6a2b3f242e5989d0c72083eef4faad9c4798cfe
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42058154"
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Implementar um cluster de Kubernetes no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

> [!Note]  
> O Kubernetes de serviços de Kubernetes do Azure (AKS) no Azure Stack está em pré-visualização privada. O operador do Azure Stack tem de pedir acesso para o item do Marketplace de Kubernetes necessário para executar as instruções neste artigo.

O seguinte artigo analisa com um modelo de solução do Azure Resource Manager para implementar e aprovisionar os recursos do Kubernetes numa operação única e coordenada. Serão necessárias para recolher as informações necessárias sobre a instalação do Azure Stack, gerar o modelo e, em seguida, implementar a sua cloud.

## <a name="kubernetes-and-containers"></a>Kubernetes e contentores

Pode instalar o Kubernetes com modelos do Azure Resource Manager gerados pelo motor de serviços de Kubernetes do Azure (AKS) no Azure Stack. [Kubernetes](https://kubernetes.io) é um sistema de código-fonte aberto para automatizar a implantação, dimensionamento e da gestão de aplicações em contentores. R [contentor](https://www.docker.com/what-container) está contida numa imagem, semelhante a uma VM. Ao contrário de uma VM, a imagem de contentor inclui apenas os recursos de que necessita para executar um aplicativo, como o código, o tempo de execução para executar o código, bibliotecas específicas e definições.

Pode utilizar o Kubernetes para:

- Desenvolva aplicações escaláveis em massa, pode ser atualizadas, que podem ser implementadas em segundos. 
- Simplificar o design da sua aplicação e melhore a sua confiabilidade ao diferentes aplicativos de Helm. [Helm](https://github.com/kubernetes/helm) é uma ferramenta de empacotamento de código-fonte aberto que ajuda a instalar e gerenciar o ciclo de vida de aplicações do Kubernetes.
- Facilmente monitorizar e diagnosticar o estado de funcionamento das suas aplicações com o dimensionamento e funcionalidade de atualização.

## <a name="prerequisites"></a>Pré-requisitos 

Para começar, certifique-se de que tem as permissões corretas e que o Azure Stack está pronto.

1. Certifique-se de que pode criar aplicativos no seu inquilino do Azure Active Directory (Azure AD). Tem estas permissões para a implementação de Kubernetes.

    Para obter instruções sobre como verificar a suas permissões, consulte [permissões de verificação do Active Directory do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

1. Gere um público e privado par de chaves SSH para iniciar sessão na VM do Linux no Azure Stack. Terá a chave pública ao criar o cluster.

    Para obter instruções sobre como gerar uma chave, consulte [geração de chave SSH](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Verifique se tem uma subscrição válida no seu portal de inquilino do Azure Stack, e que tem suficiente IP público de endereços disponíveis para adicionar novos aplicativos.

    O cluster não é possível implementar um Azure Stack **administrador** subscrição. Tem de utilizar uma subscrição do utilizador * *. 

## <a name="create-a-service-principal-in-azure-ad"></a>Criar um principal de serviço no Azure AD

1. Inicie sessão no global [portal do Azure](http://portal.azure.com).
1. Verificação que iniciou sessão com o inquilino do Azure AD associado à instância do Azure Stack.
1. Crie uma aplicação do Azure AD.

    a. Selecione **do Azure Active Directory** > **+ registos de aplicações** > **novo registo de aplicação**.

    b. Introduza um **nome** do aplicativo.

    c. Selecione **aplicação Web / API**.

    d. Introduza `http://localhost` para o **URL de início de sessão**.

    c. Clique em **Criar**.

1. Aponte o **ID da Aplicação**. Precisará do ID ao criar o cluster. O ID é referenciado como **ID de cliente do Principal de serviço**.

1. Selecione **configurações** > **chaves**.

    a. Introduza o **Descrição**.

    b. Selecione **nunca expira** para **Expires**.

    c. Selecione **Guardar**. Certifique-se de observar a cadeia de caracteres de chave. Terá a cadeia de caracteres de chave ao criar o cluster. A chave é referenciada como o **segredo do cliente de Principal de serviço**.



## <a name="give-the-service-principal-access"></a>Dar o acesso de principal de serviço

Conceder ao principal de serviço acesso à sua subscrição para que o principal pode criar recursos.

1.  Inicie sessão para o [portal de administração](https://adminportal.local.azurestack.external).

1. Selecione **mais serviços** > **subscrições do utilizador** > **+ adicionar**.

1. Selecione a subscrição que criou.

1. Selecione **controlo de acesso (IAM)** > selecione **+ adicionar**.

1. Selecione o **proprietário** função.

1. Selecione o nome da aplicação criado para o seu serviço principal. Poderá ter de escrever o nome na caixa de pesquisa.

1. Clique em **Guardar**.

## <a name="deploy-a-kubernetes-cluster"></a>Implementar um Cluster do Kubernetes

1. Abra o [portal do Azure Stack](https://portal.local.azurestack.external).

1. Selecione **+ nova** > **computação** > **Cluster do Kubernetes**. Clique em **Criar**.

    ![Implementar Modelo de Solução](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

1. Selecione **Noções básicas** na criar o Cluster de Kubernetes.

    ![Implementar Modelo de Solução](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Introduza o **nome de utilizador de administrador VM de Linux**. Nome de utilizador para as máquinas de virtuais de Linux que fazem parte do cluster de Kubernetes e DVM.

1. Introduza o **chave pública SSH** utilizado na autorização de todas as máquinas de Linux criados como parte do cluster de Kubernetes e DVM.

1. Introduza o **prefixo de DNS de perfil do mestre** que seja exclusivo para a região. Tem de ser um nome exclusivo para a região, tal como `k8s-12345`. Tente optou por ele mesmo que o grupo de recursos dê um nome como melhor prática.

    > [!Note]  
    > Para cada cluster, utilize um prefixo DNS de perfil de mestre de novos e únicos.

1. Introduza o **contagem de perfil de agrupamento de agentes**. A contagem de contém o número de agentes no cluster. Podem existir de 1 a 4.

1. Introduza o **ID de cliente do Principal de serviço** é utilizada pelo fornecedor de cloud do Azure do Kubernetes.

1. Introduza o **segredo do cliente de Principal de serviço** que criou ao criar a aplicação do principal de serviço.

1. Introduza o **versão de fornecedor de Cloud do Azure do Kubernetes**. Esta é a versão para o fornecedor do Azure do Kubernetes. O Azure Stack lança uma compilação do Kubernetes personalizada para cada versão do Azure Stack.

1. Selecione seu **subscrição** ID.

1. Introduza o nome de um novo grupo de recursos ou selecione um grupo de recursos existente. O nome do recurso tem de ser de alfanuméricos e minúsculas.

1. Selecione o **localização** do grupo de recursos. Esta é a região que escolher para a sua instalação do Azure Stack.

### <a name="specify-the-azure-stack-settings"></a>Especifique as definições do Azure Stack

1. Selecione o **definições de carimbo de data / do Azure Stack**.

    ![Implementar Modelo de Solução](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings.png)

1. Introduza o **ponto final Arm de inquilino**. Este é o ponto de final do Azure Resource Manager para ligar ao criar o grupo de recursos para o cluster de Kubernetes. Terá de obter o ponto final do seu operador do Azure Stack para um sistema integrado. Para o Azure Stack Development Kit (ASDK), pode usar `https://management.local.azurestack.external`.

1. Introduza o **ID de inquilino** para o inquilino. Se precisar de ajuda para localizar este valor, veja [obter ID de inquilino](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

## <a name="connect-to-your-cluster"></a>Ligar ao cluster

Está agora pronto para ligar ao seu cluster. O modelo global pode ser encontrado no seu grupo de recursos do cluster e, com o nome `k8s-master-<sequence-of-numbers>`. Utilize um cliente SSH para ligar a mestre. No controlador, pode usar **kubectl**, o cliente de linha de comandos do Kubernetes para gerir o cluster. Para obter instruções, consulte [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Também pode encontrar os **Helm** Gestor de pacotes úteis para a instalação e implementação de aplicações no seu cluster. Para obter instruções sobre como instalar e utilizar o Helm com o seu cluster, consulte [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Passos Seguintes

[Adicionar um Cluster de Kubernetes no Marketplace (para o operador do Azure Stack)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes no Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
