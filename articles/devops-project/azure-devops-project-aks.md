---
title: Implementar a Aplicação ASP.NET Core no Azure Kubernetes Service (AKS) com o Azure DevOps Project | Tutorial do Azure DevOps
description: O Azure DevOps Project permite começar a utilizar o Azure mais facilmente. O Azure DevOps Project facilita a implementação da Aplicação ASP.NET Core com o Azure Kubernetes Service (AKS) com alguns passos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 55ea101b3a03fdb7fc375c4594cab36d4cd79978
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299122"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-kubernetes-service-aks-with-the-azure-devops-project"></a>Tutorial: implementar a Aplicação ASP.NET Core no Azure Kubernetes Service (AKS) com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada com a qual pode utilizar o seu código existente e o repositório de Git, ou escolher um dos exemplos de aplicação, para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.  O DevOps Project cria automaticamente recursos do Azure, como o AKS, cria e configura um pipeline de versão nos Serviços de DevOps do Azure que inclui um pipeline de compilação e versão para CI/CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Irá:

> [!div class="checklist"]
> * Criar um Azure DevOps Project para uma Aplicação ASP.NET Core e AKS
> * Configurar os Serviços de DevOps do Azure e uma subscrição do Azure 
> * Examinar o cluster do AKS
> * Examinar o pipeline de CI dos Serviços de DevOps do Azure
> * Examinar o pipeline de CD dos Serviços de DevOps do Azure
> * Consolidar as alterações ao Git e implementar automaticamente no Azure
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-aks"></a>Criar um Azure DevOps Project para uma Aplicação ASP.NET Core e AKS

O Azure DevOps Project cria um pipeline de CI/CD no Azure.  Pode criar uma organização **nova dos Serviços de DevOps do Azure** ou utilizar uma **organização existente**.  O Projeto de DevOps do Azure também cria **recursos do Azure**, como um cluster do AKS na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Selecione o ícone **Criar um recurso** na barra de navegação esquerda e, em seguida, procure **DevOps Project**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-aks/fullbrowser.png)

1. Selecione **.NET** e **Seguinte**.

1. Em **Escolha uma arquitetura de aplicação**, selecione **ASP.NET Core** e selecione **Seguinte**.

1. Selecione **Serviço do Kubernetes** e escolha **Seguinte**.  

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configurar os Serviços de DevOps do Azure e uma subscrição do Azure

1. Crie uma organização **nova** do Azure DevOps ou utilize uma organização **existente**.  Escolha um **nome** para o projeto.  

1. Selecione a sua **subscrição do Azure**.

1. Selecione a ligação **Alterar** para ver as definições adicionais de configuração do Azure e identificar o **número de nós** do **cluster do Kubernetes**.  Aqui, existem várias opções para configurar o tipo e a localização de serviços do Azure.
 
1. Saia da área de configuração do Azure e selecione **Concluído**.

1. Irá demorar alguns minutos até que o processo esteja concluído.  Uma aplicação ASP.NET Core de exemplo é configurada num repositório Git do Azure na sua organização de Serviços de DevOps do Azure, é criado um cluster do AKS, é executado um pipeline de CI/CD e a sua aplicação é implementada no Azure.  

    Depois de concluído, o **dashboard do Projeto** do Azure DevOps é carregado no portal do Azure.  Também pode navegar para o **Dashboard do Projeto de DevOps do Azure** diretamente a partir de **Todos os recursos** no **portal do Azure**.  

    Este dashboard oferece visibilidade para o seu **repositório de código** do Azure, o **pipeline de CI/CD dos Serviços de DevOps do Azure** e o **cluster do AKS**.  Pode configurar opções de CI/CD adicionais no pipeline dos Serviços de DevOps do Azure.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.

## <a name="examine-the-aks-cluster"></a>Examinar o cluster do AKS

O projeto de DevOps do Azure configura automaticamente um cluster do AKS.  Pode explorar e personalizar o cluster.  Siga os passos abaixo para familiarizar-se com o AKS.

1. Navegue para o **dashboard do Projeto de DevOps do Azure**.

1. No lado direito do dashboard do DevOps Project, selecione o **serviço do Kubernetes**.

1. É aberto um painel para o cluster do AKS.  Nesta vista, pode executar várias ações, como **monitorizar o estado de funcionamento do contentor**, **pesquisar registos** e abrir o **dashboard do Kubernetes**.

1. No lado direito do ecrã, selecione **Ver o dashboard do Kubernetes**.  Opcionalmente, siga os passos para abrir o dashboard do Kubernetes.

## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Examinar o pipeline de CI dos Serviços de DevOps do Azure

O Azure DevOps Project configura automaticamente um pipeline de CI/CD do Azure na sua organização do Azure DevOps.  Pode explorar e personalizar o pipeline.  Siga os passos abaixo para familiarizar-se com o pipeline de CI/CD dos Serviços de DevOps do Azure.

1. Navegue para o **dashboard do Projeto de DevOps do Azure**.

1. Selecione **Pipelines de Compilação** na **parte superior** do **dashboard do Azure DevOps Project**.  Esta ligação abre um separador do browser e abre o pipeline de compilação dos Serviços de DevOps do Azure para o novo projeto.

