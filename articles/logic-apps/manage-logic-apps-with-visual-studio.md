---
title: Gerir as logic apps com o Visual Studio - Azure Logic Apps | Microsoft Docs
description: Gerir as logic apps e outros recursos do Azure com o Visual Studio Cloud Explorer
author: ecfan
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.date: 03/15/2018
ms.author: estfan; LADocs
ms.openlocfilehash: b4d7f557923a67ae0c9fc513cd2b4fe7555241be
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301121"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Gerir as logic apps com o Visual Studio

Embora possa criar, editar, gerir e implementar as logic apps no <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, também pode utilizar o Visual Studio quando pretender adicionar logic apps para controlo de origem, publicar versões diferentes e criar [recursos do Azure Gestor de](../azure-resource-manager/resource-group-overview.md) modelos para ambientes de implementação diferentes. Com o Visual Studio Cloud Explorer, pode localizar e gerir as logic apps, juntamente com outros recursos do Azure. Por exemplo, pode abrir, transfira, editar, executar, ver o histórico de execução, desativar e ativar as logic apps que já são implementadas no portal do Azure. Se estiver a trabalhar com Azure Logic Apps no Visual Studio, saiba [como criar aplicações lógicas com o Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Implementar ou publicar uma aplicação lógica a partir do Visual Studio substitui a versão dessa aplicação no portal do Azure. Por isso, se efetuar alterações no portal do Azure que pretende manter, certifique-se de que a [atualizar a aplicação lógica no Visual Studio](#refresh) do portal do Azure antes da próxima vez que implementar ou publicação a partir do Visual Studio.

<a name="requirements"></a>

## <a name="prerequisites"></a>Pré-requisitos

* Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>.

* Transfira e instale estas ferramentas, se ainda não as tiver: 

  * <a href="https://www.visualstudio.com/downloads" target="_blank">Visual Studio 2017 ou Visual Studio 2015 – edição Community ou superior</a>. 
  Este início rápido utiliza o Visual Studio Community 2017, que é gratuito.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">SDK do Azure (2.9.1 ou posterior)</a> e <a href="https://github.com/Azure/azure-powershell#installation" target="_blank">Azure PowerShell</a>

  * <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551" target="_blank">Ferramentas do Azure Logic Apps para o Visual Studio 2017</a> ou a <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio" target="_blank">versão Visual Studio 2015</a> 
  
    Pode transferir e instalar as Ferramentas do Azure Logic Apps diretamente a partir do Visual Studio Marketplace ou saiba <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">como instalar esta extensão a partir do Visual Studio</a>. 
    Certifique-se de que reinicia o Visual Studio depois de concluir a instalação.

* Acesso Web ao utilizar o Designer de aplicações lógicas incorporados

  O estruturador precisa de uma ligação à Internet para criar recursos no Azure e ler as propriedades e os dados a partir de conectores na sua aplicação lógica. 
  Por exemplo, se utilizar o conector do Dynamics CRM Online, o estruturador verifica se a instância do CRM tem propriedades personalizadas e predefinidas disponíveis.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Localizar as logic apps

No Visual Studio, pode encontrar todas as aplicações lógicas que estão associados a sua subscrição do Azure e são implementadas no portal do Azure utilizando o Explorador de nuvem.

1. Abra o Visual Studio. No **vista** menu, selecione **Cloud Explorer**.

2. No Cloud Explorer, escolha **gestão de contas**. Selecione a subscrição do Azure associada com as logic apps, em seguida, escolha **aplicar**. Por exemplo:

   ![Escolha "Gestão de contas"](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

2. Com base em se está a procurar por **grupos de recursos** ou **tipos de recursos**, siga estes passos:

   * **Grupos de recursos**: na sua subscrição do Azure, o Cloud Explorer apresenta todos os grupos de recursos que estão associados essa subscrição. 
   Expanda o grupo de recursos que contém a sua aplicação lógica, em seguida, selecione a sua aplicação lógica.

   * **Tipos de recursos**: na sua subscrição do Azure, expanda **Logic Apps**. Depois de Cloud Explorer mostra todas as aplicações implementadas lógica que estão associadas a sua subscrição, selecione a sua aplicação lógica.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Abrir no Visual Studio

No Visual Studio, pode abrir as logic apps anteriormente criada e implementada diretamente através do portal do Azure ou como projetos do Azure Resource Manager com o Visual Studio.

1. Abra o Explorador de nuvem e localizar a sua aplicação lógica. 

2. No menu de atalho da aplicação lógica, selecione **aberta com o Editor de aplicação lógica**.

   Este exemplo mostra as logic apps por tipo de recurso, pelo que as logic apps são apresentados no **Logic Apps** secção.

  ![Aplicação de lógica de implementado abertos do portal do Azure](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Depois da aplicação lógica abre no Designer de aplicações lógicas, na parte inferior do designer, pode escolher **vista de código** para que pode rever a estrutura de definição de aplicação lógica subjacente. 
   Se pretender criar um modelo de implementação para a aplicação lógica, saiba [como transferir um modelo Azure Resource Manager](#download-logic-app) para essa aplicação lógica. Saiba mais sobre [modelos do Resource Manager](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Transferir a partir do Azure

Pode transferir as logic apps do <a href="https://portal.azure.com" target="_blank">portal do Azure</a> e guardá-las como [do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) modelos. Pode, em seguida, localmente editar modelos com o Visual Studio e personalizar as logic apps para ambientes de implementação diferentes. Transferir automaticamente as logic apps *parameterizes* as respetivas definições dentro [modelos do Resource Manager](../azure-resource-manager/resource-group-overview.md#template-deployment), que também utilizar JavaScript Object Notation (JSON).

1. No Visual Studio, abra o Explorador de nuvem, em seguida, localize e selecione a aplicação lógica que pretende transferir a partir do Azure.

2. No menu de atalho essa aplicação, selecione **aberta com o Editor de aplicação lógica**.

   O Designer de aplicação lógica abre-se e mostra a aplicação lógica. 
   Para rever a aplicação de lógica subjacente e de estrutura, na parte inferior do designer, escolha **vista de código**. 

3. Na barra de ferramentas estruturador, escolha **transferir**.

   ![Selecione "Transferir"](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Quando lhe for pedida uma localização, navegue até à localização e guardar o modelo do Resource Manager para a definição da aplicação lógica no formato de ficheiro JSON. (JSON). 

A definição da aplicação lógica aparece no `resources` subsecção dentro do modelo do Resource Manager. Agora pode editar a definição da aplicação lógica e modelo do Resource Manager com o Visual Studio. Também pode adicionar o modelo como um projeto do Azure Resource Manager para uma solução do Visual Studio. Saiba mais sobre [projetos do Gestor de recursos para aplicações lógicas no Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Atualizar a partir do Azure

Se editar a sua aplicação lógica no portal do Azure e pretender manter essas alterações, certifique-se que Atualize a versão dessa aplicação no Visual Studio com essas alterações. 

* No Visual Studio, na barra de ferramentas Designer de aplicação lógica, selecione **atualizar**.

  -ou-

* No Visual Studio Cloud Explorer, abra o menu de atalho da sua aplicação lógica e selecione **atualizar**. 

![Atualizar aplicação lógica com atualizações](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Publicar atualizações da aplicação lógica

Quando estiver pronto para implementar as atualizações de aplicação lógica a partir do Visual Studio para o Azure, na barra de ferramentas Designer de aplicação lógica, escolha **publicar**.

![Publicar a aplicação lógica atualizado](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Executar manualmente a sua aplicação lógica

Pode acionar manualmente uma aplicação lógica implementada no Azure a partir do Visual Studio. Na barra de ferramentas Designer de aplicação lógica, escolha **executar acionador**.

![Executar manualmente a aplicação lógica](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Rever o histórico de execuções

Para verificar o estado e diagnosticar problemas com a execução de aplicação lógica, pode rever os detalhes, tais como entradas e saídas, para aqueles que é executado no Visual Studio.

1. No Cloud Explorer, abra o menu de atalho da sua aplicação lógica e selecione **histórico de execução Open**.

   ![Abra histórico de execução](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

2. Para ver os detalhes para uma execução específico, faça duplo clique em execução. Por exemplo:

   ![Histórico de execução de detalhado](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Para ordenar a tabela pela propriedade, escolha o cabeçalho de coluna para essa propriedade. 

3. Expanda os passos cujo entradas e saídas pretende rever. Por exemplo:

   ![Ver entradas e saídas de cada passo](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Desativar ou ativar a aplicação lógica

Sem eliminar a sua aplicação lógica, pode parar o acionador de acionando da próxima vez que o se a condição de acionamento for cumprida. Desativar a sua aplicação lógica, impede que o motor de Logic Apps de criar e executar instâncias de fluxo de trabalho futura para a sua aplicação lógica.
No Cloud Explorer, abra o menu de atalho da sua aplicação lógica e selecione **desativar**.

![Desativar a sua aplicação lógica](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

Quando estiver pronto para a sua aplicação lógica retomar a operação, pode reativar a sua aplicação lógica. No Cloud Explorer, abra o menu de atalho da sua aplicação lógica e selecione **ativar**.

![Ativar a sua aplicação lógica](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Eliminar a aplicação lógica

Ao eliminar a sua aplicação lógica do portal do Azure, no Cloud Explorer, abra o menu de atalho da sua aplicação lógica e selecione **eliminar**.

![Eliminar a aplicação lógica](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a gerir aplicações lógicas implementado com o Visual Studio. Em seguida, saiba mais sobre como personalizar definições da aplicação lógica para a implementação:

> [!div class="nextstepaction"]
> [Definições de aplicação de lógica de autor no JSON](../logic-apps/logic-apps-author-definitions.md)
