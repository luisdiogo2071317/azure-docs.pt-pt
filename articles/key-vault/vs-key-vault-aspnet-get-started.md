---
title: Introdução ao serviço de ligado de Cofre de chaves no Visual Studio (projetos do ASP.NET) | Documentos da Microsoft
description: Utilize este tutorial para ajudá-lo adicionar suporte de Cofre de chaves num aplicativo da web ASP.NET ou ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 3ca62d47d8e7682c80985bf5409b8540382fbf45
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056119"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Introdução ao serviço de ligado de Cofre de chaves no Visual Studio (projetos do ASP.NET)

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-what-happened.md)

Este artigo fornece orientação adicional depois de adicionar o Key Vault para um projeto de MVC do ASP.NET através da **adicionar serviços ligados** comando no Visual Studio. Se ainda não tiver adicionado o serviço ao seu projeto, pode fazê-lo em qualquer altura ao seguir as instruções em [adicione o Key Vault para a sua aplicação web com o Visual Studio ligado Services](vs-key-vault-add-connected-service.md).

Ver [o que aconteceu ao meu projeto ASP.NET?](vs-key-vault-aspnet-core-what-happened.md) as alterações feitas ao seu projeto ao adicionar o serviço ligado.

## <a name="after-you-connect"></a>Depois de ligar

1. Adicione um segredo no Cofre de chaves no Azure. Para obter o lugar certo, no portal, clique no link para **gerir segredos armazenados neste Cofre de chave**. Se fechou a página ou o projeto, pode navegar para ele no [portal do Azure](https://portal.azure.com) escolhendo **todos os serviços**, em **segurança**, escolha **doKeyVault**, em seguida, selecione o Cofre de chaves que acabou de criar.

   ![Navegar para o portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Na secção Key Vault para a chave do cofre que criou, escolha **segredos**, em seguida, **gerar/importar**.

   ![Gerar/importar um segredo](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Introduza um segredo, tal como **MySecret**e dê a ele qualquer valor de cadeia de caracteres como um teste, em seguida, escolha a **criar** botão.

   ![Criar um segredo](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (opcional) Introduza outro segredo, mas desta vez colocá-la numa categoria ao nomeá-lo **segredos – MySecret**. Essa sintaxe especifica uma categoria **segredos** que contém um segredo **MySecret**.

1. Modificar Web. config da seguinte forma. As chaves são marcadores de posição que vão ser substituídas pelo AzureKeyVault ConfigurationBuilder com os valores dos segredos no Key Vault.

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

1. Em HomeController, no método de controlador About, adicione as seguintes linhas para recuperar o segredo e armazená-la na ViewBag.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. Na vista de carregarei, adicione o seguinte para apresentar o valor do segredo (apenas para teste).

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Parabéns, agora ativou a sua aplicação web utilizar o Key Vault para aceder aos segredos armazenados em segurança.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos. Esta ação elimina o Cofre de chaves e os recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste Início Rápido nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

# <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o desenvolvimento com o Key Vault no [Guia do programador do Cofre de chave](key-vault-developers-guide.md)