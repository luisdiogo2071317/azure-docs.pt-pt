---
title: Encontrar salas disponíveis com o Azure Digital Twins (C#) | Microsoft Docs
description: Neste início rápido, vai executar dois exemplos de aplicações .NET Core para enviar telemetria simulada de movimento e dióxido de carbono para um espaço no Azure Digital Twins. O objetivo é encontrar salas disponíveis com ar fresco a partir das APIs de Gestão após o processamento calculado na cloud.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/02/2018
ms.author: alinast
ms.openlocfilehash: e0b47b1322a520ad8b09fd2fe2967e628b5e4e03
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322874"
---
# <a name="quickstart-find-available-rooms-using-azure-digital-twins"></a>Início Rápido: Encontrar salas disponíveis com o Azure Digital Twins

O serviço Azure Digital Twins permite-lhe recriar uma imagem digital do seu ambiente físico. Depois, pode ser notificado por eventos no ambiente e personalizar as respostas para esses eventos. 

Este início rápido utiliza [um par de exemplos de .NET](https://github.com/Azure-Samples/digital-twins-samples-csharp) para digitalizar um edifício de escritórios imaginário e mostra-lhe como encontrar salas disponíveis nesse edifício. Com o Digital Twins, pode associar vários sensores ao seu ambiente. Juntamente com a disponibilidade das salas, também pode saber se a qualidade do ar de uma sala livre é ideal, com a ajuda de um sensor de dióxido de carbono simulado. Um dos exemplos de aplicações gerará dados de sensor aleatórios para o ajudar a visualizar este cenário.

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma conta do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As duas aplicações de consola que vai executar neste início rápido são escritas com C#. Tem de instalar a [versão 2.1.403 ou superior do SDK de .NET Core](https://www.microsoft.com/net/download) no computador de desenvolvimento. Se tiver o SDK de .NET Core instalado, pode executar `dotnet --version` numa janela de comandos para confirmar a versão atual do C# no computador de desenvolvimento.

1. Transferir o [exemplo de projeto em C#](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) e extrair o arquivo digital-twins-samples-csharp-master.zip. 


## <a name="create-a-digital-twins-instance"></a>Criar uma instância do Digital Twins

Utilize os passos nesta secção para criar uma instância nova do Digital Twins no [portal](https://portal.azure.com).

[!INCLUDE [create-digital-twins-portal](../../includes/create-digital-twins-portal.md)]

## <a name="set-permissions-for-your-app"></a>Definir permissões para a aplicação

Esta secção regista o exemplo de aplicação no Azure Active Directory (AAD), para que possa aceder à instância do Digital Twins. Se já tiver um registo de aplicação do AAD, pode reutilizá-lo no exemplo. Para tal, confirme que está configurado conforme descrito nesta secção. 

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="build-application"></a>Compilar a aplicação

Compile a aplicação de ocupação com os passos seguintes:

1. Abra uma linha de comandos e navegue para a pasta na qual os ficheiros digital-twins-samples-csharp-master.zip foram extraídos.
1. Execute `cd occupancy-quickstart/src`.
1. Execute `dotnet restore`.
1. Edite *appSettings.json* para atualizar as variáveis seguintes:
    - *ClientId*: introduza o *ID da Aplicação* do seu registo de aplicação do AAD, indicado na secção anterior.
    - *Tenant*: introduza o *ID do Diretório* do seu inquilino do AAD, também indicado na secção anterior.
    - *BaseUrl*: o URL da *API de Gestão* da sua instância do Digital Twins, que estará no formato `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Substitua os marcadores de posição neste URL pelos valores da sua instância, indicados na secção anterior.

## <a name="provision-graph"></a>Aprovisionar o gráfico

Este passo aprovisiona o gráfico espacial do Digital Twins com vários espaços, um dispositivo, dois sensores, uma função personalizada e uma atribuição de função. O gráfico espacial é aprovisionado com o ficheiro [*provisionSample.yaml*](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

1. Execute `dotnet run ProvisionSample`.
    >[!NOTE]
    >Para autenticar o utilizador no Azure AD, utilizamos a ferramenta da CLI do Azure Device Login. O utilizador tem de introduzir um determinado código para se autenticar através da página de [início de sessão da Microsoft](https://microsoft.com/devicelogin). Após a introdução do código, siga os passos para se autenticar. Quando a ferramenta estiver em execução, é pedido ao utilizador que se autentique sempre.
    
    >[!TIP]
    > Se receber o erro seguinte ao realizar este passo, confirme que copiou as variáveis corretamente. 
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`


1. O passo de aprovisionamento pode demorar alguns minutos. Também aprovisionará um Hub IoT na sua instância do Digital Twins e entrará em ciclo até que o estado do Hub IoT seja `Running`.

    ![Exemplo de aprovisionamento][4]

1. No final da execução, copie `ConnectionString` do dispositivo para utilização no exemplo do simulador de dispositivo. Copie apenas a cadeia realçada na imagem abaixo:

    ![Exemplo de aprovisionamento][1]

## <a name="send-sensor-data"></a>Enviar dados do sensor

Pode compilar e executar a aplicação de simulador de sensor com os passos abaixo:

1. Abra uma linha de comandos nova e navegue para o projeto que transferiu, na pasta digital-twins-samples-csharp-master.
1. Execute `cd device-connectivity`.
1. Execute `dotnet restore`.
1. Edite *appsettings.json* para atualizar *DeviceConnectionString* com `ConnectionString` acima.
1. Execute `dotnet run` para começar a enviar os dados do sensor. Deverá ver os dados serem enviados para o serviço Digital Twins conforme ilustrado na imagem seguinte:

     ![Conectividade do dispositivo][2]

1. Deixe o simulador ser executado, para que possa ver os resultados lado a lado com a ação do próximo passo. Esta janela mostra os dados do sensor simulado enviados para o Digital Twins e o próximo passo vai consultar em tempo para encontrar salas disponíveis com ar fresco.

    >[!TIP]
    > Se receber o erro seguinte ao realizar este passo, confirme que `DeviceConnectionString` foi copiado corretamente.  
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Encontrar espaços disponíveis com ar fresco

O exemplo de sensor está a simular valores de dados aleatórios para dois sensores, um de movimento e outro de dióxido de carbono. No exemplo, os espaços disponíveis com ar fresco são definidos como estando vazios e como tendo um nível de dióxido de carbono inferior a 1000 ppm. Se a condição não for satisfeita, o espaço não está disponível ou a qualidade do ar é má.

1. Abra a linha de comandos que utilizou para executar o passo de aprovisionamento acima.
1. Execute `dotnet run GetAvailableAndFreshSpaces`.
1. Veja esta linha de comandos e a linha de comandos dos dados do sensor lado a lado, conforme descrito abaixo. 
1. Uma linha de comandos envia os dados simulados de movimento e de dióxido de carbono para o Digital Twins de cinco em cinco segundos. O outro comando lê o gráfico em tempo real para encontrar salas disponíveis com ar fresco, tendo por base os dados simulados aleatórios. Apresentará uma das condições seguintes em tempo quase real com base nos últimos dados do sensor enviados:
    - Salas disponíveis com ar fresco.
    - Sala ocupada ou com má qualidade de ar.

     ![Obter espaços disponíveis com ar fresco][3]

Para compreender o que aconteceu neste início rápido e que APIs foram chamadas, abra o [Visual Studio Code](https://code.visualstudio.com/Download) com o projeto de área de trabalho de código em digital-twins-samples-csharp (veja os comandos abaixo). Os tutoriais aprofundam o código e ensinam-lhe a modificar os dados de configuração e as APIs que são chamadas. Para obter mais informações sobre as APIs de Gestão, navegue para a página Swagger do Digital Twins, `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net//management/swagger`, ou, para sua comodidade, navegue para [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger). 

```
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

## <a name="clean-up-resources"></a>Limpar recursos

Os tutoriais mostram detalhadamente como compilar uma aplicação que permite aos gestores de instalações aumentar a produtividade da ocupação e gerir o edifício de forma mais eficiente.

Se quiser avançar para os tutoriais, não limpe os recursos criados neste Início Rápido. Se não planear continuar, utilize os passos seguintes para eliminar todos os recursos criados neste Início Rápido:

1. Elimine a pasta que foi criada quando transferiu o exemplo de repositório.
1. No menu do lado esquerdo do [portal do Azure](http://portal.azure.com), clique em **Todos os recurso** e selecione o seu recurso do Digital Twins. Na parte superior do painel **Todos os recursos**, clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Este início rápido mostrou-lhe uma descrição geral de um cenário simples para encontrar salas com boas condições de trabalho. Para ver uma análise mais aprofundada deste cenário, avance para o tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Deploy Azure Digital Twins and configure a spatial graph](tutorial-facilities-setup.md) (Tutorial: Implementar o Azure Digital Twins e configurar um gráfico espacial)

<!-- Images -->
[1]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png
[2]: media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png
[3]: media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png
[4]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png
