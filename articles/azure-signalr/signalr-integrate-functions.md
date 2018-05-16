---
title: Tutorial para integrar as Funções do Azure no Azure SignalR Service | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar as Funções do Azure com o Azure SignalR Service
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/24/2018
ms.author: wesmc
ms.openlocfilehash: b1bb6b3fe545d5a42fa985ab85bd7a914128e56b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-integrate-azure-functions-with-azure-signalr-service"></a>Tutorial: Integrar as Funções do Azure no Azure SignalR Service

Este tutorial baseia-se na aplicação de sala de chat criada nos tutoriais anteriores. Se não tiver concluído [Create a chat room with SignalR Service](signalr-quickstart-dotnet-core.md) (Criar uma sala de chat com o SignalR Service) e [Azure SignalR Service authentication](./signalr-authenticate-oauth.md) (Autenticação do Azure SignalR Service), conclua-os primeiro. 

Um cenário comum das aplicações em tempo real está relacionado com as atualizações de conteúdos terem origem num servidor para serem publicadas em clientes Web. As [Funções do Azure](../azure-functions/functions-overview.md) são uma excelente opção para gerar essas atualizações de conteúdos. Uma das principais vantagens de utilizar as Funções do Azure é o facto de poder executar o seu código a pedido sem se preocupar com a arquitetura de uma aplicação inteira ou com a infraestrutura para executá-la. Também só paga pelo tempo durante o qual o seu código é efetivamente executado.  

Normalmente, este cenário apresentaria um problema quando se tentasse utilizar o SignalR, porque este serviço tenta manter uma ligação entre o cliente e o servidor para enviar as atualizações de conteúdos. Uma vez que o código só é executado a pedido, não é possível manter uma ligação. No entanto, o Azure SignalR Service pode suportar este cenário, uma vez que gere as ligações por si no tempo de execução.

Neste tutorial, vai utilizar as Funções do Azure para gerar mensagens com uma função de temporizador no início de cada minuto. A função vai publicar as mensagens para todos os clientes da sala de chat criada nos tutoriais anteriores. Para obter mais informações sobre as funções de temporizador, veja [Timer Function](../azure-functions/functions-create-scheduled-function.md) (Função de temporizador).

