---
title: "Criar uma aplicação Web ASP.NET Framework no Azure | Microsoft Docs"
description: "Saiba como executar aplicações Web no serviço de aplicações do Azure com a implementação da aplicação Web ASP.NET predefinida."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 04a1becf-7756-4d4e-92d8-d9471c263d23
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/30/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: b4cde427115df5bb7cd80acd676c6788ff3a379e
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="create-an-aspnet-framework-web-app-in-azure"></a>Criar uma aplicação Web ASP.NET Framework no Azure

[As Aplicações Web do Azure](app-service-web-overview.md) fornecem um serviço de alojamento na Web altamente dimensionável e com correção automática.  Este guia de início rápido mostra como implementar uma aplicação Web ASP.NET em aplicações Web do Azure. Quando terminar, terá um grupo de recursos que consiste num plano de serviços de aplicações e uma aplicação Web do Azure com uma aplicação Web implementada.

Assista ao vídeo para ver este guia de introdução em ação e, em seguida, siga os passos para publicar a sua primeira aplicação .NET no Azure.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* Instale o <a href="https://www.visualstudio.com/downloads/" target="_blank">2017 do Visual Studio</a> com as seguintes cargas de trabalho:
    - **Desenvolvimento do ASP.NET e Web**
    - **Desenvolvimento do Azure**

    ![Desenvolvimento do ASP.NET e Web e desenvolvimento do Azure (na Web e na nuvem)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Criar uma aplicação Web ASP.NET

No Visual Studio, crie um projeto ao selecionar **Ficheiro > Novo > Projeto**. 

Na caixa de diálogo **Novo projeto**, clique em **Visual C# > Web > Aplicação Web ASP.NET (.NET Framework)**.

Atribua à aplicação o nome _myFirstAzureWebApp_ e clique em **OK**.
   
![Caixa de diálogo Novo Projeto](./media/app-service-web-get-started-dotnet-framework/new-project.png)

Pode implementar qualquer tipo de aplicação Web ASP.NET no Azure. Neste guia de início rápido, selecione o modelo **MVC** e confirme se a autenticação está definida como **Sem Autenticação**.
      
Selecione **OK**.

![Caixa de diálogo Novo Projeto ASP.NET](./media/app-service-web-get-started-dotnet-framework/select-mvc-template.png)

No menu, selecione **Depurar > Iniciar sem depuração** para executar a aplicação Web localmente.

![Executar a aplicação localmente](./media/app-service-web-get-started-dotnet-framework/local-web-app.png)

## <a name="publish-to-azure"></a>Publicar no Azure

No **Explorador de Soluções**, clique com o botão direito do rato no projeto **myFirstAzureWebApp** e selecione **Publicar**.

![Publicar a partir do Explorador de Soluções](./media/app-service-web-get-started-dotnet-framework/solution-explorer-publish.png)

Certifique-se de que o **Serviço de Aplicações do Microsoft Azure** está selecionado e selecione **Publicar**.

![Publicar a partir da página de descrição geral do projeto](./media/app-service-web-get-started-dotnet-framework/publish-to-app-service.png)

É aberta a caixa de diálogo **Criar Serviço de Aplicações**, que o ajuda a criar todos os recursos do Azure necessários para executar a sua aplicação Web ASP.NET no Azure.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Na caixa de diálogo **Criar Serviço de Aplicações**, selecione **Adicionar uma conta** e inicie sessão na sua subscrição do Azure. Se já tem sessão iniciada, selecione a conta que contém a subscrição pretendida na lista pendente.

> [!NOTE]
> Se já tiver sessão iniciada, não selecione ainda **Criar**.
>
>
   
![Iniciar sessão no Azure](./media/app-service-web-get-started-dotnet-framework/sign-in-azure.png)

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Junto a **Grupo de recursos**, selecione **Novo**.

Atribua ao grupo de recursos o nome **myResourceGroup** e selecione **OK**.

## <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Junto a **Plano do Serviço de Aplicações**, selecione **Novo**. 

Na caixa de diálogo **Configurar Plano do Serviço de Aplicações**, use as definições na tabela a seguir à captura de ecrã.

![Criar plano do App Service](./media/app-service-web-get-started-dotnet-framework/configure-app-service-plan.png)

| Definição | Valor sugerido | Descrição |
|-|-|-|
|Plano do Serviço de Aplicações| myAppServicePlan | Nome do plano de serviço de aplicações. |
| Localização | Europa Ocidental | O centro de dados onde o a aplicação Web está alojada. |
| Tamanho | Gratuito | O [escalão de preço](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) determina as funcionalidades do alojamento. |

Selecione **OK**.

## <a name="create-and-publish-the-web-app"></a>Criar e publicar a aplicação Web

No **Nome da Aplicação Web**, escreva um nome único para a aplicação (os carateres válidos são `a-z`, `0-9` e `-`) ou aceite o nome exclusivo gerado automaticamente. O URL da aplicação Web é `http://<app_name>.azurewebsites.net`, onde `<app_name>` é o nome da aplicação Web.

Selecione **Criar** para começar a criar os recursos do Azure.

![Configurar o nome da aplicação Web](./media/app-service-web-get-started-dotnet-framework/web-app-name.png)

Depois de concluir o assistente, este publica a aplicação Web ASP.NET no Azure e, em seguida, inicia a aplicação no browser predefinido.

![Aplicação Web ASP.NET publicada no Azure](./media/app-service-web-get-started-dotnet-framework/published-azure-web-app.png)

O nome da aplicação Web especificado no [passo para criar e publicar](#create-and-publish-the-web-app) é utilizado como o prefixo de URL no formato `http://<app_name>.azurewebsites.net`.

Parabéns, a primeira aplicação Web ASP.NET está em execução em direto no Serviço de Aplicações do Azure.

## <a name="update-the-app-and-redeploy"></a>Atualizar a aplicação e reimplementar

No **Explorador de Soluções**, abra _Views\Home\Index.cshtml_.

Localize a etiqueta HTML `<div class="jumbotron">` na parte superior e substitua todo o elemento pelo código seguinte:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Para voltar a implementar no Azure, clique com o botão direito do rato no projeto **myFirstAzureWebApp**, no **Explorador de Soluções** e selecione **Publicar**.

Na página de publicação, selecione **Publicar**.

Quando a publicação estiver concluída, o Visual Studio inicia um browser para o URL da aplicação Web.

![Aplicação Web ASP.NET atualizada no Azure](./media/app-service-web-get-started-dotnet-framework/updated-azure-web-app.png)

## <a name="manage-the-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao <a href="https://portal.azure.com" target="_blank">portal do Azure</a> para gerir a aplicação Web.

No menu à esquerda, selecione **Serviços Aplicacionais** e selecione o nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/app-service-web-get-started-dotnet-framework/access-portal.png)

É apresentada a página de descrição geral da sua aplicação Web. Aqui, pode realizar tarefas de gestão básicas, como navegar, parar, iniciar, reiniciar e eliminar. 

![Painel Serviço de Aplicações no portal do Azure](./media/app-service-web-get-started-dotnet-framework/web-app-blade.png)

O menu à esquerda fornece diferentes páginas para configurar a sua aplicação. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [ASP.NET com Base de Dados SQL](app-service-web-tutorial-dotnet-sqldatabase.md)
