---
title: 'Tutorial: Utilize o detetor do logotipo personalizado para reconhecer os serviços do Azure - visão personalizada'
titlesuffix: Azure Cognitive Services
description: Neste tutorial, irá percorrer uma aplicação de exemplo que utiliza a imagem digitalizada personalizado do Azure como parte de um cenário de deteção de logótipo. Saiba como a visão personalizada com outros componentes é usado para fornecer um aplicativo ponto a ponto.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pafarley
ms.openlocfilehash: 520e1981544f6ad0a6f1412a47dc8ea50fdd53a5
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773148"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Tutorial: Reconhecer logótipos de serviço do Azure nas imagens da câmara

Neste tutorial, irá percorrer uma aplicação de exemplo que utiliza a imagem digitalizada personalizado do Azure como parte de um cenário maior. A aplicação de aprovisionamento de IA Visual, uma aplicação xamarin. Forms para plataformas móveis, analisa imagens da câmara de logotipos do serviço do Azure e, em seguida, implementa o serviço real para a conta de utilizador do Azure. Aqui aprenderá como ele usa visão personalizada em coordenação com outros componentes para fornecer um aplicativo útil de ponto-a-ponto. Pode pretender executar a aplicação toda para si próprio, ou pode simplesmente preencha a parte de visão personalizada da configuração e explorar a forma como a aplicação utiliza ele.

Este tutorial irá mostrar-lhe como:

> [!div class="checklist"]
> - Criar um detector de objeto personalizado para reconhecer logotipos do serviço do Azure
> - Ligar a sua aplicação de imagem digitalizada do Azure e de visão personalizada
> - Criar uma conta do principal de serviço do Azure para implementar serviços do Azure a partir da aplicação

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar. 

