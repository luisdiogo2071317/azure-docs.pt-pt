---
title: Implementar a Aplicação ASP.NET em Máquinas Virtuais do Azure com o Projeto de DevOps do Azure | Tutorial do VSTS
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. O projeto de DevOps do Azure facilita a implementação de uma Aplicação ASP.NET em Máquinas Virtuais do Azure em alguns passos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b5a9ce204f68d812da4dfcfebb18b79b9c70c6c9
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967446"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Tutorial: Implementar a Aplicação ASP.NET em Máquinas Virtuais do Azure com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada com a qual pode utilizar o seu código existente e o repositório de Git ou escolher um dos exemplos de aplicação para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.  O projeto de DevOps cria automaticamente recursos do Azure, como uma nova máquina virtual do Azure, cria e configura um pipeline de versão no VSTS, que inclui uma definição de compilação para CI, configura uma definição de versão para CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Irá:

> [!div class="checklist"]
> * Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET
> * Configurar o VSTS e uma subscrição do Azure 
> * Examinar a definição de compilação de CI do VSTS
> * Examinar a definição de Gestão de Versões de CD do VSTS
> * Consolidar as alterações ao VSTS e implementar automaticamente no Azure
> * Configurar a monitorização do Azure Application Insights
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET

O Projeto de DevOps do Azure cria um pipeline de CI/CD no VSTS.  Pode criar uma **nova conta VSTS** ou utilizar uma **conta existente**.  O Projeto de DevOps do Azure também cria **recursos do Azure**, tais como máquinas virtuais na **subscrição do Azure** à sua escolha.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **+ Novo** na barra de navegação esquerda e, em seguida, procure **projeto de DevOps**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-github/fullbrowser.png)

1. Selecione **.NET** e escolha **Seguinte**.

1. Em **Escolha uma arquitetura de aplicação**, selecione **ASP.NET** e, em seguida, escolha **Seguinte**. 

1. A arquitetura de aplicação que escolheu nos passos anteriores, dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Selecione a **Máquina Virtual** e escolha **Seguinte**.

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurar o VSTS e uma subscrição do Azure

1. Crie uma **nova** conta VSTS ou escolha uma conta **existente**.  Escolha um **nome** para o seu projeto VSTS.  

1. Selecione a sua **subscrição do Azure**.  Opcionalmente, pode selecionar a ligação **Alterar** e, em seguida, introduzir mais detalhes de configuração, como alterar a localização dos recursos do Azure.
 
1. Introduza um **nome da Máquina Virtual**, **Nome de utilizador** e **Palavra-passe** para o seu novo recurso de máquina virtual do Azure e, em seguida, selecione **Concluído**.

1. Irá demorar vários minutos até que a máquina virtual do Azure esteja pronta.  Um exemplo de aplicação ASP.NET é configurado num repositório da sua conta VSTS, é executada uma compilação e uma versão, e a sua aplicação é implementada na VM do Azure recentemente criada.  

    Depois de concluído, o **dashboard do projeto** de DevOps do Azure é carregado no portal do Azure.  Também pode navegar até ao **Dashboard do Projeto de DevOps do Azure** diretamente a partir de **Todos os recursos** no **portal do Azure**.  

    Este dashboard oferece visibilidade para o **repositório de código** do VSTS, o **pipeline CI/CD do VSTS** e a sua **aplicação em execução no Azure**.    

    ![Vista do Dashboard](_img/azure-devops-project-vms/dashboardnopreview.png)

1. O projeto de DevOps do Azure configura automaticamente uma compilação de CI e um acionador de versão que implementa automaticamente quaisquer alterações de código no seu repositório.  Pode configurar opções adicionais no VSTS.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.
    
## <a name="examine-the-vsts-ci-build-definition"></a>Examinar a definição de compilação de CI do VSTS

O Projeto de DevOps do Azure configura automaticamente um pipeline CI/CD completo do VSTS na sua conta VSTS.  Pode explorar e personalizar o pipeline.  Siga os passos abaixo para familiarizar-se com a definição de compilação do VSTS.

1. Selecione **Criar Pipelines** na **parte superior** do **dashboard do projeto de DevOps do Azure**.  Esta ligação abre um separador do browser e abre a definição de compilação do VSTS para o novo projeto.

1. Mova o cursor do rato para o lado direito da definição da compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode realizar várias atividades, como **colocar uma nova compilação em fila**, **colocar uma compilação em pausa** e **editar a definição de compilação**.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** da sua definição de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git do VSTS, restaurar dependências e publicar saídas utilizadas para implementações.

1. Na parte superior da definição de compilação, selecione o **nome da definição de compilação**.

1. Altere o **nome** da sua definição de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome da definição de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  O VSTS mantém um registo de todas as alterações feitas na definição de compilação e permite-lhe comparar versões.

1. Selecione **Acionadores**.  O projeto de DevOps do Azure criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Examinar a definição de Gestão de Versões de CD do VSTS

