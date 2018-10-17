---
title: Implementar a Aplicação ASP.NET em Máquinas Virtuais do Azure com o Azure DevOps Project | Tutorial dos Serviços de DevOps do Azure
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. O Azure DevOps Project facilita a implementação de uma Aplicação ASP.NET em Máquinas Virtuais do Azure em alguns passos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b05e2c2c46aa9bfa8c92d3d3c5c83d018c547b9f
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299139"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Tutorial: Implementar a Aplicação ASP.NET em Máquinas Virtuais do Azure com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada com a qual pode utilizar o seu código existente e o repositório de Git, ou escolher um dos exemplos de aplicação, para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.  O DevOps Project cria automaticamente recursos do Azure, como uma nova máquina virtual do Azure, cria e configura um pipeline de versão no Azure DevOps, que inclui um pipeline de compilação para CI, configura um pipeline de versão para CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Irá:

> [!div class="checklist"]
> * Criar um Azure DevOps Project para uma Aplicação ASP.NET
> * Configurar os Serviços de DevOps do Azure e uma subscrição do Azure 
> * Examinar o pipeline de CI dos Serviços de DevOps do Azure
> * Examinar o pipeline de CD dos Serviços de DevOps do Azure
> * Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure
> * Configurar a monitorização do Azure Application Insights
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Criar um Azure DevOps Project para uma Aplicação ASP.NET

O Azure DevOps Project cria um pipeline de CI/CD no Azure.  Pode criar uma organização **nova dos Serviços de DevOps do Azure** ou utilizar uma **organização existente**.  O Projeto de DevOps do Azure também cria **recursos do Azure**, tais como máquinas virtuais na **subscrição do Azure** à sua escolha.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **+ Novo** na barra de navegação esquerda e, em seguida, procure **DevOps Project**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-github/fullbrowser.png)

1. Selecione **.NET** e escolha **Seguinte**.

1. Em **Escolha uma arquitetura de aplicação**, selecione **ASP.NET** e, em seguida, escolha **Seguinte**. 

1. A arquitetura de aplicação que escolheu nos passos anteriores, dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Selecione a **Máquina Virtual** e escolha **Seguinte**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configurar os Serviços de DevOps do Azure e uma subscrição do Azure

1. Crie uma organização **nova** dos Serviços de DevOps do Azure ou utilize uma organização **existente**.  Escolha um **nome** para o seu Azure DevOps Project.  

1. Selecione os seus **serviços de subscrição do Azure**.  Opcionalmente, pode selecionar a ligação **Alterar** e, em seguida, introduzir mais detalhes de configuração, como alterar a localização dos recursos do Azure.
 
1. Introduza um **nome da Máquina Virtual**, **Nome de utilizador** e **Palavra-passe** para o seu novo recurso de máquina virtual do Azure e, em seguida, selecione **Concluído**.

1. Irá demorar vários minutos até que a máquina virtual do Azure esteja pronta.  É configurada uma aplicação ASP.NET de exemplo num repositório da sua organização de Serviços de DevOps do Azure, é executada uma compilação e uma versão, e a sua aplicação é implementada na VM do Azure recentemente criada.  

    Depois de concluído, o **dashboard do projeto** do Azure DevOps é carregado no portal do Azure.  Também pode navegar para o **Dashboard do Projeto de DevOps do Azure** diretamente a partir de **Todos os recursos** no **portal do Azure**.  

    Este dashboard oferece visibilidade para o **repositório de código**, o **pipeline CI/CD do Azure** e a **aplicação em execução no Azure** dos Serviços de DevOps do Azure.    

    ![Vista do Dashboard](_img/azure-devops-project-vms/dashboardnopreview.png)

1. O Azure DevOps Project configura automaticamente uma compilação de CI e um acionador de versão que implementa automaticamente quaisquer alterações de código no seu repositório.  Pode configurar opções adicionais no Azure DevOps.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Examinar o pipeline de CI dos Serviços de DevOps do Azure
 
O Azure DevOps Project configura automaticamente um pipeline de CI/CD completo do Azure na sua organização dos Serviços DevOps do Azure.  Pode explorar e personalizar o pipeline.  Siga os passos abaixo para familiarizar-se com o pipeline de compilação dos Serviços de DevOps do Azure.

