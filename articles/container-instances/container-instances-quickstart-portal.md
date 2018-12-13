---
title: Início rápido - executar uma aplicação no Azure Container Instances - Portal
description: Neste início rápido, vai utilizar o portal do Azure para implementar uma aplicação de contentor do Docker para executar num contentor isolado no Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 8f547977e544854e281e1c6be442607d55149e5e
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53190293"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-in-the-azure-portal"></a>Início rápido: Execute uma aplicação de contentor no Azure Container Instances no portal do Azure

Utilize o Azure Container Instances para executar contentores de Docker no Azure com simplicidade e celeridade. Não precisa de implementar máquinas virtuais nem de utilizar uma plataforma de orquestração de contentores completa, como o Kubernetes. Neste início rápido, vai utilizar o portal do Azure para criar um contentor no Azure e disponibilizar a sua aplicação com um nome de domínio completamente qualificado (FQDN). Depois de configurar algumas definições e implementar o contentor, pode navegar para a aplicação em execução:

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
* Grupo de recursos: **Criar um novo** > `myResourceGroup`

![Configurar definições básicas para uma nova instância do contentor no portal do Azure][aci-portal-03]

Pode criar contentores Windows e Linux no Azure Container Instances. Neste início rápido, deixe a predefinição de **Linux** para implementar a imagem `microsoft/aci-helloworld` baseada em Linux.

Em **Configuração**, especifique uma **Etiqueta de nome DNS** para o contentor. O nome tem de ser exclusivo na região do Azure em que cria a instância de contentor. O contentor estará publicamente acessível em `<dns-name-label>.<region>.azurecontainer.io`.

Deixe as outras predefinições da **Configuração** e, em seguida, selecione **OK** para validar a configuração.

![Configurar uma nova instância do contentor no portal do Azure][aci-portal-04]

Quando a validação estiver concluída, é apresentado um resumo das definições de contentor. Selecione **OK** para submeter o pedido de implementação do contentor.

![Resumo das definições para uma nova instância do contentor no portal do Azure][aci-portal-05]

Quando inicia a implementação, é apresentado uma notificação que indica que a implementação está em curso. É apresentada outra notificação quando o grupo de contentores tiver sido implementado.

![Progresso da criação de uma nova instância do contentor no portal do Azure][aci-portal-08]

Aceda à descrição geral do grupo de contentores ao navegar até **Grupos de Recursos** > **myResourceGroup** > **mycontainer**. Tome nota do **FQDN** (nome de domínio completamente qualificado) da instância do contentor, bem como do **Estado**.

![Descrição geral do grupo de contentores no portal do Azure][aci-portal-06]

Quando o **Estado** for *Em execução*, navegue para o FQDN do contentor no seu browser.

![Aplicação implementada com o Azure Container Instances vista no browser][aci-portal-07]

Parabéns! Ao configurar apenas algumas definições, implementou uma aplicação acessível publicamente no Azure Container Instances.

## <a name="view-container-logs"></a>Ver registos de contentor

Ver os registos de uma instância de contentor é útil quando estiver a resolver problemas no contentor ou na aplicação nele executada.

Para ver os registos do contentor, em **Definições**, selecione **Contentores** e, em seguida, **Registos**. Deverá ver o pedido HTTP GET gerado quando visualizou a aplicação no seu browser.

![Registos de contentor no portal do Azure][aci-portal-11]

## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado com o contentor, selecione **Descrição Geral** para a instância de contentor *mycontainer* e, em seguida, selecione **Eliminar**.

![Eliminar instância do contentor no portal do Azure][aci-portal-09]

Selecione **Sim** quando caixa de diálogo de confirmação for apresentada.

![Eliminar confirmação da instância do contentor no portal do Azure][aci-portal-10]

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou uma instância de contentor do Azure a partir de uma imagem no registo do Hub do Docker público. Se deseja criar uma imagem do contentor e implementá-la partir de um registo de contentor privado do Azure, prossiga para o tutorial do Azure Container Instances.

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