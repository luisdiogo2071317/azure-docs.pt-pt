---
title: Neste tutorial, vai criar uma VM de pilha do Azure através de um modelo | Documentos da Microsoft
description: Descreve como utilizar o ASDK para criar uma VM com um modelo de predfined e um modelo personalizado do GitHub.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 0b3ba5c3a091cf673d8b3dbc413d36cb5fb75de5
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713413"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Tutorial: criar uma VM com um modelo de Comunidade
Como operador do Azure Stack ou um utilizador, pode criar uma VM com [modelos de início rápido do GitHub personalizados](https://github.com/Azure/AzureStack-QuickStart-Templates) em vez de implantar um manualmente a partir do marketplace do Azure Stack.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Saiba mais sobre modelos de início rápido do Azure Stack 
> * Criar uma VM com um modelo personalizado do GitHub
> * Iniciar minikube e instalar uma aplicação

## <a name="learn-about-azure-stack-quickstart-templates"></a>Saiba mais sobre modelos de início rápido do Azure Stack
Modelos de início rápido do Azure Stack são armazenados no [repositório de modelos de início rápido de AzureStack público](https://github.com/Azure/AzureStack-QuickStart-Templates) no GitHub. Este repositório contém modelos de implementação Azure Resource Manager que foram testados com o Microsoft Azure Stack Development Kit (ASDK). Pode usá-los para que seja mais fácil para avaliar o Azure Stack e utilize o ambiente de ASDK. 

Ao longo do tempo muitos usuários do GitHub contribuíram para o repositório, resultando numa imensa coleção de mais de 400 modelos de implementação. Este repositório é um ótimo ponto de partida para obter uma melhor compreensão de como pode implementar vários tipos de ambientes no Azure Stack. 

>[!IMPORTANT]
> Alguns destes modelos são criados por membros da Comunidade e não pela Microsoft. Cada modelo é licenciado sob um contrato de licença pelo respetivo proprietário e não Microsoft. A Microsoft não se responsabiliza por estes modelos e não avaliza segurança, compatibilidade ou desempenho. Modelos da Comunidade não são suportados por nenhum serviço ou programa de suporte da Microsoft e são disponibilizados "Tal como está" sem garantias de qualquer tipo.

Se quiser contribuir com seus modelos do Azure Resource Manager para o GitHub, deve fazer sua contribuição para o [repositório de modelos de início rápido do azure](https://github.com/Azure/AzureStack-QuickStart-Templates).

Para saber mais sobre o repositório do GitHub e como contribuir para o mesmo, consulte a [ficheiro Leia-me](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 


## <a name="create-a-vm-using-a-custom-github-template"></a>Criar uma VM com um modelo personalizado do GitHub
Neste tutorial de exemplo, o [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) modelo de início rápido do Azure Stack é utilizado para implementar uma máquina virtual do Ubuntu 16.04 no Azure Stack Minikube para gerir um cluster de Kubernetes a executar.

Minikube é uma ferramenta que simplifica a executar localmente o Kubernetes. Minikube executa um cluster de Kubernetes de nó único dentro de uma VM, permitindo-lhe experimentar o Kubernetes ou desenvolver com o mesmo dia a dia. Ele oferece suporte um cluster do Kubernetes simple, um nó em execução numa VM do Linux. Minikube é a forma mais rápida e mais simples para obter um cluster do Kubernetes totalmente funcional em execução. Permite que os desenvolvedores desenvolver e testar as suas implementações de aplicações com base em Kubernetes em suas máquinas locais. Em termos de arquitetura, a VM Minikube executa mestre e componentes do nó de agente localmente:

- Componentes do nó principal, como o servidor de API, Scheduler, e [etcd servidor](https://coreos.com/etcd/) são executados num único processo do Linux denominado LocalKube.
- Componentes do nó de agente são executados dentro de contentores do docker, exatamente como eles seriam executada num nó de agente normal. De um ponto de vista de implementação de aplicação, não existe nenhuma diferença quando a aplicação é implementada num cluster do Kubernetes regular ou Minikube.

Esse modelo instala os seguintes componentes:

- VM do Ubuntu 16.04 LTS
- [Docker-CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> A imagem de VM do Ubuntu (Ubuntu Server 16.04 LTS neste exemplo) tem já foram adicionada para o mercado do Azure Stack antes de iniciar estes passos.

1.  Clique em **+ criar um recurso** > **personalizado** > **implementação do modelo**.

    ![](media/azure-stack-create-vm-template/1.PNG) 

2. Clique em **Editar modelo**.

    ![](media/azure-stack-create-vm-template/2.PNG) 

3.  Clique em **modelo de início rápido**.

    ![](media/azure-stack-create-vm-template/3.PNG)

4. Selecione **101-vm-linux-minikube** entre os modelos disponíveis utilizando o **selecionar um modelo** menu pendente lista e, em seguida, clique em **OK**.  

    ![](media/azure-stack-create-vm-template/4.PNG)

5. Se quiser fazer modificações para o modelo JSON, pode fazê-lo, se não, ou quando terminar, clique em **guardar** para fechar a caixa de diálogo do modelo de edição.

    ![](media/azure-stack-create-vm-template/5.PNG) 

6.  Clique em **parâmetros**, preencha ou modificar os campos disponíveis conforme necessário e, em seguida, clique em **OK**. Escolha a subscrição que pretende utilizar, criar ou escolha um nome de grupo de recursos existente e, em seguida, clique em **criar** para iniciar a implementação do modelo.

    ![](media/azure-stack-create-vm-template/6.PNG)

7. Escolha a subscrição que pretende utilizar, criar ou escolha um nome de grupo de recursos existente e, em seguida, clique em **criar** para iniciar a implementação do modelo.

    ![](media/azure-stack-create-vm-template/7.PNG)

8. A implementação do grupo de recursos demora vários minutos para criar a VM baseada em modelo personalizada. Pode monitorizar o estado da instalação através de notificações e para as propriedades do grupo de recursos. 

    ![](media/azure-stack-create-vm-template/8.PNG)

    >[!NOTE]
    > A VM será executado quando a implementação estiver concluída. 

## <a name="start-minikube-and-install-an-application"></a>Iniciar minikube e instalar uma aplicação
Agora que a VM com Linux foi criada com êxito, pode iniciar sessão iniciar o minikube e instalar uma aplicação. 

1. Depois de concluída a implementação, clique em **Connect** para ver o endereço IP público que será utilizado para ligar à VM do Linux. 

    ![](media/azure-stack-create-vm-template/9.PNG)

2. A partir da linha de comandos elevada, execute **mstsc.exe** para abrir a ligação de ambiente de trabalho remoto e ligar ao endereço IP público da VM Linux detetado no passo anterior. Quando lhe for pedido para iniciar sessão no xRDP, utilize as credenciais que especificou quando criou a VM.

    ![](media/azure-stack-create-vm-template/10.PNG)

3. Abra o emulador do Terminal e introduza os seguintes comandos para iniciar minikube:

    ```shell
    sudo minikube start --vm-driver=none
    sudo minikube addons enable dashboard
    sudo minikube dashboard --url
    ```

    ![](media/azure-stack-create-vm-template/11.PNG)

4. Abra o navegador da web e visite o endereço de dashboard do Kubernetes. Parabéns, tem agora a funcionar totalmente instalação de Kubernetes com minikube!

    ![](media/azure-stack-create-vm-template/12.PNG)

5. Se gostaria de implementar uma aplicação de exemplo, visite a página de documentação oficial do kubernetes, ignore a secção "Criar o Cluster Minikube" como já tiver criado um acima. Simplesmente mergulhar de cabeça para a secção "Criar a sua aplicação node. js" na https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Saiba mais sobre modelos de início rápido do Azure Stack 
> * Criar uma VM com um modelo personalizado do GitHub
> * Iniciar minikube e instalar uma aplicação

