---
title: Desenvolver aplicações de .NET Core Azure Service Fabric com o Visual Studio Code | Microsoft Docs
description: Este artigo mostra como criar, implementar e depurar aplicações de .NET Core Service Fabric com o Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 27c7c62125f3f559fb1764292729cbbfdc1c4e5f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116291"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Desenvolver aplicações de Service Fabric de c# com o Visual Studio Code

O [extensão de recursos de infraestrutura de serviço Reliable Services para o VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) torna mais fácil criar aplicações de .NET Core Service Fabric em sistemas operativos Windows, Linux e macOS.

Este artigo mostra como criar, implementar e depurar uma aplicação de Service Fabric do .NET Core utilizando o Visual Studio Code.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que já tem instalado o VS Code, a extensão de recursos de infraestrutura de serviço Reliable Services para o VS Code e as dependências necessárias para o seu ambiente de desenvolvimento. Para obter mais informações, consulte [introdução](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Transferir o exemplo
Este artigo utiliza a aplicação de CounterService o [repositório do GitHub exemplos de Service Fabric .NET Core introdução](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Para clonar o repositório à sua máquina de desenvolvimento, execute o seguinte comando a partir de uma janela de terminal (janela de comandos no Windows):

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Abra a aplicação no VS Code

### <a name="windows"></a>Windows
Faça duplo clique no ícone de VS Code no Menu Iniciar e escolha **executar como administrador**. Para ligar o depurador aos seus serviços, terá de executar o VS Code como administrador.

### <a name="linux"></a>Linux
Utilizar o terminal, navegue para o caminho /service-fabric-dotnet-core-getting-started/Services/CounterService do diretório que a aplicação foi clonada na localmente.

Execute o seguinte comando para abrir o VS Code como utilizador raiz, para que o depurador pode anexar aos seus serviços.
```
sudo code . --user-data-dir='.'
```

A aplicação deverá agora aparecer na sua área de trabalho do VS Code.

![Aplicação de serviço de contador na área de trabalho](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>Criar a aplicação
1. Prima (Ctrl + Shift + p) para abrir o **paleta de comando** no VS Code.
2. Procure e selecione o **Service Fabric: Criar aplicação** comando. Saída da compilação é enviada para o terminal integrado.

   ![Criar o comando de aplicação no VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Implementar a aplicação ao local cluster
Depois de criada a aplicação, pode implementá-la ao local cluster. 

1. Do **paleta de comando**, selecione o **Service Fabric: o comando de implementar aplicações (Localhost)**. A saída do processo de instalação é enviada para o terminal integrado.

   ![Implementar o comando de aplicação no VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Quando a implementação estiver concluída, iniciar um browser e abra o Explorador do Service Fabric: http://localhost:19080/Explorer. Deverá ver que a aplicação está em execução. Esta operação pode demorar algum tempo, por isso ser patient. 

   ![Aplicação de serviço de contador no Service Fabric Explorer](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Depois de verificar que a aplicação é executada, iniciar um browser e abra esta página: http://localhost:31002. Este é o web front-end da aplicação. Atualize a página para ver o valor atual do contador que será contado.

   ![Aplicação de serviço no Browser do contador](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="debug-the-application"></a>Depurar a aplicação
Quando a depuração de aplicações no VS Code, a aplicação tem de estar em execução num local cluster. Pontos de interrupção, em seguida, é possível adicionar o código.

Para definir um ponto de interrupção e de depuração, conclua os seguintes passos:
1. No Explorador, abra o */src/CounterServiceApplication/CounterService/CounterService.cs* de ficheiros e defina um ponto de interrupção na linha 62 dentro de `RunAsync` método.
3. Clique no ícone de depuração no **atividade barra** para abrir a vista de depurador no VS Code. Clique no ícone de equipamento no topo da vista de depurador e selecione **.NET Core** no menu de ambiente de lista pendente. Para abrir o ficheiro launch.json. Pode fechar este ficheiro. Agora deverá ver as opções de configuração no menu de configuração de depuração, localizado junto ao botão execução (seta verde).

   ![Depurar o ícone na área de trabalho do VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Selecione **.NET Core anexar** no menu de configuração de depuração.

   ![Depurar o ícone na área de trabalho do VS Code](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Abra o Explorador de recursos de infraestrutura de serviço num browser: http://localhost:19080/Explorer. Clique em **aplicações** e desagregar dowwn para determinar que o CounterService está em execução no nó principal. Na imagem abaixo do nó principal para o CounterService é nó 0.

   ![Nó principal para CounterService](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. No VS Code, clique no ícone de execução (seta verde) junto a **.NET Core anexar** configuração de depuração. Na caixa de diálogo de seleção de processo, selecione o processo CounterService que está em execução no nó principal que identificou no passo 4.

   ![Processo primário](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. O ponto de interrupção de *CounterService.cs* ficheiro ser acederá muito rapidamente. Em seguida, pode explorar os valores das variáveis locais. Utilize a barra de ferramentas de depuração na parte superior do VS Code para continuar a execução do passo ao longo de linhas, passo para métodos, ou passo fora do método atual. 

   ![Os valores de depuração](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Para terminar a sessão de depuração, clique no ícone de plug na barra de ferramentas de depuração na parte superior do código de VS...
   
   ![Desligue o depurador](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Quando tiver terminado de depuração, pode utilizar o **Service Fabric: remover aplicação** comando para remover a aplicação CounterService do seu cluster local. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [desenvolver e depurar aplicações de Java do Service Fabric com o código de VS](./service-fabric-develop-java-applications-with-vs-code.md).



