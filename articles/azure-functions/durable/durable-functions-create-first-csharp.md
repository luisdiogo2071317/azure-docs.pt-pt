---
title: Criar a sua primeira função durável no Azure comC#
description: Crie e publique uma função de durável do Azure com o Visual Studio.
services: functions
documentationcenter: na
author: jeffhollan
manager: jeconnoc
keywords: funções do azure, funções, processamento de eventos, computação, arquitetura sem servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 11/07/2018
ms.author: azfuncdf
ms.openlocfilehash: a0aed1fd65444397b3ca084cc0100aa5af49c918
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839515"
---
# <a name="create-your-first-durable-function-in-c"></a>Criar a sua primeira função durável em C\#

*Funções duráveis* é uma extensão da [as funções do Azure](../functions-overview.md) que permite que escreva funções com monitoração de estado num ambiente sem servidor. A extensão gere o estado, os pontos de verificação e reinicializações por si.

Neste artigo, irá aprender a utilizar as ferramentas do Visual Studio 2017 para as funções do Azure para criar localmente e testar uma função durável "hello world".  Esta função será orquestrar e em conjunto da cadeia de chamadas para outras funções. Em seguida, publique o código de função no Azure. Estas ferramentas estão disponíveis como parte da carga de trabalho de desenvolvimento do Azure no Visual Studio 2017.