## <a name="prerequisites"></a>Pré-requisitos

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- Carga de trabalho do Xamarin para Visual Studio (veja [instalar o Xamarin](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- IOS ou emulador do Android para o Visual Studio
- [Interface de linha de comandos (CLI) do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (opcional)

## <a name="get-the-source-code"></a>Obter o código-fonte

Se gostaria de utilizar a aplicação web fornecido, clonar ou transferir o código-fonte da aplicação do [aprovisionar de IA Visual](https://github.com/Microsoft/AIVisualProvision) repositório no GitHub. Abra o *Source/VisualProvision.sln* ficheiro no Visual Studio. Mais tarde em diante, irá tornar as edições necessárias para alguns dos ficheiros de projeto para que seja possível executar a aplicação.

## <a name="create-an-object-detector"></a>Criar um detector de objeto

Inicie sessão para o [Web site de visão personalizada](https://customvision.ai/) e crie um novo projeto. Especifique um projeto de deteção de objetos e utilizar o domínio de logótipo; Isso permitirá que o serviço para utilizar um algoritmo otimizado para a deteção de logótipo. 

![janela de caixa de diálogo novo projeto no site da visão personalizada no browser do Chrome](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Carregar e etiquetar imagens

Em seguida, terá de preparar o algoritmo de deteção de logótipo ao carregar imagens de logotipos do serviço do Azure e marcá-los manualmente. O repositório de AIVisualProvision inclui um conjunto de imagens de formação que pode utilizar. No Web site, selecione o **adicionar imagens** botão no **imagens de formação** separador e, em seguida, navegue para o **documentos/imagens/Training_DataSet** pasta do repositório. Tem de marcar manualmente logótipos em cada imagem, portanto, se estiver apenas a testar este projeto, pode pretender carregar apenas um subconjunto das imagens. No mínimo, tem de carregar 15 instâncias de cada etiqueta que pretende utilizar.

Depois de carregar as imagens de formação, selecione a primeira na exibição. Será apresentada a janela de etiquetagem. Desenhar caixas e atribuir etiquetas para cada logótipo em cada imagem. 

![imagem de logotipos com as etiquetas que está a ser aplicadas no site da visão personalizada](media/azure-logo-tutorial/tag-logos.png)

A aplicação está configurada para trabalhar com cadeias de caracteres de etiqueta específica; consulte as definições do *Source\VisualProvision\Services\Recognition\RecognitionService.cs* ficheiro:

[!code-csharp[tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?range=18-33)]

Quando já marcou uma imagem, navegue para a direita para marcar a próxima Sílaba. Quando tiver terminado, sair fora da janela de etiquetagem.

## <a name="train-the-object-detector"></a>Preparar o detetor de objeto

No painel da esquerda, defina o **etiquetas** mudar para **Tagged**, e deve ver todas as suas imagens. Em seguida, clique no botão verde na parte superior da página para preparar o modelo. Isto irá ensiná-o algoritmo para reconhecer as etiquetas mesmo em novas imagens. Ele também irá testar o modelo em algumas das suas imagens existentes para gerar as pontuações de precisão.

![o site de visão personalizada, no separador imagens de formação; o botão de comboio é descrito](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Obter o URL de predição

Assim que o seu modelo é preparado, está pronto para integrá-la na sua aplicação. Para tal, terá de obter o URL de ponto de extremidade (o endereço do seu modelo, o que a aplicação irá consultar) e a chave de previsão (para conceder o acesso de aplicação para pedidos de predição). Na **desempenho** separador, clique nas **URL de predição** botão na parte superior da página.

![o site de visão personalizada com uma tela de API de predição, que mostra um URL de chave de api e o endereço](media/azure-logo-tutorial/cusvis-endpoint.png)

Copie o URL do ficheiro de imagem e o `Prediction-key` valor para os campos adequados na *Source\VisualProvision\AppSettings.cs* ficheiro:

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=22-26)]

## <a name="examine-custom-vision-usage"></a>Examine a utilização de visão personalizada

Abra o *Source/VisualProvision/Services/Recognition/CustomVisionService.cs* arquivo para ver como o URL de chave e o ponto final de visão personalizada são usados na aplicação. O **PredictImageContentsAsync** método assume um fluxo de bytes de um ficheiro de imagem, juntamente com um cancelamento token (para gestão de tarefa assíncrona), chama a API de predição de visão personalizada e devolve o resultado da predição. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?range=12-28)]

Este resultado assume a forma de um **PredictionResult** instância, que por si só contém uma lista de **predição** instâncias. R **predição** contém uma etiqueta detetada e a localização da caixa delimitadora na imagem.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?range=3-12)]

Se quiser saber mais sobre como a aplicação processa esses dados, iniciar o **GetResourcesAsync** método, definido no *Source/VisualProvision/Services/Recognition/RecognitionService.cs* ficheiro. 

## <a name="add-computer-vision"></a>Adicionar imagem digitalizada

A parte de visão personalizada do tutorial é concluída, mas se quiser executar a aplicação, terá de integrar o serviço de visão do computador também. A aplicação utiliza a funcionalidade de reconhecimento de imagem digitalizada texto para complementar o processo de deteção de logótipo; um logótipo do Azure pode ser reconhecido pela sua aparência _ou_ pelo texto impresso perto do telefone. Ao contrário dos modelos de visão personalizada, de imagem digitalizada previamente está preparada para executar determinadas operações em imagens ou vídeos.

Simplesmente subscreva o serviço de visão do computador para obter um URL de chave e o ponto final. Ver [como obter chaves de subscrição](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe) se precisar de ajuda com este passo.

![o serviço de visão do computador no portal do Azure, com o menu de início rápido marcado; uma ligação para as chaves fica realçada, como é o URL de ponto final de API](media/azure-logo-tutorial/comvis-keys.png)

Em seguida, abra a *Source\VisualProvision\AppSettings.cs* de ficheiros e preencher o `ComputerVisionEndpoint` e `ComputerVisionKey` variáveis com os valores corretos.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=28-32)]


## <a name="create-a-service-principal"></a>Criar um principal de serviço

A aplicação requer uma conta do principal de serviço do Azure para implementar serviços à sua subscrição do Azure. Um principal de serviço permite-lhe delegar permissões específicas a uma aplicação com o controlo de acesso baseado em funções. Consulte a [guiam de principais de serviço](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals) se quiser saber mais.

