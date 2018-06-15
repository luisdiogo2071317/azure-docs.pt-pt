---
title: Introdução à chave de cofre ligado serviço no Visual Studio (projetos de ASP.NET) | Microsoft Docs
description: Utilize este tutorial para o ajudar a saber como adicionar suporte de Cofre de chaves para uma aplicação de web do ASP.NET ou ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: cd305801f10c899682aa6d751e48f30b6e8303fa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787403"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Introdução à chave de cofre ligado serviço no Visual Studio (projetos de ASP.NET)

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-what-happened.md)

Este artigo fornece orientação adicional depois de adicionar o Cofre de chaves para o projeto MVC do ASP.NET através de **adicionar serviços ligados** comando no Visual Studio. Se ainda não adicionou o serviço ao seu projeto, pode fazê-em qualquer altura ao seguir as instruções no [Cofre de chaves de adicionar à sua aplicação web utilizando o Visual Studio ligado Services](vs-key-vault-add-connected-service.md).

Consulte [o que aconteceu ao meu projeto ASP.NET?](vs-key-vault-aspnet-core-what-happened.md) para que as alterações efetuadas ao seu projeto ao adicionar o serviço ligado.

## <a name="after-you-connect"></a>Depois de ligar

1. Adicione um segredo no seu Cofre de chaves do Azure. Para obter o local certo no portal, clique na ligação para **gerir segredos armazenados neste Cofre de chave**. Se fechar a página ou o projeto, pode navegar para-lo no [portal do Azure](https://portal.azure.com) escolhendo **todos os serviços**, em **segurança**, escolha **doCofredechaves**, em seguida, selecione o Cofre de chaves que acabou de criar.

   ![Navegar para o portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Na secção do Cofre de chaves para a chave do cofre que criou, escolha **segredos**, em seguida, **gerar/importação**.

   ![Gerar/importação um segredo](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Introduza um segredo, tais como **MySecret**e atribua-lhe qualquer valor de cadeia como um teste, em seguida, escolha o **criar** botão.

   ![Criar um segredo](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (opcional) Introduza outro segredo, mas este tempo, coloque-o numa categoria ao nomear- **segredos – MySecret**. Esta sintaxe especifica uma categoria **segredos** que contenha um segredo **MySecret**.

1. Modificar o Web. config da seguinte forma. As chaves são marcadores de posição que serão substituídos pelo AzureKeyVault ConfigurationBuilder com os valores dos segredos no Cofre de chaves.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. HomeController, no método de controlador acerca, adicione as seguintes linhas ao obter o segredo e armazene-o a ViewBag.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. Na vista de About.cshtml, adicione o seguinte para apresentar o valor do segredo (para apenas teste).

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Parabéns, agora ativou a sua aplicação web utilizar o Cofre de chaves para aceder a segredos armazenados em segurança.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos. Isto elimina o Cofre de chaves e os recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste Início Rápido nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

# <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como desenvolver com o Cofre de chaves no [Guia do programador do Cofre de chave](key-vault-developers-guide.md)