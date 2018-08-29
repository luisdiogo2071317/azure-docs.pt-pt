---
title: Crie aplicações sem servidor com o Visual Studio | Documentos da Microsoft
description: Criar, implementar e gerir a sua primeira aplicação sem servidor com o Azure Logic Apps e as funções do Azure no Visual Studio
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.custom: vs-azure
ms.topic: article
ms.date: 08/01/2018
ms.openlocfilehash: a69c129d5ae1405462e3a54a24cd2edbad2a86a7
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126782"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Crie seu primeiro aplicativo sem servidor com o Azure Logic Apps e as funções do Azure - Visual Studio

Além disso, pode rapidamente desenvolver e implementar aplicações na cloud com os recursos e ferramentas sem servidor no Azure, tal como [do Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [as funções do Azure](../azure-functions/functions-overview.md). Este artigo mostra como começar a criar uma aplicação sem servidor, que utiliza uma aplicação lógica que chama uma função do Azure, no Visual Studio. Para saber mais sobre as soluções sem servidor no Azure, veja [do Azure sem servidor com as funções e Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

Para criar uma aplicação sem servidor no Visual Studio, precisa destes itens:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, [inscreva-se para obter uma conta do Azure gratuita](https://azure.microsoft.com/free/).

* [Visual Studio 2017](https://www.visualstudio.com/vs/) ou o Visual Studio 2015 – da Comunidade, Professional ou Enterprise

* [SDK do Microsoft Azure](https://azure.microsoft.com/downloads/) (2.9.1 ou posterior)

* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

* [Ferramentas do Azure Logic Apps para o Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) ou a [versão Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio)

  Pode transferir e instalar as ferramentas do Azure Logic Apps diretamente a partir do Visual Studio Marketplace, ou [Saiba como instalar esta extensão de dentro do Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
  Certifique-se de que reiniciar o Visual Studio depois de concluir a instalação. 

* [As ferramentas de núcleo das funções do Azure](https://www.npmjs.com/package/azure-functions-core-tools) para funções de depuração localmente

* Acesso à web ao utilizar o Estruturador da aplicação lógica incorporado no Visual Studio

  O estruturador precisa de uma ligação à Internet para criar recursos no Azure e ler as propriedades e os dados a partir de conectores na sua aplicação lógica. 
  Por exemplo, se utilizar o conector do Dynamics CRM Online, o estruturador verifica se a instância do CRM tem propriedades personalizadas e predefinidas disponíveis.

## <a name="create-resource-group-project"></a>Criar projeto do grupo de recursos

Para começar, crie uma [projeto do grupo de recursos do Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para a sua aplicação sem servidor. No Azure, criar recursos dentro de um grupo de recursos, o que é uma coleção lógica que utiliza para organizar, gerenciamento e implementação de recursos para uma aplicação completa como um recurso único. Para uma aplicação sem servidor no Azure, o seu grupo de recursos inclui recursos para o Azure Logic Apps e as funções do Azure. Saiba mais sobre [recursos e grupos de recursos do Azure](../azure-resource-manager/resource-group-overview.md).

1. Inicie o Visual Studio e inicie sessão com a sua conta do Azure. 

1. No menu **Ficheiro**, selecione **Novo** > **Projeto**. 

   ![Criar novo projeto no Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. Em **Instalado**, selecione **Visual C#** ou **Visual Basic**. Selecione **Cloud** > **Grupo de Recursos do Azure**.

   Se o **Cloud** categoria ou **grupo de recursos do Azure** projeto não existir, certifique-se de que instalou o SDK do Azure para Visual Studio.

1. Dar ao seu projeto, um nome e uma localização e, em seguida, escolha **OK**. 

   Visual Studio irá pedir-lhe selecionar um modelo. 
   Pode começar com um espaço em branco, a aplicação lógica ou a outro modelo, mas este exemplo utiliza um modelo de início rápido do Azure para a criação de uma aplicação sem servidor que inclui uma aplicação lógica e uma chamada para uma função do Azure.

   Para criar apenas uma aplicação lógica no Visual Studio, selecione o **aplicação lógica** modelo. Este modelo cria uma aplicação lógica vazia que se abre no Estruturador da aplicação lógica sem ter de pré-implementar sua solução num grupo de recursos do Azure.

1. Sob **Mostrar modelos a partir desta localização**, selecione **início rápido do Azure (github/Azure/azure--modelos de início rápido)**. 

1. Na caixa de pesquisa, introduza "aplicação lógica" como o filtro e selecione este modelo de início rápido sem servidor e escolha **OK**: **101-logic-app-and-function-app**

   ![Selecione o modelo de início rápido do Azure](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio cria e abre-se de uma solução para seu projeto do grupo de recursos. 
   O modelo de início rápido que selecionou cria um modelo de implementação com o nome `azuredeploy.json` dentro de seu projeto do grupo de recursos. 
   Este modelo de implementação inclui a definição para uma aplicação lógica simples que aciona numa solicitação HTTP, chama uma função do Azure e devolve o resultado como uma resposta HTTP. 
   
   ![Nova solução sem servidor](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. Em seguida, tem de implementar sua solução no Azure antes de poder abrir o modelo de implementação e reveja os recursos para a sua aplicação sem servidor. 

## <a name="deploy-your-solution"></a>Implemente a sua solução

Antes de pode abrir a aplicação lógica com o Estruturador da aplicação lógica no Visual Studio, tem de ter um grupo de recursos do Azure que já tenha sido implementado no Azure. O designer, em seguida, pode criar ligações para recursos e serviços na sua aplicação lógica. Para essa tarefa, implemente a sua solução do Visual Studio para o portal do Azure.

1. No Solution Explorer, abra o menu de atalho do seu projeto de recursos e, em seguida, selecione **Deploy** > **New**.

   ![Criar nova implementação para o grupo de recursos](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Se não estiver selecionado, selecione a sua subscrição do Azure e o grupo de recursos para onde pretende implementar. Escolher **implementar**.

   ![Definições de implementação](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Se o **Editar parâmetros** caixa apresentada, forneça o nome de recurso a utilizar para a aplicação lógica e a aplicação de funções do Azure na implementação e, em seguida, guarde as suas definições. Certifique-se de que utilizar um nome globalmente exclusivo para a sua aplicação de função.

   ![Fornecer nomes para a aplicação lógica e a aplicação de funções](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Quando o Visual Studio inicia a implementação para o seu grupo de recurso especificado, o estado da implementação da sua solução é apresentada no Visual Studio **saída** janela. 
   Após a conclusão da implementação, a aplicação lógica fica ativa no portal do Azure.

## <a name="edit-logic-app-in-visual-studio"></a>Editar a aplicação lógica no Visual Studio

Agora que a sua solução for implementada para o grupo de recursos, abra a aplicação lógica com o Estruturador da aplicação lógica para que possa editar e alterar a sua aplicação lógica.

1. No Explorador de soluções, abra a `azuredeploy.json` menu de atalho do ficheiro e em seguida, selecione **abrir com o Estruturador da aplicação lógica**.

   ![Abrir "azuredeploy. JSON" no Estruturador da aplicação lógica](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Depois do **propriedades da aplicação lógica** é apresentada a caixa e, se ainda não estiver selecionado, em **subscrição**, selecione a sua subscrição do Azure. Sob **grupo de recursos**, selecione o grupo de recursos e o local onde implementou a sua solução e, em seguida, escolha **OK**.

   ![Propriedades da aplicação lógica](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Depois de abrir o Estruturador da aplicação lógica, pode continuar a adicionar passos ou alterar o fluxo de trabalho e guardar as atualizações.

   ![Aplicação de lógica aberto no Estruturador da aplicação lógica](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Criar projeto de funções do Azure

Para criar o seu projeto de funções e a função com JavaScript, Python, F #, PowerShell, Batch ou Bash, siga os passos no artigo [trabalhar com as ferramentas de núcleo de funções do Azure](../azure-functions/functions-run-local.md). Para desenvolver a sua função do Azure com c# dentro de sua solução, pode utilizar uma biblioteca de classes do c# ao seguir os passos no artigo [publicar uma biblioteca de classes do .NET como uma aplicação de funções](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/).

## <a name="deploy-functions-from-visual-studio"></a>Implementar as funções a partir do Visual Studio

O modelo de implementação implementa todas as funções do Azure que tem na sua solução do repositório Git que é especificado por meio de variáveis no `azuredeploy.json` ficheiro. Se cria e cria o projeto de funções na sua solução, pode verificar esse projeto no controle de origem do Git, por exemplo, o GitHub ou o Visual Studio Team Services e, em seguida, atualize o `repo` variável para que o modelo implementa a função do Azure.

## <a name="manage-logic-apps-and-view-run-history"></a>Gerir aplicações lógicas e ver o histórico de execuções

No logic apps já implementadas no Azure, pode ainda editar, gerir, ver histórico de execuções e desativar essas aplicações a partir do Visual Studio. 

1. Do **View** menu no Visual Studio, abra **Cloud Explorer**. 

1. Sob **todas as subscrições**, selecione a subscrição do Azure associada às aplicações lógicas que pretende gerir e escolha **aplicar**.

1. Sob **Logic Apps**, selecione a sua aplicação lógica. No menu de atalho essa aplicação, selecione **aberto com o Editor de aplicação lógica**. 

Agora pode transferir a aplicação lógica já publicadas para o seu projeto do grupo de recursos. Portanto, embora possam iniciar uma aplicação lógica no portal do Azure, ainda pode importar e gerir essa aplicação no Visual Studio. Para obter mais informações, consulte [gerir aplicações lógicas com o Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Passos Seguintes

* [Criar um dashboard de rede social sem servidor](logic-apps-scenario-social-serverless.md)
* [Gerir aplicações lógicas com o Visual Studio](manage-logic-apps-with-visual-studio.md)
* [Linguagem de definição de fluxo de trabalho de aplicação lógica](logic-apps-workflow-definition-language.md)