![Executar a função durável no Azure](./media/durable-functions-create-first-csharp/functions-vs-complete.png)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* Instale o [Visual Studio 2017](https://azure.microsoft.com/downloads/) e verifique se a carga de trabalho **Desenvolvimento do Azure** também é instalada.

* Verifique se tem as [ferramentas de Funções do Azure mais recentes](../functions-develop-vs.md#check-your-tools-version).

* Certifique-se de que tem o [emulador de armazenamento do Azure](../../storage/common/storage-use-emulator.md) instalado e em execução.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-function-app-project"></a>Criar um projeto de aplicação de função

O modelo do projeto das Funções do Azure no Visual Studio cria um projeto que pode ser publicado numa aplicação de funções no Azure. As aplicações de funções permitem-lhe agrupar funções como unidades lógicas para a gestão, implementação e partilha de recursos.

1. No Visual Studio, selecione **Novo** > **Projeto**, no menu **Ficheiro**.

2. Na caixa de diálogo **Novo Projeto**, selecione **Instalado**, expanda **Visual C#** > **Cloud**, selecione **Funções do Azure**, escreva um **Nome** para o projeto e clique em **OK**. O nome da aplicação de funções deve ser válido como um espaço de nomes C#. Portanto, não use sublinhados, hífenes ou outros carateres não alfanuméricos.

    ![Caixa de diálogo Novo projeto para criar uma função no Visual Studio](./media/durable-functions-create-first-csharp/functions-vs-new-project.png)

3. Utilize as definições especificadas na tabela a seguir à imagem.

    ![Caixa de diálogo de função nova no Visual Studio](./media/durable-functions-create-first-csharp/functions-vs-new-function.png)

    | Definição      | Valor sugerido  | Descrição                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Versão** | Funções do Azure 2.x <br />(.NET Core) | Cria um projeto de função que utiliza a versão 2.x do runtime das funções do Azure que suporta o .NET Core. As Funções do Azure 1.x suportam o .NET Framework. Para obter mais informações, veja [How to target Azure Functions runtime version](../functions-versions.md) (Como segmentar a versão do runtime das Funções do Azure).   |
    | **Modelo** | Vazio | Esta ação cria uma aplicação de função vazia. |
    | **Conta de armazenamento**  | Emulador do Armazenamento | Uma conta de armazenamento é necessária para a gestão de estado de função durável. |

4. Clique em **OK** para criar um projeto de função vazia.

## <a name="add-functions-to-the-app"></a>Adicionar funções para a aplicação

Visual Studio cria um projeto de aplicação de função vazia.  Ela contém os ficheiros de configuração básica necessários para uma aplicação, mas ainda não contém quaisquer funções.  Vamos precisar de adicionar um modelo de função durável ao projeto.

1. Com o botão direito no projeto no Visual Studio e selecione **Add** > **nova função do Azure**.

    ![Adicionar nova função](./media/durable-functions-create-first-csharp/functions-vs-add-new-function.png)

2. Certifique-se **função do Azure** está selecionada no menu Adicionar e dê sua C# um nome de ficheiro.  Prima **Adicionar**.

3. Selecione o **orquestração de funções durável** modelo e clique em **Ok**

    ![Selecionar modelo durável](./media/durable-functions-create-first-csharp/functions-vs-select-template.png)  

Uma nova função durável será adicionada à aplicação.  Abra o ficheiro novo para exibir o conteúdo.  Esta função durável é uma função simples exemplo de encadeamento.  

* O `RunOrchestrator` método é associado a função de orquestrador.  Esta função irá iniciar, crie uma lista e adicionar o resultado de três chamadas de funções à lista.  Quando as chamadas de três função forem concluídas, ela retornará a lista.  A função está chamando é o `SayHello` método (padrão, ele será chamado "<NameOfFile>_Hello").
* O `SayHello` a função devolve um hello.
* O `HttpStart` método descreve a função que irá iniciar instâncias da orquestração.  Ela é associada a uma [acionador HTTP](../functions-bindings-http-webhook.md) que irá iniciar uma nova instância do orquestrador e devolve uma resposta da verificação de estado.

Agora que criou o projeto de função e uma função durável, pode testá-lo no seu computador local.

## <a name="test-the-function-locally"></a>Testar localmente a função

As Ferramentas de Núcleo das Funções do Azure permitem-lhe executar um projeto de funções do Azure no seu computador de programação local. Deverá instalar essas ferramentas da primeira vez que iniciar uma função do Visual Studio.

1. Para testar a sua função, prima F5. Se solicitado, aceite o pedido do Visual Studio para transferir e instalar as ferramentas de Núcleo das Funções do Azure (CLI). Também poderá ativar a exceção da firewall para que todas as ferramentas possam aceitar os pedidos de HTTP.

2. Copie o URL da sua função na saída do tempo de execução das funções do Azure.

    ![Tempo de execução local do Azure](./media/durable-functions-create-first-csharp/functions-vs-debugging.png)

3. Cole o URL do pedido HTTP na barra de endereço do browser e execute o pedido. O exemplo apresentado em seguida mostra a resposta no browser relativamente ao pedido GET devolvido retornado pela função: 

    ![Resposta da função localhost no browser](./media/durable-functions-create-first-csharp/functions-vs-status.png)

    A resposta é o resultado inicial da função HTTP nos informar a orquestração durável foi iniciado com êxito.  Ainda não é o resultado final da orquestração.  A resposta inclui alguns URLs útil.  Por enquanto, vamos consultar o estado da orquestração.

4. Copie o valor de URL para `statusQueryGetUri` e colá-la no endereço do browser da barra e execute o pedido.

    O pedido irá consultar a instância de orquestração para o estado. Obterá uma resposta eventual que é semelhante ao seguinte.  Isto mostra-na instância tiver sido concluída e inclui as saídas ou os resultados da função durável.

    ```json
    {
        "instanceId": "d495cb0ac10d4e13b22729c37e335190",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-11-08T07:07:40Z",
        "lastUpdatedTime": "2018-11-08T07:07:52Z"
    }
    ```

5. Para parar a depuração, prima **Shift + F5**.

Depois de verificar que a função é executada corretamente no computador local, deve publicar o projeto no Azure.

## <a name="publish-the-project-to-azure"></a>Publicar o projeto no Azure

Deverá ter uma aplicação de funções na sua subscrição do Azure antes de poder publicar o seu projeto. Pode criar uma aplicação de funções diretamente a partir do Visual Studio.

[!INCLUDE [Publish the project to Azure](../../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testar a sua função no Azure

1. Copie a URL de base da aplicação de funções a partir da página de perfil Publicar. Substitua a `localhost:port` parte da URL que utilizou ao testar a localmente a função com a nova URL de base.

    O URL que chama o acionador de HTTP de função durável deve estar no seguinte formato:

        http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>_HttpStart

2. Cole este novo URL do pedido HTTP na barra de endereço do browser. Deve obter a mesma resposta como Estado, quando utilizar a aplicação publicada.

## <a name="next-steps"></a>Passos Seguintes

Utilizou o Visual Studio para criar e publicar um C# aplicação de funções durável.

> [!div class="nextstepaction"]
> [Saiba mais sobre os padrões comuns de função durável.](durable-functions-overview.md)
