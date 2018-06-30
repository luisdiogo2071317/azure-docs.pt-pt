---
title: Desenvolver aplicações de Java do Azure Service Fabric com o Visual Studio Code | Microsoft Docs
description: Este artigo mostra como criar, implementar e depurar aplicações de Java Service Fabric com o Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: JimacoMS
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 54c94c50f6292694e947d97a10fd6976c14e19df
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116178"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Desenvolver aplicações de Java do Service Fabric com o Visual Studio Code

O [extensão de recursos de infraestrutura de serviço Reliable Services para o VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) torna mais fácil criar aplicações de Java Service Fabric em sistemas operativos Windows, Linux e macOS.

Este artigo mostra como criar, implementar e depurar uma aplicação de Service Fabric do Java utilizando o Visual Studio Code.

> [!IMPORTANT]
> Aplicações de Java de recursos de infraestrutura de serviço podem ser desenvolvidas nas máquinas do Windows, mas podem ser implementadas em clusters do Linux do Azure apenas. A depuração de aplicações Java não é suportada no Windows.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que já tem instalado o VS Code, a extensão de recursos de infraestrutura de serviço Reliable Services para o VS Code e as dependências necessárias para o seu ambiente de desenvolvimento. Para obter mais informações, consulte [introdução](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Transferir o exemplo
Este artigo utiliza a aplicação de voto no [repositório do GitHub de exemplo início rápido do Service Fabric Java aplicação](https://github.com/Azure-Samples/service-fabric-java-quickstart). 

Para clonar o repositório à sua máquina de desenvolvimento, execute o seguinte comando a partir de uma janela de terminal (janela de comandos no Windows):

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>Abra a aplicação no VS Code

Abra o VS Code.  Clique no ícone do Explorador no **atividade barra** e clique em **Abrir pasta**, ou clique em **ficheiro -> pasta abra**. Navegue para o *./service-fabric-java-quickstart/Voting* diretório na pasta onde clonou o repositório, em seguida, clique em **OK**. A área de trabalho deve conter os mesmos ficheiros mostrados na captura de ecrã abaixo.

![Aplicação de voto do Java na área de trabalho](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>Criar a aplicação

1. Prima (Ctrl + Shift + p) para abrir o **paleta de comando** no VS Code.
2. Procure e selecione o **Service Fabric: Criar aplicação** comando. Saída da compilação é enviada para o terminal integrado.

   ![Criar aplicação comando no VS Code](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Implementar a aplicação ao local cluster
Depois de criada a aplicação, pode implementá-la ao local cluster. 

> [!IMPORTANT]
> Implementação de aplicações Java ao local cluster não é suportada em máquinas do Windows.

1. Do **paleta de comando**, selecione o **Service Fabric: o comando de implementar aplicações (Localhost)**. A saída do processo de instalação é enviada para o terminal integrado.

   ![Implementar a aplicação de comando no VS Code](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Quando a implementação estiver concluída, iniciar um browser e abra o Explorador do Service Fabric: http://localhost:19080/Explorer. Deverá ver que a aplicação está em execução. Esta operação pode demorar algum tempo, por isso ser patient. 

   ![Aplicação de voto no Service Fabric Explorer](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Depois de ter verificado que a aplicação está em execução, iniciar um browser e abra esta página: http://localhost:8080. Este é o web front-end da aplicação. Pode adicionar itens e clique nos mesmos votar.

   ![Aplicação de voto no Browser](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. Para remover a aplicação do cluster, selecione o **Service Fabric: remover aplicações** comando a partir de **paleta de comando**. A saída do processo de desinstalação é enviada para o terminal integrado. Pode utilizar o Service Fabric Explorer para verificar que a aplicação foi removida do local cluster.

## <a name="debug-the-application"></a>Depurar a aplicação
Quando a depuração de aplicações no VS Code, a aplicação tem de estar em execução num local cluster. Pontos de interrupção, em seguida, é possível adicionar o código.

> [!IMPORTANT]
> A depuração de aplicações Java não é suportada em máquinas do Windows.

Para preparar o VotingDataService e a aplicação de voto de depuração, conclua os seguintes passos:

1. Atualização do *Voting/VotingApplication/VotingDataServicePkg/Code/entryPoint.sh* ficheiro.
Comente o comando na linha 6 (utilize '#') e adicione o seguinte comando na parte inferior do ficheiro:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. Atualização do *Voting/VotingApplication/ApplicationManifest.xml* ficheiro. Definir o **MinReplicaSetSize** e **TargetReplicaSetSize** atributos "1" no **StatefulService** elemento:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. Clique no ícone de depuração no **atividade barra** para abrir a vista de depurador no VS Code. Clique no ícone de equipamento no topo da vista de depurador e selecione **Java** no menu de ambiente de lista pendente. Para abrir o ficheiro launch.json. 

   ![Depurar o ícone na área de trabalho do VS Code](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. No ficheiro launch.json, defina o valor da porta na configuração com o nome **depurar (ligar)** para **8001**. Guarde o ficheiro.

   ![Configuração de depuração para o launch.json](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. Implementar a aplicação ao local cluster utilizando o **Service Fabric: implementar aplicações (Localhost)** comando. Certifique-se de que a aplicação está em execução no Service Fabric Explorer. A aplicação está agora pronta para ser debugged.

Para definir um ponto de interrupção, conclua os seguintes passos:

1. No Explorador, abra o */Voting/VotingDataService/src/statefulservice/VotingDataService.java* ficheiro. Definir um ponto de interrupção na primeira linha de código no `try` bloquear o `addItem` método (linha 80).
   
   ![Definir o ponto de interrupção no serviço de dados de voto](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Certifique-se de que define os pontos de interrupção no executável linhas de código. Por exemplo pontos de interrupção definido em declarações de método `try` declarações, ou `catch` instruções irão ser omitidas pelo depurador.
2. Para começar a depuração, clique no ícone de depuração no **atividade barra**, selecione o **depuração (ligar)** configuração a partir do menu de depuração e clique no botão de execução (seta verde).

   ![Depurar (ligar) configuração](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. Num browser, aceda a http://localhost:8080. Escreva um novo item na caixa de texto e clique em **+ adicionar**. Deve ser atingiu o seu ponto de interrupção. Pode utilizar a barra de ferramentas de depuração na parte superior do VS Code para continuar a execução do passo ao longo de linhas, passo para métodos, ou passo fora do método atual. 
   
   ![Atingiu o ponto de interrupção](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Para terminar a sessão de depuração, clique no ícone de plug na barra de ferramentas de depuração na parte superior do VS Code.
   
   ![Desligue o depurador](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Quando tiver terminado de depuração, pode utilizar o **Service Fabric: remover aplicação** comando para remover a aplicação de voto do local cluster. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [desenvolver e depurar aplicações de Service Fabric de c# com o código de VS](./service-fabric-develop-csharp-applications-with-vs-code.md).
