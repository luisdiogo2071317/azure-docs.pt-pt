---
title: "Guia de introdução - Criar o seu primeiro contentor do Azure Container Instances com o portal do Azure"
description: "Implementar e começar a utilizar o Azure Container Instances"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 01/02/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 16e726d10b159d0a3f08f8bca197c675d748c764
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Criar o seu primeiro contentor no Azure Container Instances

O Azure Container Instances permite criar e gerir facilmente contentores no Azure. Neste guia de introdução, cria um contentor no Azure e expõe-no na Internet com um endereço IP público. Esta operação foi concluída com o portal do Azure. Com apenas alguns cliques, verá isto no seu browser:

![Aplicação implementada com o Azure Container Instances vista no browser][aci-portal-07]

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-container-instance"></a>Criar uma instância de contentor

Selecione **Novo** > **Contentores** > **Azure Container Instances (pré-visualização)**.

![Começar a criar uma nova instância do contentor no portal do Azure][aci-portal-01]

Introduza os seguintes valores nas caixas de texto **Nome do contentor**, **Imagem do contentor** e **Grupo de recursos**. Deixe os outros valores nas predefinições e, em seguida, clique em **OK**.

* Nome do contentor: `mycontainer`
* Imagem de contentor: `microsoft/aci-helloworld`
* Grupo de recursos: `myResourceGroup`

![Configurar definições básicas para uma nova instância do contentor no portal do Azure][aci-portal-03]

Pode criar contentores Windows e Linux no Azure Container Instances. Neste guia de introdução, iremos deixar a predefinição do **Linux**, uma vez que especificámos um contentor baseado em Linux (`microsoft/aci-helloworld`) no passo anterior.

Deixe as outras predefinições da **Configuração** e, em seguida, clique em **OK** para validar a configuração.

![Configurar uma nova instância do contentor no portal do Azure][aci-portal-04]

Quando a validação é concluída, é apresentado um resumo das definições de contentor. Selecione **OK** para submeter o pedido de implementação do contentor.

![Resumo das definições para uma nova instância do contentor no portal do Azure][aci-portal-05]

Quando inicia a implementação, é colocado um mosaico no dashboard do portal que indica o progresso da implementação. Uma vez concluída a implementação, o mosaico é atualizado para apresentar o seu novo grupo de contentores **mycontainer-myc1**.

![Progresso da criação de uma nova instância do contentor no portal do Azure][aci-portal-08]

Selecione o grupo de contentores **mycontainer-myc1** para apresentar as propriedades do grupo do contentor. Tome nota do **endereço Ip** do grupo de contentores, bem como o **ESTADO** do seu contentor.

![Descrição geral do grupo de contentores no portal do Azure][aci-portal-06]

Assim que o contentor é movido para o estado **Em execução**, navegue para o endereço IP que anotou no passo anterior para apresentar a aplicação alojada no novo contentor.

![Aplicação implementada com o Azure Container Instances vista no browser][aci-portal-07]

## <a name="delete-the-container"></a>Eliminar o contentor
Quando tiver terminado com o contentor, selecione o grupo de contentor **mycontainer-myc1** e, em seguida, clique em **Eliminar**.

![Eliminar instância do contentor no portal do Azure][aci-portal-09]

Isto irá iniciar uma caixa de diálogo de confirmação, selecione **Sim** quando lhe for pedido.

![Eliminar confirmação da instância do contentor no portal do Azure][aci-portal-10]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução criou uma Instância do Contentor do Azure a partir de uma imagem num repositório do Docker Hub público. Se quiser tentar criar um contentor sozinho e implementá-lo no Azure Container Instances com o Azure Container Registry, avance para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Azure Container Instances tutorials](./container-instances-tutorial-prepare-app.md) (Tutoriais do Azure Container Instances)