1. Selecione **Pipelines de Compilação** na **parte superior** do **dashboard do Azure DevOps Project**.  Esta ligação abre um separador do browser e abre o pipeline de compilação dos Serviços de DevOps do Azure para o novo projeto.

1. Mova o cursor do rato para o lado direito do pipeline de compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode executar várias atividades, como **colocar uma nova compilação em fila**, **colocar uma compilação em pausa** e **editar o pipeline de compilação**.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** do seu pipeline de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git dos Serviços de DevOps do Azure, restaurar dependências e publicar saídas utilizadas para implementações.

1. Na parte superior do pipeline de compilação, selecione o **nome do pipeline de compilação**.

1. Altere o **nome** do seu pipeline de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome do pipeline de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  Os Serviços de DevOps do Azure mantêm um registo de todas as alterações efetuadas no pipeline de compilação e permitem-lhe comparar versões.

1. Selecione **Acionadores**.  O Azure DevOps Project criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Examinar o pipeline de CD dos Serviços de DevOps do Azure

O Azure DevOps Project cria e configura automaticamente os passos necessários para implementar da sua organização de Serviços de DevOps do Azure para a sua subscrição do Azure.  Estes passos incluem a configuração de uma ligação ao serviço do Azure para autenticar os Serviços de DevOps do Azure para a sua subscrição do Azure.  A automatização também cria um pipeline de CD dos Serviços de DevOps do Azure e fornece a CD para a máquina virtual do Azure.  Siga os passos abaixo para examinar mais sobre o pipeline de CD dos Serviços de DevOps do Azure.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O Azure DevOps Project criou um pipeline de versão dos Serviços de DevOps do Azure para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto ao pipeline de versão e, em seguida, selecione **Editar**.

1. O pipeline de versão contém um **pipeline**, que define o processo da versão.  Em **Artefactos**, selecione **Remover**.  O pipeline de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **ícone** **Acionador de implementação contínua** (que é apresentado como um relâmpago.)  Este pipeline de versão tem um acionador de CD ativado.  O acionador inicia uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado esquerdo do browser, selecione **Tarefas** e, em seguida, escolha o seu **ambiente**.  

1. As tarefas são as atividades que o seu processo de implementação executa e estão agrupadas em **Fases**.  Existem duas **Fases** para este pipeline de versão.  A primeira fase contém uma tarefa de **Implementação do Grupo de Recursos do Azure**, que configura a VM para implementação e adiciona a nova VM a um **Grupo de Implementação do Azure DevOps**.  O grupo de implementação da VM no Azure DevOps gere os grupos lógicos das máquinas de **destino da implementação**.

1. Nesta segunda fase, foi criada uma tarefa de **Gestão da Aplicação Web do IIS** para criar um Site do IIS na VM.  Uma segunda tarefa de **Implementação da Aplicação Web do IIS** foi criada para implementar o site.

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Existem vários menus para explorar a partir desta vista, como um **resumo da versão**, **itens relacionados com trabalho** e **Testes**.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. Compare as versões para ver as diferenças de consolidação entre implementações.

1. Selecione **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure 

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório Git dos Serviços de DevOps do Azure inicia uma compilação nos Serviços de DevOps do Azure e um pipeline de CD dos Serviços de DevOps do Azure executa uma implementação na VM do Azure.  Siga os passos abaixo ou utilize outras técnicas para consolidar as alterações ao seu repositório.  As alterações de código iniciam o processo de CI/CD e implementam automaticamente as novas alterações no site do IIS na VM do Azure.

1. Selecione **Código** no menu dos Serviços de DevOps do Azure e navegue para o seu repositório.

1. Navegue para o diretório **Vistas\Home**, selecione as **reticências** junto ao ficheiro **Index.cshtml** e, em seguida, selecione **Editar**.

1. Faça uma alteração ao ficheiro, como algum texto dentro de uma das **etiquetas div**.  No canto superior direito, selecione **Consolidar**.  Selecione **Consolidar** novamente para emitir a alteração. 

1. Dentro de momentos, **é iniciada uma compilação nos Serviços de DevOps do Azure** e, em seguida, é executada uma versão para implementar as alterações.  Monitorize o **estado da compilação** com o dashboard do DevOps Project ou no browser com a sua organização de Serviços de DevOps do Azure.