1. Mova o cursor do rato para o lado direito do pipeline de compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode executar várias atividades, como **colocar uma nova compilação em fila**, **colocar uma compilação em pausa** e **editar o pipeline de compilação**.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** do seu pipeline de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git dos Serviços de DevOps do Azure, restaurar dependências e publicar as saídas utilizadas para implementações.

1. Na parte superior do pipeline de compilação, selecione o **nome do pipeline de compilação**.

1. Altere o **nome** do seu pipeline de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome do pipeline de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  Os Serviços de DevOps do Azure mantêm um registo de todas as alterações efetuadas no pipeline de compilação e permitem-lhe comparar versões.

1. Selecione **Acionadores**.  O Azure DevOps Project criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

## <a name="examine-the-azure-devops-services-cd-release-pipeline"></a>Examinar o pipeline de versão de CD dos Serviços de DevOps do Azure

O Azure DevOps Project cria e configura automaticamente os passos necessários para implementar da sua organização de Serviços de DevOps do Azure para a sua subscrição do Azure.  Estes passos incluem a configuração de uma ligação ao serviço do Azure para autenticar os Serviços de DevOps do Azure para a sua subscrição do Azure.  A automatização também cria um pipeline de versão dos Serviços de DevOps do Azure e este pipeline disponibiliza a CD para o Azure.  Siga os passos abaixo para examinar mais sobre o pipeline de versão dos Serviços de DevOps do Azure.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O Azure DevOps Project criou um pipeline de versão dos Serviços de DevOps do Azure para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto ao pipeline de versão e, em seguida, selecione **Editar**.

1. O pipeline de versão contém um **pipeline**, que define o processo da versão.  Em **Artefactos**, selecione **Remover**.  O pipeline de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **ícone** **Acionador de implementação contínua** (que é apresentado como um relâmpago.)  Este pipeline de versão tem um acionador de CD ativado.  O acionador cria uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Existem vários menus para explorar a partir desta vista, como um **resumo da versão**, **itens relacionados com trabalho** e **Testes**.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. Pode comparar versões para ver as diferenças de consolidação entre implementações.

1. Selecionar **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure 

 > [!NOTE]
 > Os passos abaixo testam o pipeline de CI/CD com uma alteração de texto simples à sua aplicação Web.

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório Git dos Serviços de DevOps do Azure inicia uma compilação nos Serviços de DevOps do Azure e um pipeline de CD implementa as suas alterações no Azure.  Siga os passos abaixo ou utilize outras técnicas para consolidar as alterações ao seu repositório.  Por exemplo, pode **clonar** o repositório Git na sua ferramenta favorita ou IDE e, em seguida, enviar as alterações para este repositório.

1. Selecione **Código** e, em seguida, **Ficheiros** no menu dos Serviços de DevOps do Azure e navegue para o seu repositório.
1. Navegue para o diretório **Vistas\Home**, selecione as **reticências** junto ao ficheiro **Index.cshtml** e, em seguida, selecione **Editar**.

1. Faça uma alteração ao ficheiro, como algum texto dentro de uma das **etiquetas div**.  No canto superior direito, selecione **Consolidar**.  Selecione **Consolidar** novamente para emitir a alteração. 

1. Dentro de momentos, **é iniciada uma compilação nos Serviços de DevOps do Azure** e, em seguida, é executada uma versão para implementar as alterações.  Pode monitorizar o **estado da compilação** com o dashboard do DevOps Project ou no browser com a sua organização de Serviços de DevOps do Azure.

1. Assim que a versão estiver concluída, **atualize a aplicação** no browser para verificar as alterações.

## <a name="clean-up-resources"></a>Limpar recursos

 > [!NOTE]
 > Os passos abaixo irão eliminar permanentemente os recursos.  Utilize esta funcionalidade só depois de ler atentamente as instruções.

Se estiver a testar, pode limpar os recursos para evitar a criação de encargos de faturação.  Quando já não for necessário, pode eliminar o cluster do Azure Kubernetes e os recursos relacionados que criou neste tutorial, utilizando a funcionalidade **Eliminar** do dashboard do Projeto de DevOps do Azure.  **Tenha cuidado**, uma vez que a funcionalidade de eliminação destrói os dados criados pelo Azure DevOps Project, tanto no Azure como nos Serviços de DevOps do Azure, e não conseguirá recuperá-los depois.

1. No **portal do Azure**, navegue para o **Projeto de DevOps do Azure**.
2. No **canto superior direito** do dashboard, selecione **Eliminar**.  Depois de ler a instrução, selecione **Sim** para **eliminar permanentemente** os recursos.

## <a name="next-steps"></a>Passos seguintes

Opcionalmente, pode modificar estes pipelines de compilação e versão para satisfazer as necessidades da sua equipa. Também pode utilizar este padrão de CI/CD como modelo para outros pipelines.  Aprendeu a:

> [!div class="checklist"]
> * Criar um Azure DevOps Project para uma Aplicação ASP.NET Core e AKS
> * Configurar os Serviços de DevOps do Azure e uma subscrição do Azure 
> * Examinar o cluster do AKS
> * Examinar o pipeline de CI dos Serviços de DevOps do Azure
> * Examinar o pipeline de CD dos Serviços de DevOps do Azure
> * Consolidar as alterações ao Git e implementar automaticamente no Azure
> * Limpar recursos

Para saber mais sobre como utilizar o dashboard do Kubernetes, veja abaixo:

> [!div class="nextstepaction"]
> [Utilizar o dashboard do Kubernetes](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