Pode utilizar qualquer editor de código para concluir os passos deste início rápido. No entanto, o [Visual Studio Code](https://code.visualstudio.com/) é uma excelente opção, disponível nas plataformas Windows, macOS e Linux.

O código deste tutorial está disponível para transferência no [repositório do GitHub AzureSignalR-samples](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer).

![Aplicação de chat com mensagens do servidor](./media/signalr-integrate-functions/signalr-functions-complete.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Utilizar a CLI do Azure para criar uma função de temporizador nova com as Funções do Azure.
> * Configurar a função de temporizador para implementação do repositório do Git local.
> * Ligar o temporizador ao SignalR Service para enviar as atualizações a cada minuto

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, deve ter os seguintes pré-requisitos:

* [Git](https://git-scm.com/)
* [SDK .NET Core](https://www.microsoft.com/net/download/windows) 
* [O Azure Cloud Shell configurado](https://docs.microsoft.com/azure/cloud-shell/quickstart)
* Transferir ou clonar o repositório do GitHub [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-function-app"></a>Criar uma aplicação de função

Tem de criar uma aplicação de função para definir o ambiente de execução das suas funções. A aplicação de função também lhe permite agrupar várias funções como uma unidade lógica, para facilitar a gestão, a implementação e a partilha de recursos. Para obter mais informações, veja [Criar a sua primeira função com a CLI do Azure](../azure-functions/functions-create-first-azure-function-azure-cli.md).

Nesta secção, vai utilizar o Azure Cloud Shell para criar uma nova aplicação de função das Funções do Azure configurada para implementação a partir de um repositório do Git local. 

Quando criar os recursos da aplicação de função, crie-os no mesmo grupo de recursos que utilizou nos tutoriais anteriores. Esta abordagem facilita a gestão de todos os recursos do tutorial.

Copie o script abaixo para o seu editor de texto e substitua os valores dos parâmetros da variável pelos valores dos seus recursos. Copie e cole o script atualizado para o Azure Cloud Shell e prima **Enter** para criar uma conta de armazenamento e uma aplicação de função.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your values from previous    === 
#=== tutorials.                                                   ===
#====================================================================
ResourceGroupName=SignalRTestResources
location=eastus

#====================================================================
#=== Update these variables for the new function app and storage  ===
#=== account.                                                     ===
#====================================================================
functionappName=mysignalfunctionapp
storageAccountName=mystorageaccount

# Create a storage account to hold function app code and settings
az storage account create --resource-group $ResourceGroupName \
--name $storageAccountName \
--location $location --sku Standard_LRS

# Create the function app
az functionapp create --resource-group $ResourceGroupName \
--name $functionappName \
--consumption-plan-location $location \
--storage-account $storageAccountName

```

| Parâmetro | Descrição |
| -------------------- | --------------- |
| ResourceGroupName | Este nome de grupo de recursos foi sugerido nos tutoriais anteriores. É aconselhável manter todos os recursos dos tutoriais agrupados. Utilize o mesmo grupo de recursos que utilizou nos tutoriais anteriores. | 
| localização | Atualize esta variável para a mesma localização que utilizou para criar o grupo de recursos nos tutoriais anteriores. | 
| functionappName | Atualize esta variável com um nome exclusivo para a aplicação de função nova. Por exemplo, signalrfunctionapp22665120. | 
| storageAccountName | Introduza um nome para a conta de armazenamento nova que vai armazenar o código e as definições da aplicação de função. | 



## <a name="configure-the-function-app"></a>Configurar a aplicação de funções

Nesta secção, vai configurar a aplicação de função com uma definição de aplicação que contém a cadeia de ligação para o seu recurso do Azure SignalR Service. O código da função vai utilizar esta definição para ligar e publicar mensagens na sala de chat. Também vai configurar a aplicação de função para implementação a partir de um repositório do Git local.

Copie o script abaixo e substitua os valores dos parâmetros. Cole o script atualizado no Azure Cloud Shell e prima **Enter**.

```azurecli-interactive
#====================================================================
#=== Update these variables to match your resources.              === 
#====================================================================
ResourceGroupName=SignalRTestResources
functionappName=mysignalfunctionapp

#========================================================================
#=== Replace this value with the connection string for your           ===
#=== SignalR Service resource.                                        ===
#========================================================================
connstring="Endpoint=<service_endpoint>;AccessKey=<access_key>;"

# Add the SignalR Service connection string app setting
az functionapp config appsettings set --resource-group $ResourceGroupName \
    --name $functionappName \
    --setting "Azure:SignalR:ConnectionString=$connstring"

# configure for deployment from a local Git repository
az functionapp deployment source config-local-git --name $functionappName \
    --resource-group $ResourceGroupName

```

| Parâmetro | Descrição |
| -------------------- | --------------- |
| ResourceGroupName | Este nome de grupo de recursos foi sugerido nos tutoriais anteriores. É aconselhável manter todos os recursos dos tutoriais agrupados. Utilize o mesmo grupo de recursos que utilizou nos tutoriais anteriores. | 
| functionappName | Atualize esta variável com um nome exclusivo para a aplicação de função nova. Por exemplo, signalrfunctionapp22665120. | 
| connstring | Introduza a cadeia de ligação do recurso do SignalR Service. Pode clicar em **Chaves**, em **DEFINIÇÕES**, na página do recurso do SignalR Service no portal do Azure, para obter esta cadeia de ligação. | 



Anote o URL de implementação do Git que o último comando devolve. Vai utilizar este URL para implementar o código a função.


## <a name="the-timer-function"></a>A função de temporizador

O exemplo da função de temporizador está localizado no diretório */samples/Timer* da sua transferência ou no clone do repositório [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Timer) do GitHub. O código da função de temporizador está localizado em *TimerFunction.cs*. Este código utiliza a cadeia de ligação armazenada com a chave de configuração predefinida (*Azure:SignalR:ConnectionString*) para criar um proxy para o hub. Uma vez que o código da função está em execução no lado do servidor, não há nenhuma razão para exigir que se autentique como um cliente normal. O código tem confiança para utilizar a cadeia de ligação. Ao utilizar este proxy do hub, o código da função pode chamar qualquer método que tenha definido no seu hub. O código chama o método `BroadcastMessage` para publicar uma mensagem que contém a hora atual em que o acionador foi acionado.

O acionador do código da função é um *timerTrigger*, definido nos enlaces em *TimerFunction/function.json*. Inclui uma [expressão CRON](https://wikipedia.org/wiki/Cron#CRON_expression) para definir o acionamento do acionador do temporizador para o início de cada minuto.

```json
{
  "bindings": [
    {
      "type": "timerTrigger",
      "schedule": "0 * * * * *",
      "useMonitor": true,
      "runOnStartup": false,
      "name": "myTimer"
    }
  ],
  "disabled": false,
  "scriptFile": "../Timer.dll",
  "entryPoint": "Timer.TimerFunction.Run"
}
```


## <a name="building-the-timer-function"></a>Criar a função de temporizador

Utilize a [interface de linha de comandos .NET Core (CLI)](https://docs.microsoft.com/dotnet/core/tools/) nos passos seguintes para criar a função e prepará-la para a implementação:

1. Navegue para o subdiretório */samples/Timer* da sua transferência ou para o clone do repositório [AzureSignalR-sample](https://github.com/aspnet/AzureSignalR-samples) do GitHub.

2. Restaure os pacotes de NuGet com o seguinte comando:

        dotnet restore

3. Crie a aplicação de função *Temporizador* com o seguinte comando:

        msbuild /p:Configuration=Release


## <a name="create-and-deploy-the-local-git-repo"></a>Criar e implementar o repositório do Git local

1. Numa shell do Git, navegue para o subdiretório da compilação, */samples/Timer/bin/Release/net461*.

        cd ./AzureSignalR-samples/samples/Timer/bin/Release/net461

2. Utilize o seguinte comando para inicializar o diretório como um repositório do Git novo:

        git init

3. Adicione uma consolidação nova a todos os ficheiros no diretório da compilação.

        git add -A
        git commit -v -a -m "Initial Timer function commit"        

4. Adicione um ponto final remoto para o URL de implementação do Git que anotou durante a configuração da aplicação de função:

        git remote add Azure <enter your Git deployment URL>

5. Implementar a aplicação de função

        git push Azure master

   Quando o código estiver implementado, o temporizador começa a ser acionado imediatamente a cada minuto para executar o código.

## <a name="test-the-chat-app"></a>Testar a aplicação de chat

Navegue para a aplicação de chat. A função de temporizador que acabou de criar vai agora reportar a hora no início de cada minuto.

![Aplicação de chat com mensagens do servidor](./media/signalr-integrate-functions/signalr-functions-complete.png)



## <a name="clean-up-resources"></a>Limpar recursos

Se pretender continuar a testar a aplicação, pode manter os recursos que criou.

Caso contrário, se tiver concluído a aplicação de exemplo, pode eliminar os recursos do Azure, para evitar encargos. 

> [!IMPORTANT]
> A eliminação de um grupo de recursos é irreversível e o grupo de recursos e todos os recursos contidos no mesmo serão permanentemente eliminados. Confirme que não elimina acidentalmente o grupo de recursos ou recursos errados. Se tiver criado os recursos para alojar este exemplo num grupo de recursos existente que contém os recursos que pretende manter, pode eliminar cada recurso individualmente nos respetivos painéis em vez de eliminar o grupo de recursos.
> 
> 

Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.

Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos. As instruções neste artigo utilizaram um grupo de recursos denominado *SignalRTestResources*. No seu grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.

   
![Eliminar](./media/signalr-integrate-functions/signalr-delete-resource-group.png)


É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos para confirmar e clique em **Eliminar**.
   
Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a integrar nas Funções do Azure para enviar atualizações para clientes com base em acionadores daquele serviço. Para saber mais sobre como utilizar o Azure SignalR Service, avance para os exemplos da CLI do Azure para SignalR Service.

> [!div class="nextstepaction"]
> [Azure SignalR CLI Samples](./signalr-cli-samples.md) (Exemplos da CLI do Azure SignalR)