1. Assim que a versão estiver concluída, **atualize a aplicação** no browser para verificar as alterações.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar a monitorização do Azure Application Insights

Com o Azure Application Insights, pode monitorizar facilmente o desempenho e a utilização da aplicação.  O Azure DevOps Project configurou automaticamente um recurso do Application Insights para a sua aplicação.  Pode configurar vários alertas e capacidades de monitorização, conforme necessário.

1. Navegue para o dashboard do **Projeto de DevOps do Azure** no **portal do Azure**.  No canto inferior direito do dashboard, escolha a ligação ao **Application Insights** para a sua aplicação.

1. O painel **Application Insights** abre no portal do Azure.  Esta vista contém informações sobre a monitorização da utilização, do desempenho e da disponibilidade da aplicação.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Selecione **Intervalo de tempo** e, em seguida, escolha **Última hora**.  Selecione **Atualizar** para filtrar os resultados.  Pode agora ver todas as atividades dos últimos 60 minutos.  Selecione o **x** para sair do intervalo de tempo.

1. Pode encontrar **Alertas** e várias outras ligações úteis perto da parte superior do dashboard.  Selecione **Alertas** e, em seguida, selecione **+ Adicionar alerta de métrica**.

1. Introduza um **Nome** para o alerta.

1. O alerta predefinido é para um **tempo de resposta de servidor superior a 1 segundo**.  Selecione o menu pendente **Métrica** para examinar as várias métricas de alerta.  Por exemplo, pode configurar o **tempo de execução de um pedido ASP.NET** para uma aplicação ASP.NET.  Pode configurar facilmente vários alertas para melhorar as capacidades de monitorização da aplicação.

1. Selecione a caixa de verificação para **Notificar através de proprietários de e-mail, contribuidores e leitores**.  Opcionalmente, pode realizar ações adicionais quando um alerta é acionado, executando uma aplicação lógica do Azure.

1. Escolha **Ok** para criar o alerta.  Dentro de momentos, o alerta aparece como ativo no dashboard.  **Saia** da área Alertas e navegue de volta para o **painel do Application Insights**.

1. Selecione **Disponibilidade** e, em seguida, selecione **+ Adicionar teste**. 

1. Introduza o **Nome do teste** e, em seguida, escolha **Criar**.  Esta ação cria um teste de ping simples para verificar a disponibilidade da sua aplicação.  Após alguns minutos, os resultados do teste ficam disponíveis e o dashboard do Application Insights apresenta um estado de disponibilidade.

## <a name="clean-up-resources"></a>Limpar recursos

 > [!NOTE]
 > Os passos abaixo irão eliminar permanentemente os recursos.  Utilize esta funcionalidade só depois de ler atentamente as instruções.

Se estiver a testar, pode limpar os recursos para evitar a acumulação de encargos de faturação.  Quando já não for necessário, pode eliminar a máquina virtual do Azure e os recursos relacionados que criou neste tutorial, utilizando a funcionalidade **Eliminar** do dashboard do Projeto de DevOps do Azure.  **Tenha cuidado**, uma vez que a funcionalidade de eliminação destrói os dados criados pelo Azure DevOps Project, tanto no Azure como no Azure DevOps, e não conseguirá recuperá-los depois.

1. No **portal do Azure**, navegue para o **Projeto de DevOps do Azure**.
2. No **canto superior direito** do dashboard, selecione **Eliminar**.  Depois de ler a instrução, selecione **Sim** para **eliminar permanentemente** os recursos.

## <a name="next-steps"></a>Passos seguintes

Opcionalmente, pode modificar estes pipelines de compilação e versão para satisfazer as necessidades da sua equipa. Também pode utilizar este padrão de CI/CD como um modelo para outros projetos.  Aprendeu a:

> [!div class="checklist"]
> * Criar um Azure DevOps Project para uma Aplicação ASP.NET
> * Configurar os Serviços de DevOps do Azure e uma subscrição do Azure 
> * Examinar o pipeline de CI dos Serviços de DevOps do Azure
> * Examinar o pipeline de CD dos Serviços de DevOps do Azure
> * Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure
> * Configurar a monitorização do Azure Application Insights
> * Limpar recursos

Para saber mais sobre o pipeline de CI/CD do Azure, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
