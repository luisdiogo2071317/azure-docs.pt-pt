---
title: Adicionar suporte de Cofre de chaves ao seu projeto ASP.NET com o Visual Studio | Microsoft Docs
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
ms.openlocfilehash: 9f0cc6ee06042948442aace05d56fcffa3742a8d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Adicionar o Cofre de chaves para a sua aplicação web utilizando o Visual Studio ligado Services

Neste tutorial, irá aprender como facilmente adicionar tudo o que necessita para começar a utilizar o Cofre de chaves do Azure para gerir os seus segredos para projetos web no Visual Studio, se estiver a utilizar o ASP.NET Core ou de qualquer tipo de projeto ASP.NET. Ao utilizar a funcionalidade de serviços ligados no Visual Studio 2017, pode ter o Visual Studio adicionam automaticamente todos os pacotes NuGet e definições de configuração para ligar ao Cofre de chaves no Azure. 

Para obter detalhes sobre as alterações que serviços ligados torna no seu projeto para ativar o Cofre de chaves, consulte [chave de cofre serviço ligado - o que aconteceu ao meu ASP.NET 4.7.1 projeto](vs-key-vault-aspnet-what-happened.md) ou [chave de cofre serviço ligado - o que aconteceu ao a minha projeto em ASP.NET Core](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Pré-requisitos

- **Uma subscrição do Azure**. Se não tiver uma conta, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versão 15.7** com o **desenvolvimento de Web** instalada da carga de trabalho. [Transferi-la agora](https://aka.ms/vsdownload).
- Para o ASP.NET (e não núcleo), terá das ferramentas de desenvolvimento do .NET Framework 4.7.1, que não estão instaladas por predefinição. Para instalá-los, inicie o instalador do Studio Visual, escolha **modificar**e, em seguida, escolha **componentes individuais**, em seguida, no lado direito, expanda **ASP.NET e web desenvolvimento**e escolha **ferramentas de desenvolvimento do .NET Framework 4.7.1**.
- Um projeto web de ASP.NET Core 2.0 aberto ou ASP.NET 4.7.1.

## <a name="add-key-vault-support-to-your-project"></a>Adicionar suporte de Cofre de chaves ao seu projeto

1. No **Explorador de soluções**, escolha **adicionar** > **serviço ligado**.
   É apresentada a página de serviço ligado com os serviços, que pode adicionar ao seu projeto.
1. No menu de serviços disponíveis, escolha **Secure segredos com o cofre do Azure chave**.

   ![Escolha "Segredos segurados com o Cofre de chaves do Azure"](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Se tiver sessão iniciada para o Visual Studio e tiver uma subscrição do Azure associada à sua conta, é apresentada uma página com uma lista de lista pendente com as suas subscrições.
1. Selecione a subscrição que pretende utilizar e, em seguida, escolha um cofre de chaves novas ou existentes ou escolha a ligação de editar para modificar o nome gerado automaticamente.

   ![Selecione a sua subscrição](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Escreva o nome que pretende utilizar para o Cofre de chaves.

   ![Mudar o nome do Cofre de chaves e escolha um grupo de recursos](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Selecione um grupo de recursos existente ou optar por criar uma nova com um nome de unqiue geradas automaticamente.  Se pretender criar um novo grupo com um nome diferente, pode utilizar o [Portal do Azure](https://portal.azure.com)e, em seguida, feche a página e reinicie recarregar a lista de grupos de recursos.
1. Escolha a região na qual pretende criar o Cofre de chaves. Se a sua aplicação web está alojada no Azure, escolha a região que aloja a aplicação web para um desempenho ideal.
1. Escolha um modelo de preços. Para obter mais informações, consulte [preços do Cofre de chave](https://azure.microsoft.com/pricing/details/key-vault/).
1. Escolha OK para aceitar as opções de configuração.
1. Escolha **adicionar** para criar o Cofre de chaves. O processo de criação poderá falhar se escolher um nome que já foi utilizado.  Se isto acontecer, utilize o **editar** ligação mudar o nome do Cofre de chaves e tente novamente.

   ![Adicionar serviço ligado ao projeto](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Agora, adicione um segredo no seu Cofre de chaves do Azure. Para obter o local certo no portal, clique na ligação para segredos gerir armazenados neste Cofre de chave. Se fechar a página ou o projeto, pode navegar para-lo no [portal do Azure](https://portal.azure.com) escolhendo **todos os serviços**, em **segurança**, escolha **doCofredechaves**, em seguida, selecione o Cofre de chaves que acabou de criar.

   ![Navegar para o portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Na secção do Cofre de chaves para a chave do cofre que criou, escolha **segredos**, em seguida, **gerar/importação**.

   ![Gerar/importação um segredo](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Introduza um segredo, tais como "MySecret" e atribua-lhe qualquer valor de cadeia como um teste, em seguida, escolha o **criar** botão.

   ![Criar um segredo](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (opcional) Introduza outro segredo, mas desta vez, coloque-o numa categoria ao nomear-"Segredos – MySecret". Esta sintaxe especifica uma categoria "Segredos" que contém um segredo "MySecret."
 
Agora, pode aceder aos seus segredos no código. Os passos seguintes são diferentes dependendo se está a utilizar o ASP.NET 4.7.1 ou ASP.NET Core.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Aceder aos seus segredos no código (projetos de ASP.NET Core)

1. No Visual Studio, no seu projeto ASP.NET Core, agora, pode referenciar estes segredos utilizando as seguintes expressões no código:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. Na página. cshtml, diga About.cshtml, adicione o @inject directiva perto da parte superior do ficheiro para definir uma variável pode utilizar para aceder à configuração do Cofre de chaves.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Como um teste, pode confirmar que o valor do segredo está disponível, apresentando-lo das páginas. Utilize @config para referenciar a variável de configuração.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Criar e executar a aplicação web, navegue para a página acerca e ver o valor de "segredo".

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Os segredos no código de acesso (ASP.NET 4.7.1 projetos)

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

Parabéns, agora tiver confirmado que a aplicação web pode utilizar o Cofre de chaves para aceder à segredos armazenados em segurança.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos. Isto elimina o Cofre de chaves e os recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste Início Rápido nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o desenvolvimento do Cofre de chaves ao ler o [Guia do programador do Cofre de chave](key-vault-developers-guide.md)