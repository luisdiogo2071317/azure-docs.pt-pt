---
title: Implementar a Aplicação ASP.NET Core no Azure Kubernetes Service (AKS) com o Projeto de DevOps do Azure | Tutorial do VSTS
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. O projeto de DevOps do Azure facilita a implementação da Aplicação ASP.NET Core com o Azure Kubernetes Service (AKS) com alguns passos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 98c585cc0c6136496fe190cd3eceb27fd856af21
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967372"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-kubernetes-service-aks-with-the-azure-devops-project"></a>Tutorial: implementar a Aplicação ASP.NET Core no Azure Kubernetes Service (AKS) com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada com a qual pode utilizar o seu código existente e o repositório Git, ou escolher um dos exemplos de aplicação, para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.  O projeto de DevOps cria automaticamente recursos do Azure, como o AKS, cria e configura um pipeline de versão no VSTS, que inclui uma definição de compilação para CI, configura uma definição de versão para CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Irá:

> [!div class="checklist"]
> * Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET Core e AKS
> * Configurar o VSTS e uma subscrição do Azure 
> * Examinar o cluster do AKS
> * Examinar a definição de Compilação de CI do VSTS
> * Examinar a definição de Gestão de Versões de CD do VSTS
> * Consolidar as alterações ao VSTS e implementar automaticamente no Azure
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-aks"></a>Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET Core e AKS

O Projeto de DevOps do Azure cria um pipeline de CI/CD no VSTS.  Pode criar uma **nova conta VSTS** ou utilizar uma **conta existente**.  O Projeto de DevOps do Azure também cria **recursos do Azure**, como um cluster do AKS na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **Criar um recurso** na barra de navegação esquerda e, em seguida, procure **Projeto DevOps**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-aks/fullbrowser.png)

1. Selecione **.NET** e escolha **Seguinte**.

1. Em **Escolha uma arquitetura de aplicação**, selecione **ASP.NET Core** e selecione **Seguinte**.

1. Selecione **Serviço do Kubernetes** e escolha **Seguinte**.  

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurar o VSTS e uma subscrição do Azure

1. Crie uma **nova** conta VSTS ou escolha uma conta **existente**.  Escolha um **nome** para o seu projeto VSTS.  

1. Selecione a sua **subscrição do Azure**.

1. Selecione a ligação **Alterar** para ver as definições adicionais de configuração do Azure e identificar o **número de nós** do **cluster do Kubernetes**.  Aqui, existem várias opções para configurar o tipo e a localização de serviços do Azure.
 
1. Saia da área de configuração do Azure e escolha **Concluído**.

1. A conclusão do processo poderá demorar alguns minutos.  Uma aplicação ASP.NET Core de exemplo é configurada num repositório da sua conta VSTS, é criado um cluster do AKS, é executada uma compilação e uma versão, e a sua aplicação é implementada no Azure.  

    Depois de concluído, o **dashboard do projeto** de DevOps do Azure é carregado no portal do Azure.  Também pode navegar para o **Dashboard do Projeto de DevOps do Azure** diretamente a partir de **Todos os recursos** no **portal do Azure**.  

    Este dashboard fornece visibilidade para o **repositório de código** do VSTS, o **pipeline de CI/CD do VSTS** e o **cluster do AKS**.  Pode configurar opções adicionais no VSTS.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.

## <a name="examine-the-aks-cluster"></a>Examinar o cluster do AKS

O projeto de DevOps do Azure configura automaticamente um cluster do AKS.  Pode explorar e personalizar o cluster.  Siga os passos abaixo para familiarizar-se com o AKS.

1. Navegue para o **dashboard do Projeto de DevOps do Azure**.

1. No lado direito do dashboard do projeto de DevOps, selecione o **serviço do Kubernetes**.

1. É aberto um painel para o cluster do AKS.  Nesta vista, pode executar várias ações, como **monitorizar o estado de funcionamento do contentor**, **pesquisar registos** e abrir o **dashboard do Kubernetes**.

1. No lado direito do ecrã, selecione **Ver o dashboard do Kubernetes**.  Opcionalmente, siga os passos para abrir o dashboard do Kubernetes.

## <a name="examine-the-vsts-ci-build-definition"></a>Examinar a definição de Compilação de CI do VSTS

O Projeto de DevOps do Azure configura automaticamente um pipeline de CI/CD completo do VSTS na sua conta VSTS.  Pode explorar e personalizar o pipeline.  Siga os passos abaixo para familiarizar-se com a definição de compilação do VSTS.

1. Navegue para o **dashboard do Projeto de DevOps do Azure**.

1. Selecione **Criar Pipelines** na **parte superior** do **dashboard do projeto de DevOps do Azure**.  Esta ligação abre um separador do browser e abre a definição de compilação do VSTS para o novo projeto.

