---
title: Criar um Reliable Services do Azure Service Fabric com C#
description: "Crie, implemente e depure uma aplicação Reliable Services baseada no Azure Service Fabric com o Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 43f77a1a2e1bbe28bb646aa23c28c253c20e8dda
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Criar a sua primeira aplicação Reliable Services com monitorização de estado do Service Fabric em C#

Saiba como implementar a sua primeira aplicação do Azure Service Fabric para .NET no Windows em poucos minutos. Quando tiver terminado, terá um cluster local em execução com uma aplicação Reliable Services.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que [configurou o seu ambiente de desenvolvimento](service-fabric-get-started.md). Este processo inclui instalar o Service Fabric SDK e o Visual Studio 2017 ou 2015.

## <a name="create-the-application"></a>Criar a aplicação

1. Inicie o Visual Studio como administrador.

2. Crie um projeto ao selecionar Ctrl+Shift+N.

3. Na caixa de diálogo **Novo Projeto**, selecione **Cloud** > **Aplicação do Service Fabric**.

4. Atribua o nome **MyApplication** à aplicação. Em seguida, selecione **OK**.

   ![Caixa de diálogo Novo projeto no Visual Studio][1]

5. Pode criar qualquer tipo de aplicação do Service Fabric na caixa de diálogo seguinte. Neste início rápido, escolha **Serviço com Estado** .

6. Atribua o nome **MyStatefulService** ao serviço. Em seguida, selecione **OK**.

    ![Caixa de diálogo Novo serviço no Visual Studio][2]

    O Visual Studio cria o projeto de aplicação e o projeto de serviço com estado. Em seguida, apresenta-os no Explorador de Soluções.

    ![Explorador de Soluções a seguir à criação de uma aplicação com o serviço com estado][3]

    O projeto de aplicação (**MyApplication**) não tem qualquer código. Em vez disso, referencia um conjunto de projetos de serviço. Também tem três outros tipos de conteúdo:

    * **Perfis de publicação**  
    Perfis para implementar em diferentes ambientes.

    * **Scripts**  
    Scripts de PowerShell para implementar ou atualizar a sua aplicação.

    * **Definição da aplicação**  
Inclui o ficheiro ApplicationManifest.xml em *ApplicationPackageRoot*, que descreve a composição da sua aplicação. Os ficheiros de parâmetros da aplicação associados estão localizados em *ApplicationParameters*, que podem ser utilizados para especificar parâmetros específicos para o ambiente. O Visual Studio seleciona um ficheiro de parâmetros de aplicação que é especificado no perfil de publicação associado.
    
