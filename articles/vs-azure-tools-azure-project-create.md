---
title: Criar um projeto do serviço cloud do Azure com o Visual Studio | Documentos da Microsoft
description: Saiba como criar um projeto do serviço cloud do Azure com o Visual Studio
services: visual-studio-online
author: ghogen
manager: douge
assetId: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: f41628c29f40443cc213892a0cc457eda110ebca
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055827"
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Criar um projeto do serviço cloud do Azure com o Visual Studio
As ferramentas do Azure para Visual Studio fornece um modelo de projeto que lhe permite criar um serviço cloud do Azure. Depois do projeto ter sido criado, o Visual Studio permite-lhe configurar, depurar e implementar o serviço em nuvem no Azure.

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a>Passos para criar um projeto do serviço cloud do Azure no Visual Studio
Esta secção explica como criar um projeto do serviço cloud do Azure no Visual Studio com um ou mais funções da web.  

1. Inicie o Visual Studio como administrador.

1. No menu principal, selecione **arquivo** > **New** > **projeto**.

1. Selecione **Cloud** do Visual c# ou Visual Basic nós do modelo de projeto e selecione **serviço Cloud do Azure** na lista de modelos.

    ![Novo serviço cloud do Azure](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Especifica que versão do .NET Framework que pretende utilizar para desenvolver o seu projeto.

1. Introduza um nome e a localização do seu projeto e um nome para a solução. 

1. Selecione **OK**.

1. Na **novo serviço de nuvem do Microsoft Azure** caixa de diálogo, selecione as funções de que pretende adicionar e selecione o botão de seta para a direita para adicioná-los à sua solução.

    ![Selecione as novas funções do serviço cloud do Azure](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. Para mudar o nome de uma função que adicionou, Paire o rato sobre a função no **novo serviço de nuvem do Microsoft Azure** caixa de diálogo e, no menu de contexto, selecione **mudar o nome**. Pode também mudar o nome de uma função em sua solução (no **Explorador de soluções**) depois que ele foi adicionado.

    ![Mudar o nome da função do serviço cloud do Azure](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

O projeto do Visual Studio do Azure tem associações para os projetos de função na solução. O projeto também inclui a *o ficheiro de definição de serviço* e *ficheiro de configuração do serviço*:

- **Ficheiro de definição de serviço** -define as definições de tempo de execução para a sua aplicação, incluindo a quais funções são necessárias, pontos finais e tamanho da máquina virtual. 
- **Ficheiro de configuração do serviço** -configura o número de instâncias de uma função são execução e os valores das definições definidas para uma função. 

Para obter mais informações sobre estes ficheiros, consulte [configuram as funções para um serviço cloud do Azure com o Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Passos Seguintes
- [Gerir funções em projetos de serviço cloud do Azure com o Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)