O Projeto de DevOps do Azure cria e configura automaticamente os passos necessários para implementar da sua conta VSTS para a sua subscrição do Azure.  Estes passos incluem a configuração de uma ligação de serviço do Azure para autenticar o VSTS para a sua subscrição do Azure.  A automatização também cria uma Definição de Versão do VSTS, e esta fornece a CD para a máquina virtual do Azure.  Siga os passos abaixo para examinar mais sobre a Definição de Versão do VSTS.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O projeto de DevOps do Azure criou uma definição de versão do VSTS para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto à definição de versão e, em seguida, escolha **Editar**.

1. A definição de versão contém um **pipeline**, que define o processo de lançamento.  Em **Artefactos**, selecione **Remover**.  A definição de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **ícone** **Acionador de implementação contínua** (que é apresentado como um relâmpago.)  Esta definição de versão tem um acionador de CD ativado.  O acionador inicia uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado esquerdo do browser, selecione **Tarefas** e, em seguida, escolha o seu **ambiente**.  

1. As tarefas são as atividades que o seu processo de implementação executa e estão agrupadas em **Fases**.  Existem duas **Fases** para esta definição de versão.  A primeira fase contém uma tarefa de **Implementação do Grupo de Recursos do Azure**, que configura a VM para implementação e adiciona a nova VM a um **Grupo de Implementação do VSTS**.  O grupo de implementação da VM no VSTS gere os grupos lógicos das máquinas de **destino da implementação**.

1. Nesta segunda fase, foi criada uma tarefa de **Gestão da Aplicação Web do IIS** para criar um Site do IIS na VM.  Uma segunda tarefa de **Implementação da Aplicação Web do IIS** foi criada para implementar o site.

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Estão disponíveis vários menus para explorar nesta vista, como um **resumo de versões**, **itens de trabalho associados** e **Testes**.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. Compare as versões para ver as diferenças de consolidação entre implementações.

1. Selecione **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Consolidar as alterações ao VSTS e implementar automaticamente no Azure 

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório git do VSTS inicia uma compilação no VSTS e uma definição de Gestão de Versões do VSTS executa uma implementação na sua VM do Azure.  Siga os passos abaixo ou utilize outras técnicas para consolidar as alterações ao seu repositório.  As alterações de código iniciam o processo de CI/CD e implementam automaticamente as novas alterações no site do IIS na VM do Azure.

1. Selecione **Código** no menu do VSTS e navegue para o seu repositório.

1. Navegue para o diretório **Vistas\Home**, selecione as **reticências** junto ao ficheiro **Index.cshtml** e, em seguida, escolha **Editar**.

1. Faça uma alteração ao ficheiro, como algum texto dentro de uma da **etiquetas div**.  Na parte superior direita, selecione **Consolidar**.  Selecione **Consolidar** novamente para emitir a alteração. 

1. Dentro de momentos, uma **compilação é iniciada no VSTS** e, em seguida, uma versão é executada para implementar as alterações.  Monitorize o **estado da compilação** com o dashboard do projeto de DevOps ou no browser com a sua conta VSTS.

1. Assim que a versão estiver concluída, **atualize a aplicação** no browser para verificar se vê as alterações.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar a monitorização do Azure Application Insights

Com o Azure Application Insights, pode monitorizar facilmente o desempenho e a utilização da aplicação.  O projeto de DevOps do Azure configurou automaticamente um recurso do Application Insights para a sua aplicação.  Pode configurar vários alertas e capacidades de monitorização, conforme necessário.

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

Se estiver a testar, pode limpar os recursos para evitar a acumulação de encargos de faturação.  Quando já não for necessário, pode eliminar a máquina virtual do Azure e os recursos relacionados que criou neste tutorial, utilizando a funcionalidade **Eliminar** do dashboard do Projeto de DevOps do Azure.  **Tenha cuidado**, uma vez que a funcionalidade de eliminação destrói os dados criados pelo Projeto de DevOps do Azure, tanto no Azure como no VSTS, e não conseguirá recuperá-los depois.

1. No **portal do Azure**, navegue para o **Projeto de DevOps do Azure**.
2. No **lado superior direito** do dashboard, selecione **Eliminar**.  Depois de ler a instrução, selecione **Sim** para **eliminar permanentemente** os recursos.

## <a name="next-steps"></a>Passos seguintes

Opcionalmente, pode modificar estas definições de compilação e versão para satisfazer as necessidades da sua equipa. Também pode utilizar este padrão de CI/CD como um modelo para outros projetos.  Aprendeu a:

> [!div class="checklist"]
> * Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET
> * Configurar o VSTS e uma subscrição do Azure 
> * Examinar a definição de compilação de CI do VSTS
> * Examinar a definição de Gestão de Versões de CD do VSTS
> * Consolidar as alterações ao VSTS e implementar automaticamente no Azure
> * Configurar a monitorização do Azure Application Insights
> * Limpar recursos

Para saber mais sobre o pipeline do VSTS, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
