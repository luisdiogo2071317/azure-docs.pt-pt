---
title: Guia de introdução - Criar o seu primeiro contentor do Azure Container Instances com o portal do Azure
description: Neste início rápido, vai utilizar o portal do Azure para implementar um contentor no Azure Container Instances
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 6aa6fb27b2aa7c8b9614e5812fadc629b1e185f8
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076243"
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Início Rápido: crie o seu primeiro contentor no Azure Container Instances

O Azure Container Instances facilita a criação e a gestão de contentores do Docker no Azure, sem ter de aprovisionar as máquinas virtuais ou adotar um serviço de nível mais elevado. Neste início rápido, vai utilizar o portal do Azure criar um contentor no Azure e expõe-lo na Internet com nome de domínio completamente qualificado (FQDN). Depois de configurar algumas definições, irá ver isto no seu browser:

![Aplicação implementada com o Azure Container Instances vista no browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em https://portal.azure.com.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita][azure-free-account] antes de começar.

## <a name="create-a-container-instance"></a>Criar uma instância de contentor

Selecione **Criar um recurso** > **Contentores** >  **Container Instances**.

![Começar a criar uma nova instância do contentor no portal do Azure][aci-portal-01]

Introduza os seguintes valores nas caixas de texto **Nome do contentor**, **Imagem do contentor** e **Grupo de recursos**. Deixe os outros valores nas predefinições e, em seguida, selecione **OK**.

* Nome do contentor: `mycontainer`
* Imagem de contentor: `microsoft/aci-helloworld`
* Grupo de recursos: `myResourceGroup`

![Configurar definições básicas para uma nova instância do contentor no portal do Azure][aci-portal-03]

Pode criar contentores Windows e Linux no Azure Container Instances. Neste início rápido, deixe a predefinição de **Linux** para implementar a imagem `microsoft/aci-helloworld` baseada em Linux.

Em **Configuração**, especifique uma **Etiqueta de nome DNS** para o contentor. O nome tem de ser exclusivo na região do Azure em que cria a instância de contentor. O contentor estará publicamente acessível em `<dns-name-label>.<region>.azurecontainer.io`.

Deixe as outras predefinições da **Configuração** e, em seguida, selecione **OK** para validar a configuração.

![Configurar uma nova instância do contentor no portal do Azure][aci-portal-04]

Quando a validação estiver concluída, é apresentado um resumo das definições de contentor. Selecione **OK** para submeter o pedido de implementação do contentor.

![Resumo das definições para uma nova instância do contentor no portal do Azure][aci-portal-05]

Quando inicia a implementação, é apresentado um mosaico no dashboard do portal que indica que a implementação está em curso. Depois de implementada, o mosaico apresenta a nova instância de contentor.

![Progresso da criação de uma nova instância do contentor no portal do Azure][aci-portal-08]

Selecione a instância de contentor **mycontainer** para apresentar as respetivas propriedades. Tome nota do **FQDN** (nome de domínio completamente qualificado) da instância do contentor, bem como do **Estado**.

![Descrição geral do grupo de contentores no portal do Azure][aci-portal-06]

Quando o **Estado** for *Em execução*, navegue para o FQDN do contentor no seu browser.

![Aplicação implementada com o Azure Container Instances vista no browser][aci-portal-07]

Parabéns! Ao configurar apenas algumas definições, implementou uma aplicação acessível publicamente no Azure Container Instances.

## <a name="view-container-logs"></a>Ver registos de contentor

Ver os registos de uma instância de contentor é útil quando estiver a resolver problemas no contentor ou na aplicação nele executada.

Para ver os registos do contentor, em **DEFINIÇÕES**, selecione **Contentores** e, em seguida, **Registos**. Deverá ver o pedido HTTP GET gerado quando visualizou a aplicação no seu browser.

![Registos de contentor no portal do Azure][aci-portal-11]

## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado com o contentor, selecione **Descrição Geral** para a instância de contentor *mycontainer* e, em seguida, selecione **Eliminar**.

![Eliminar instância do contentor no portal do Azure][aci-portal-09]

Selecione **Sim** quando caixa de diálogo de confirmação for apresentada.

![Eliminar confirmação da instância do contentor no portal do Azure][aci-portal-10]

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou uma instância de contentor do Azure a partir de uma imagem no registo do Hub do Docker público. Se quiser criar uma imagem de contentor sozinho e implementá-lo no Azure Container Instances a partir de um registo de contentor do Azure, avance para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/