Pode criar um principal de serviço com o Azure Cloud Shell ou a CLI do Azure (como a seguir). Primeiro, inicie sessão e selecione a subscrição que pretende utilizar.

```console
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Em seguida, crie o seu principal de serviço (tenha em atenção que poderá demorar algum tempo para concluir).

```console
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

Após a conclusão com êxito, deverá ver o seguinte JSON de saída que contém as credenciais necessárias.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```
Anote o `clientId`, e `tenantId` valores. Adicioná-los para os campos adequados na *Source\VisualProvision\AppSettings.cs* ficheiro.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=8-16)]

## <a name="run-the-app"></a>Executar a aplicação

Neste momento, que forneceu o acesso de aplicação para:
* um modelo de visão personalizada preparado
* o serviço de visão do computador
* uma conta do principal de serviço 

Siga estes passos para executar a aplicação:

1. No Explorador de soluções do Visual Studio, selecione o VisualProvision.Android ou VisualProvision.iOS projeto e escolha um emulador correspondente ou o dispositivo móvel ligado no menu pendente na barra de ferramentas principal (Observe que precisa de um dispositivo MacOS para executar um emulador de iOS). Em seguida, execute a aplicação.

1. Na primeira tela que carrega, introduza o seu ID de cliente do principal de serviço, o ID de inquilino e a palavra-passe. Clique nas **início de sessão** botão.

    > [!NOTE]
    > Em alguns emuladores, o **início de sessão** botão não pode ativar este passo. Se isto acontecer, pare a aplicação, abra a _Source/VisualProvision/Pages/LoginPage.xaml_ do ficheiro, encontre o `Button` elemento assinaladas como o botão de início de sessão e remova a linha:
      ```xaml
      IsEnabled="{Binding IsValid}"
      ```
    
    > Em seguida, execute novamente a aplicação.

    ![o ecrã da aplicação com campos para credenciais do principal de serviço](media/azure-logo-tutorial/app-credentials.png)

1. No ecrã seguinte, selecione a sua subscrição do Azure no menu suspenso (isso deve conter todas as subscrições a que o seu principal de serviço tem acesso). Clique nas **continuar** botão.

    ![o ecrã da aplicação com um campo de lista pendente de subscrição do Azure de destino](media/azure-logo-tutorial/app-az-subscription.png)

    Neste momento a aplicação poderá pedir-lhe conceder acesso aos dispositivos para o armazenamento de câmera e fotos. Aceite estas permissões.

1. Em seguida, irá ativar a câmara no seu dispositivo. Tire uma fotografia de um dos logótipos de serviço do Azure que é preparado. Uma caixa de diálogo de implantação deve pedir-lhe para selecionar um região e grupo de recursos para os novos serviços (como faria se estivesse implantando-las no portal do Azure). 

    ![uma tela de câmera de smartphone com dois cutouts de documento de logotipos do Azure no modo de exibição](media/azure-logo-tutorial/app-camera-capture.png)

    ![um ecrã de aplicação com campos de entrada de grupo de recursos e região de implementação](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Limpar recursos 

Se tiver seguido todos os passos deste cenário e utilizado a aplicação para implementar serviços do Azure à sua conta, certifique-se de que vá para o [portal do Azure](https://ms.portal.azure.com/) quando tiver terminado e cancelar os serviços que não pretende utilizar.

Além disso, se planeja criar projeto de deteção do próprio objeto com visão personalizada no futuro, pode querer eliminar o projeto de deteção do logótipo que criou neste tutorial. Uma avaliação gratuita de visão personalizada permite que dois projetos. Na [Web site de visão personalizada](https://customvision.ai), navegue até à **projetos** e selecione o caixote do lixo pode ser executados sob **meu novo projeto**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, vai configurar e explorou uma aplicação xamarin. Forms com todas as funcionalidades que utiliza o serviço de visão personalizada para detetar logótipos nas imagens da câmara móveis. Em seguida, conheça as práticas recomendadas para a criação de um modelo de visão personalizada, para que quando cria uma para a sua própria aplicação pode torná-lo poderosa e precisas.

> [!div class="nextstepaction"]
> [Como melhorar o seu classificador](getting-started-improving-your-classifier.md)