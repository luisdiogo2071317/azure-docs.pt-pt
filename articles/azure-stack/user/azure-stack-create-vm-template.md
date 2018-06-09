---
title: Neste tutorial, vai criar uma VM de pilha do Azure através de um modelo | Microsoft Docs
description: Descreve como utilizar o ASDK para criar uma VM com um modelo de predfined e um modelo personalizado do GitHub.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: e772dc41ce2cb77a03b91515cae35ffc48f5dbc3
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238502"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Tutorial: criar uma VM utilizando um modelo de Comunidade
Como um utilizador ou o operador de pilha do Azure, pode criar uma VM utilizando [modelos de início rápido do GitHub personalizados](https://github.com/Azure/AzureStack-QuickStart-Templates) em vez de implementar um manualmente do marketplace pilha do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Saiba mais sobre modelos de início rápido de pilha do Azure 
> * Criar uma VM utilizando um modelo personalizado do GitHub
> * Iniciar minikube e instalar uma aplicação

## <a name="learn-about-azure-stack-quickstart-templates"></a>Saiba mais sobre modelos de início rápido de pilha do Azure
Os modelos de início rápido de pilha do Azure são armazenados no [repositório público de modelos de início rápido AzureStack](https://github.com/Azure/AzureStack-QuickStart-Templates) no GitHub. Este repositório inclui modelos de implementação Azure Resource Manager que foi testados com o Microsoft Azure pilha Development Kit (ASDK). Pode utilizá-los para tornar mais fácil para si avaliar a pilha do Azure e utilizar o ambiente de ASDK. 

Ao longo do tempo muitos utilizadores GitHub têm contribuído para o repositório, resultando numa coleção enorme de mais de 400 modelos de implementação. Este repositório é um excelente ponto de partida para obter uma melhor compreensão sobre como pode implementar vários tipos de ambientes de pilha do Azure. 

>[!IMPORTANT]
> Alguns destes modelos são criados por membros da Comunidade e não pela Microsoft. Cada modelo está licenciado sob um contrato de licença pelo respetivo proprietário, não Microsoft. A Microsoft não é responsável por estes modelos e não avaliza segurança, compatibilidade ou desempenho. Comunidade modelos não são suportados em qualquer serviço ou programa de suporte da Microsoft e são disponibilizados "Tal como está", sem garantias de qualquer tipo.

Se pretender contribuir os modelos Azure Resource Manager para o GitHub, efetua a contribuição para o [repositório de modelos de início rápido do azure](https://github.com/Azure/AzureStack-QuickStart-Templates).

Para saber mais sobre o repositório do GitHub e como contribuir para o mesmo, consulte o [ficheiro Leia-me do repositório](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 


## <a name="create-a-vm-using-a-custom-github-template"></a>Criar uma VM utilizando um modelo personalizado do GitHub
Neste tutorial de exemplo, o [101-vm linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) modelo de início rápido do Azure pilha é utilizado para implementar uma máquina de virtual Ubuntu 16.04 no AzureStack Minikube para gerir um cluster de kubenetes a executar.

Minikube é uma ferramenta que torna mais fácil executar Kubernetes localmente. Minikube é executado um cluster de nó único Kubernetes dentro de uma VM para os utilizadores que querem experimentar Kubernetes ou desenvolver com o mesmo diárias. Suporta uma simples, o cluster de Kubernetes de um nó em execução numa VM com Linux. É a forma mais rápida e mais diretamente reencaminhar para obter um cluster de Kubernetes totalmente funcional, em execução. Permite aos programadores desenvolver e testar as respetivas implementações de aplicação baseada em Kubernetes nos respetivos computadores locais. Em termos de arquitetura, Minikube é executada a VM principal e componentes do nó de agente localmente:
- Componentes de nó principal como servidor de API, o programador e o etcd servidor são executados num único processo Linux denominado LocalKube.
- Componentes do nó de agente são executados no interior do docker contentores exatamente como executam num nó de agente normal. De um ponto de vista de implementação de aplicação, não há qualquer diferença quando a aplicação é implementada num Minikube ou regular Kubernetes cluster.

Este modelo instala os seguintes componentes:

- Ubuntu 16.04 LTS VM
- [Docker-CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> A imagem de VM com Ubuntu (Ubuntu Server 16.04 LTS neste exemplo) tem já foram adicionada para o mercado de pilha do Azure antes de iniciar estes passos.

1.  Clique em **+ novo** > **personalizada** > **implementação do modelo**.

    ![](media/azure-stack-create-vm-template/1.PNG) 

2. Clique em **Editar modelo**.

   ![](media/azure-stack-create-vm-template/2.PNG) 

3.  Clique em **modelo de início rápido**.

       ![](media/azure-stack-create-vm-template/3.PNG)

4. Selecione **101-vm linux-minikube** entre os modelos disponíveis utilizando o **selecionar um modelo** pendente lista e, em seguida, clique em **OK**.  

   ![](media/azure-stack-create-vm-template/4.PNG)

5. Se pretender efetuar modificações ao modelo JSON, pode fazê-lo, se não, ou quando terminar, clique em **guardar** para fechar a caixa de diálogo do modelo de edição.

   ![](media/azure-stack-create-vm-template/5.PNG) 

6.  Clique em **parâmetros**, preencha ou modificar os campos disponíveis conforme necessário e, em seguida, clique em **OK**. Selecione a subscrição para utilizar, criar ou escolha um nome de grupo de recursos existente e, em seguida, clique em **criar** para iniciar a implementação do modelo.

       ![](media/azure-stack-create-vm-template/6.PNG)

7. Selecione a subscrição para utilizar, criar ou escolha um nome de grupo de recursos existente e, em seguida, clique em **criar** para iniciar a implementação do modelo.

   ![](media/azure-stack-create-vm-template/7.PNG)

8. A implementação do grupo de recursos demora vários minutos a criar a VM com base no modelo personalizada. Pode monitorizar o estado da instalação através de notificações e para as propriedades do grupo de recursos. 

   ![](media/azure-stack-create-vm-template/8.PNG)

   >[!NOTE]
   > A VM será executado quando a implementação estiver concluída. 

## <a name="start-minikube-and-install-an-application"></a>Iniciar minikube e instalar uma aplicação
Agora que a VM com Linux foi criada com êxito, pode iniciar sessão para iniciar minikube e instalar uma aplicação. 

1. Depois de concluída a implementação, clique em **Connect** para ver o endereço IP público que será utilizado para ligar à VM do Linux. 

   ![](media/azure-stack-create-vm-template/9.PNG)

2. A partir da linha de comandos elevada, execute **mstsc.exe** para abrir as ligações de ambiente de trabalho remoto e ligar ao endereço IP público a VM com Linux detetado no passo anterior. Quando lhe for pedido para iniciar sessão no xRDP, utilize as credenciais especificadas ao criar a VM.

   ![](media/azure-stack-create-vm-template/10.PNG)

3. Abra o emulador de Terminal e introduza os seguintes comandos para iniciar minikube:

    >    `sudo minikube start --vm-driver=none`
    >   
    >    `sudo minikube addons enable dashboard`
    >    
    >    `sudo minikube dashboard --url`

   ![](media/azure-stack-create-vm-template/11.PNG)

4. Abra o browser e visite o endereço de dashboard kubernetes. Parabéns, tem agora uma totalmente kubernetes instalação utilizando minikube!

   ![](media/azure-stack-create-vm-template/12.PNG)

5. Se gostaria de implementar um exemplo de aplicação, visite a página de documentação oficial do kubernetes, ignorar a secção "Criar o Cluster Minikube", que já criou um acima. Basta ir para a secção "Criar a sua aplicação Node.js" em https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Saiba mais sobre modelos de início rápido de pilha do Azure 
> * Criar uma VM utilizando um modelo personalizado do GitHub
> * Iniciar minikube e instalar uma aplicação