1. Mova o cursor do rato para o lado direito da definição da compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode executar várias atividades, como **colocar uma nova compilação em fila**, **colocar uma compilação em pausa** e **editar a definição de compilação**.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** da sua definição de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git do VSTS, restaurar dependências e publicar as saídas utilizadas para implementações.

1. Na parte superior da definição de compilação, selecione o **nome da definição de compilação**.

1. Altere o **nome** da sua definição de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome da definição de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  O VSTS mantém um registo de todas as alterações efetuadas na definição de compilação e permite-lhe comparar versões.

1. Selecione **Acionadores**.  O projeto de DevOps do Azure criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Examinar a definição de Gestão de Versões de CD do VSTS

O Projeto de DevOps do Azure cria e configura automaticamente os passos necessários para implementar da sua conta VSTS para a sua subscrição do Azure.  Estes passos incluem a configuração de uma ligação de serviço do Azure para autenticar o VSTS para a sua subscrição do Azure.  A automatização também cria uma Definição de Versão do VSTS, e a Definição de Versão fornece a CD para o Azure.  Siga os passos abaixo para examinar mais sobre a Definição de Versão do VSTS.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O projeto de DevOps do Azure criou uma definição de versão do VSTS para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto à definição de versão e, em seguida, escolha **Editar**.

1. A definição de versão contém um **pipeline**, que define o processo de lançamento.  Em **Artefactos**, selecione **Remover**.  A definição de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **ícone** **Acionador de implementação contínua** (que é apresentado como um relâmpago.)  Esta definição de versão tem um acionador de CD ativado.  O acionador cria uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Estão disponíveis vários menus para explorar nesta vista, como um **resumo de versões**, **itens de trabalho associados** e **Testes**.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. Pode comparar versões para ver as diferenças de consolidação entre implementações.

1. Selecione **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Consolidar as alterações ao VSTS e implementar automaticamente no Azure 

 > [!NOTE]
 > Os passos abaixo testam o pipeline de CI/CD com uma alteração de texto simples à sua aplicação Web.

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório git do VSTS inicia uma compilação no VSTS, e uma definição de Gestão de Versões do VSTS implementa as alterações no Azure.  Siga os passos abaixo ou utilize outras técnicas para consolidar as alterações ao seu repositório.  Por exemplo, pode **clonar** o repositório Git na sua ferramenta favorita ou IDE e, em seguida, enviar as alterações para este repositório.

1. Selecione **Código** e, em seguida, **Ficheiros** no menu do VSTS e navegue para o seu repositório.
1. Navegue para o diretório **Vistas\Home**, selecione as **reticências** junto ao ficheiro **Index.cshtml** e, em seguida, escolha **Editar**.

1. Faça uma alteração no ficheiro, como algum texto dentro de uma da **etiquetas div**.  Na parte superior direita, selecione **Consolidar**.  Selecione **Consolidar** novamente para emitir a alteração. 

1. Dentro de momentos, uma **compilação é iniciada no VSTS** e, em seguida, uma versão é executada para implementar as alterações.  Pode monitorizar o **estado da compilação** com o dashboard do projeto de DevOps ou no browser com a sua conta VSTS.

1. Assim que a versão estiver concluída, **atualize a aplicação** no browser para verificar a ver as alterações.

## <a name="clean-up-resources"></a>Limpar recursos

 > [!NOTE]
 > Os passos abaixo irão eliminar permanentemente os recursos.  Utilize esta funcionalidade só depois de ler atentamente as instruções.

Se estiver a testar, pode limpar os recursos para evitar a criação de encargos de faturação.  Quando já não for necessário, pode eliminar o cluster do Azure Kubernetes e os recursos relacionados que criou neste tutorial, utilizando a funcionalidade **Eliminar** do dashboard do Projeto de DevOps do Azure.  **Tenha cuidado**, uma vez que a funcionalidade de eliminação destrói os dados criados pelo Projeto de DevOps do Azure, tanto no Azure como no VSTS, e não conseguirá recuperá-los depois.

1. No **portal do Azure**, navegue para o **Projeto de DevOps do Azure**.
2. No **lado superior direito** do dashboard, selecione **Eliminar**.  Depois de ler a instrução, selecione **Sim** para **eliminar permanentemente** os recursos.

## <a name="next-steps"></a>Passos seguintes

Opcionalmente, pode modificar estas definições de compilação e versão para satisfazer as necessidades da sua equipa. Também pode utilizar este padrão de CI/CD como um modelo para outros projetos.  Aprendeu a:

> [!div class="checklist"]
> * Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET Core e AKS
> * Configurar o VSTS e uma subscrição do Azure 
> * Examinar o cluster do AKS
> * Examinar a definição de Compilação de CI do VSTS
> * Examinar a definição de Gestão de Versões de CD do VSTS
> * Consolidar as alterações ao VSTS e implementar automaticamente no Azure
> * Limpar recursos

Para saber mais sobre como utilizar o dashboard do Kubernetes, veja abaixo:

> [!div class="nextstepaction"]
> [Utilizar o dashboard do Kubernetes](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
