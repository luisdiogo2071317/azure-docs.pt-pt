---
title: Introdução à API de associação de entidade | Microsoft Docs
description: Analise o texto e a ligação com o nome de entidades para as entradas relevantes na base de dados de conhecimento utilizando a API de associação de entidade nos serviços de Cognition.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: entity-linking-intelligence
ms.topic: article
ms.date: 07/06/2016
ms.author: davl
ms.openlocfilehash: 54c4a3bbb3637c248bd7705ed291633368b542c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351770"
---
# <a name="get-started-with-entity-linking-api-in-c35"></a>Introdução à API na C de ligação de entidade&#35;

Associar a entidade da Microsoft é uma ferramenta de processamento de linguagem natural para analisar o texto e entidades com o nome de ligação para as entradas relevantes na base de dados de conhecimento. 

Este tutorial explicar entidade de ligação utilizando a biblioteca de clientes de associação de entidade como um pacote NuGet. 

### <a name="Prerequisites">Pré-requisitos</a>

- Visual Studio 2015
- Chave de API de serviços de um Microsoft cognitivos
- Instalar o cliente de biblioteca e de exemplo
- Pacote NuGet de associar a entidade da Microsoft

Poderá transferi-lo a entidade de ligação Intelligence serviço API biblioteca de clientes através de [SDK](https://www.github.com/microsoft/cognitive-entitylinking-windows). O ficheiro zip transferido tem de ser extraídos para uma pasta à sua escolha, número de utilizadores que escolha a pasta do Visual Studio 2015.

### <a name="step-1-subscribe-entity-linking-intelligence-service-and-get-your-own-key">Passo 1: Subscrever o serviço do Asset Intelligence de associação de entidade e obter a chave</a>
Antes de utilizar o serviço do entidade de ligação do Asset Intelligence, deve inscrever-se para uma chave de API. Consulte [subscrições](https://www.microsoft.com/cognitive-services/en-us/sign-up). Tanto a chave primária e secundária pode ser utilizada neste tutorial.

### <a name="step-2-create-a-new-project-in-visual-studio"> Passo 2: Criar um novo projeto no Visual Studio</a>

Vamos começar por criar um novo projeto no Visual Studio. Em primeiro lugar, inicie o Visual Studio 2015, no menu Iniciar. Em seguida, crie um novo projeto selecionando **instalado → modelos → Visual c# → aplicação em branco Windows Universal →** para o modelo de projeto:

 ![Aplicação universal Createa](./Images/CreateUWP.png)

### <a name="step-3-add-the-entity-linking-nuget-package-to-your-project">Passo 3: Adicionar o pacote do entidade de ligação NuGet ao seu projeto</a>

Entidade de ligação de serviços cognitivos for lançado como um pacote de NuGet.org e tem de ser instalado para poder utilizá-lo.
Para o adicionar ao seu projeto, vá para o **Explorador de soluções** separador, clique com o botão direito do rato em projeto e selecione **gerir pacotes Nuget**.

Primeiro, no **Gestor de pacotes NuGet** janela, selecione NuGet.org como seu **origem do pacote** no canto superior direito. Selecione **procurar** no canto superior esquerdo e, na caixa de pesquisa, escreva "ProjectOxford.EntityLinking". Selecione o **Microsoft.ProjectOxford.EntityLinking** NuGet do pacote e clique em **instalar**.

Em seguida, procure newtonsoft e instalar. Se lhe for pedido para rever as alterações, clique em **OK**. Se é apresentada com os termos de licenciamento EntityLinking, clique em **aceito**.

Entidade Linking está agora instalado como parte da sua aplicação. Pode confirmar isto verificando que o * * Microsoft.ProjectOxford.EntityLinking** referência está presente como parte do seu projeto no Explorador de soluções.

 ![Biblioteca do nuget incluída no projeto](./Images/NugetLibraryInProject.png)
 
### <a name="step-4-add-an-input-and-output-text-block-to-your-apps-xaml">Passo 4: Adicionar uma entrada e saída do bloco de texto em XAML da sua aplicação</a>
Navegue para * * MainPage.xaml * * no **Explorador de soluções**, em seguida, faça duplo clique no ficheiro que será aberto numa nova janela. Para sua comodidade, pode fazer duplo clique em de **XAML** clique no botão no **Designer** separador, esta será a ocultar a **Visual Designer** e reservar todo o espaço para a vista de código.

 ![Biblioteca do nuget incluída no projeto](./Images/UWPMainPage.png)
 
Como um serviço de texto, a melhor forma de visualizar a funcionalidade está a criar uma entrada e de um bloco de texto de saída. Para tal, adicione o XAML seguinte no **grelha**. Este código adiciona três componentes, uma caixa de texto de entrada, um bloco de texto de saída e um botão de início.
 
 ```XAML
 <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <TextBox x:Name="inputBox" Grid.Row="0" TextWrapping="Wrap" Text="Enter a paragraph" Margin="10" AcceptsReturn="True" />
    <TextBlock x:Name="outputBlock" Grid.Row="1" TextWrapping="Wrap" Text="Result will be here" Margin="10" />
    <Button x:Name="button" Grid.Row="2" Content="Get Result" />
</Grid>
 ```
 
### <a name="step-5-proceed-to-add-entity-linking-intelligence-service">Passo 5: Continuar para adicionar o serviço do entidade de ligação do Asset Intelligence</a>
 
A interface de utilizador está agora criada. Antes de utilizar o serviço de associação de entidade, precisamos de adicionar o processador de clique no botão. Abra **MainPage.xaml** de **Explorador de soluções**. Adiciona um processador de button_Click no final do botão.
 
 ```XAML
 <Button x:Name="button" Grid.Row="2" Content="Get Result" Click="button_Click" />
 ```
 
Um processador de clique no botão tem de ser implementado no código. Abra **MainPage.xaml.cs** de **Explorador de soluções** para implementar o clique no botão. O EntityLinkingServiceClient é um wrapper para obter respostas de entidade de ligação. O argumento de construtor de EntityLinkingServiceClient é a chave de subscrição de serviços cognitivos. Cole a chave de subscrição que obteve no **passo 1** para chamar o serviço de associação de entidade. 

Abaixo é o código de exemplo, o qual adiciona "wikipediaId" para a resposta utilizando o serviço de associação de entidade. 
 
 ```csharp
 private async void button_Click(object sender, RoutedEventArgs e)
{
    var text = this.inputBox.Text;
    var client = new EntityLinkingServiceClient("Your subscription key","https://api.labs.cognitive.microsoft.com");
    var linkResponse = await client.LinkAsync(text);
    var result = string.Join(", ", linkResponse.Select(i => i.WikipediaID).ToList());
    this.outputBlock.Text = result;
}
 ```
 
Agora, está pronto para executar a sua primeira linguagem natural a processar a aplicação de associação de entidade. Prima a **tecla F5** para compilar e executar a aplicação. Colar fragmentos de texto ou parágrafos na caixa de entrada. Clique no botão "Obter Resultado" e observe as entidades identificadas no bloco de saída.
 
 ![Resultado de exemplo de UWP](./Images/DemoCodeResult.png)
 
### <a name="summary">Resumo</a>
 
Neste tutorial aprendeu como criar uma aplicação para tirar partido de entidade de ligação Intelligence serviço biblioteca de clientes com apenas algumas linhas do c# e código XAML. 