Para obter uma descrição geral do conteúdo do projeto de serviço, consulte o artigo [Introdução a Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Implemente a aplicação e depure-a

Agora que tem uma aplicação, execute-a, implemente-a e depure-a ao realizar os seguintes passos.

1. No Visual Studio, selecione F5 para implementar a aplicação com o fim de a depurar.

    >[!NOTE]
    >Da primeira vez que executar e implementar a aplicação localmente, o Visual Studio cria um cluster local para depuração. Esta operação pode demorar algum tempo. O estado da criação do cluster aparece na janela de saída do Visual Studio.

    Quando o cluster estiver pronto, recebe uma notificação da aplicação do gestor de tabuleiro de sistema do cluster local incluída no SDK.
    
    ![Notificação do tabuleiro de sistema do cluster local][4]

    Depois de a aplicação ser iniciada, o Visual Studio apresenta automaticamente o Visualizador de Eventos de Diagnóstico, onde pode ver os resultados do rastreio dos seus serviços.
    
    ![Visualizador de eventos de diagnóstico][5]

    >[!NOTE]
    >O controlo de eventos deverá ser iniciado automaticamente no Visualizador de Eventos de Diagnóstico. Se precisar de configurar manualmente o Visualizador de Eventos de Diagnóstico, em primeiro lugar, abra o ficheiro `ServiceEventSource.cs`, que se encontra localizado no projeto **MyStatefulService**. Copie o valor do atributo `EventSource` na parte superior da classe `ServiceEventSource`. No exemplo seguinte, a origem de evento tem o nome `"MyCompany-MyApplication-MyStatefulService"`, mas este pode ser outro no seu caso.
>
    >![Localizar o Nome da Origem de Evento do Serviço][service-event-source-name]


2. Em seguida, abra a caixa de diálogo **Fornecedores de ETW**. Depois disso, selecione o ícone de engrenagem localizado no separador **Eventos de Diagnóstico**. Cole o nome da origem de evento que copiou na caixa de entrada **Fornecedores de ETW**. Em seguida, selecione o botão **Aplicar**. Esta ação inicia automaticamente eventos de rastreio.

    ![Definir o nome da origem de Evento do Diagnóstico][setting-event-source-name]

    A janela Eventos de Diagnóstico deverá começar a apresentar eventos.

    O modelo de serviço com estado mostra um valor de contador que vai registando incrementos no método `RunAsync` de **MyStatefulService.cs**.

3. Expanda um dos eventos para ver mais detalhes, incluindo o nó em que se executa o código. Neste caso, é **\_Nó\_0**, embora possa ser diferente no seu computador.
   
    ![Detalhe do visualizador de eventos de diagnóstico][6]

4. O cluster local contém cinco nós que estão alojados num único computador. Num ambiente de produção, cada nó é alojada num computador físico ou virtual diferente. Para simular a perda de um computador enquanto executa o depurador do Visual Studio, desative um dos nós no cluster local.

5. Na janela **Explorador de Soluções**, abra **MyStatefulService.cs**. 

6. Localize o método `RunAsync` e, em seguida, defina um ponto de interrupção na primeira linha do método.

    ![Método RunAsync com ponto de interrupção do serviço com estado ][7]

7. Inicie a ferramenta Service Fabric Explorer, ao clicar com o botão direito do rato na aplicação de tabuleiro de sistema do **Gestor de Clusters Locais** e depois selecionar **Gerir Cluster Local**.

    ![Iniciar o Service Fabric Explorer a partir do gestor de clusters locais][systray-launch-sfx]

    O [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) oferece uma representação visual de um cluster. Inclui o conjunto de aplicações que se encontram implementadas no mesmo e o conjunto de nós físicos que o constituem.

8. No painel da esquerda, expanda **Cluster** > **Nós** e localize o nó no qual o seu código se encontra em execução. Em seguida, para simular o reinício de um computador, selecione **Ações** > **Desativar (Reiniciar)**.

    ![Parar um nó no Service Fabric Explorer][sfx-stop-node]

    Momentaneamente, deverá ver o ponto de interrupção atingido no Visual Studio, dado que a computação que estava a fazer diretamente num nó falha no outro.

9. Em seguida, regresse ao Visualizador de Eventos de Diagnóstico e observe as mensagens. O contador não deixa de aumentar, apesar de os eventos, na verdade, estarem a vir por um nó diferente.

    ![Visualizador de eventos de diagnóstico após falha][diagnostic-events-viewer-detail-post-failover]

## <a name="clean-up-the-local-cluster-optional"></a>Limpar o cluster local (opcional)

Lembre-se de que este cluster local é real. Parar o depurador remove a instância da aplicação e anula o registo do tipo de aplicação. Contudo, o cluster continua a ser executado em segundo plano. Quando estiver pronto para parar o cluster local, tem duas opções à sua disposição.

### <a name="keep-application-and-trace-data"></a>Manter os dados da aplicação e de rastreio

Encerre o cluster, ao clicar com o botão direito do rato na aplicação de tabuleiro de sistema **Gestor de Clusters Locais** e depois selecionar **Parar Cluster Local**.

### <a name="delete-the-cluster-and-all-data"></a>Eliminar o cluster e todos os dados

Remova o cluster, ao clicar com o botão direito do rato na aplicação de tabuleiro de sistema **Gestor de Clusters Locais**. Em seguida, escolha **Remover Cluster Local**. 

Se escolher esta opção, o Visual Studio reimplementa o cluster da próxima vez que a aplicação for executada. Escolha esta opção se não pretender utilizar o cluster local durante algum tempo ou se precisar de recuperar recursos.

## <a name="next-steps"></a>Passos seguintes
Leia mais sobre o [Reliable Services](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
