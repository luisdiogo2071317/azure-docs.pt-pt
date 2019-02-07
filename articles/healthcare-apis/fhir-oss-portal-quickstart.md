---
title: Implementar o servidor de Open FHIR de origem para o Azure através do Portal do Azure - APIs de cuidados de saúde Microsoft
description: Este início rápido explica como implementar o servidor de Microsoft Open origem FHIR através do Portal do Azure.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 2bd7a7d0e332d281a0ca9a9017219b50a3fbb472
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823538"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-portal"></a>Início rápido: Implementar o servidor Open FHIR de origem com o portal do Azure

Neste início rápido, irá aprender como implantar um servidor de FHIR de código fonte aberto no Azure com o portal do Azure. Iremos utilizar ligações de facilitar a implementação no [repositório de código-fonte aberto](https://github.com/Microsoft/fhir-server)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="github-open-source-repository"></a>Repositório de código aberto do GitHub

Navegue para o [página de implementação do GitHub](https://github.com/Microsoft/fhir-server/blob/master/docs/DefaultDeployment.md) e localize os botões de "Implementar no Azure":

![Página de implementação de código-fonte aberto](media/quickstart-oss-portal/deployment-page-oss.png)

Clique no botão de implementação e abre o portal do Azure.

## <a name="fill-in-deployment-parameters"></a>Preencha os parâmetros de implementação

Opte por criar um novo grupo de recursos e atribua um nome. Apenas outro parâmetro necessário é um nome para o serviço.

![Parâmetros de implementação personalizada](media/quickstart-oss-portal/deployment-custom-parameters.png)

Tenha em atenção que a implementação irá extrair o código-fonte diretamente a partir do repositório de código-fonte aberto no GitHub. Caso tenha escolhido o repositório, pode apontar para a sua própria para e de um ramo específico.

Depois de preencher os detalhes, pode iniciar a implementação.

## <a name="validate-fhir-server-is-running"></a>Validar FHIR servidor está em execução

Assim que a implementação estiver concluída, pode apontar seu navegador para `https://SERVICENAME.azurewebsites.net/metadata` para obter uma instrução de capacidade. Irá demorar um minuto ou para o servidor responda pela primeira vez.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o grupo de recursos e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém os recursos aprovisionados, selecione **eliminar grupo de recursos**, em seguida, confirme o nome do grupo de recursos para eliminar.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou o Microsoft Open FHIR servidor de origem para o Azure na sua subscrição. Para saber como aceder à API de FHIR com o Postman, avance para o tutorial do Postman.
 
>[!div class="nextstepaction"]
>[Aceda à API de FHIR com o Postman](access-fhir-postman-tutorial.md)