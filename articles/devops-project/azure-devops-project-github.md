---
title: Criar um pipeline de CI/CD para o código existente com o Projeto de DevOps do Azure | Tutorial dos Serviços DevOps do Azure
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. Ajuda-o a utilizar o seu próprio código e o repositório GitHub para lançar uma aplicação num serviço do Azure à sua escolha com alguns passos rápidos.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 4e0e28ff9ea14e42e1df7ce35bb90e8720a0d0b6
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407302"
---
# <a name="create-a-cicd-pipeline-for-your-existing-code-with-the-azure-devops-project"></a>Criar um pipeline de CI/CD para o seu código existente com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada, onde pode utilizar o seu código existente e o repositório Git, ou escolher um dos exemplos de aplicação, para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.

Vai:

> [!div class="checklist"]
> * Criar um Projeto de DevOps do Azure
> * Configurar o acesso ao seu repositório GitHub e escolher uma arquitetura
> * Configurar os Serviços DevOps do Azure e uma subscrição do Azure 
> * Consolidar as alterações ao GitHub e implementar automaticamente no Azure
> * Examinar o pipeline de CI/CD dos Serviços DevOps do Azure
> * Configurar a monitorização do Azure Application Insights

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).
* Acesso a um repositório GitHub ou Git externo que contenha código .NET, Java, PHP, Node, Python ou código Web estático.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

O Projeto de DevOps do Azure cria um pipeline de CI/CD nos Serviços DevOps do Azure.  Pode criar uma organização **nova dos Serviços DevOps do Azure** ou utilizar uma **organização existente**.  O projeto de DevOps do Azure também cria **recursos do Azure** na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **+ Novo** na barra de navegação esquerda e, em seguida, procure **Projeto de DevOps**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-github/fullbrowser.png)

1. Selecione **Traga o seu próprio código**.  Quando tiver terminado, escolha **Seguinte**.

## <a name="configure-access-to-your-github-repository-and-choose-a-framework"></a>Configurar o acesso ao seu repositório GitHub e escolher uma arquitetura

1. Selecione **GitHub**.  Opcionalmente, pode escolher um **repositório Git externo**.  Escolha o seu **repositório** e o **ramo** que contém a sua aplicação.

1. Selecione a sua **arquitetura Web**.  Quando tiver terminado, escolha **Seguinte**.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

1. A arquitetura de aplicações que escolheu nos passos anteriores dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Selecione o **serviço de destino** que escolher.  Quando tiver terminado, escolha **Seguinte**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configurar os Serviços DevOps do Azure e uma subscrição do Azure 

1. Crie uma organização **nova** dos Serviços DevOps do Azure ou utilize uma organização **existente**.  Escolha um **nome** para o seu Projeto de DevOps do Azure.  Selecione a sua **subscrição do Azure**, a **localização** e escolha um **nome** para a sua aplicação.  Quando tiver terminado, escolha **Concluído**.

1. Dentro de alguns minutos, o **dashboard do Projeto de DevOps do Azure** é carregado no portal do Azure.  Um exemplo de aplicação é configurado num repositório da sua organização de Serviços DevOps, uma compilação é executada e a sua aplicação é implementada no Azure.  Este dashboard oferece visibilidade para o seu **repositório de código** do GitHub, o **pipeline CI/CD dos Serviços DevOps do Azure** e a sua **aplicação em execução no Azure**.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.

    ![Vista do Dashboard](_img/azure-devops-project-github/dashboardnopreview.png) 
    
O Projeto de DevOps do Azure configura automaticamente um acionador de lançamento e de compilação de CI.  O código permanece no seu repositório GitHub ou noutro repositório externo.  

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Consolidar as alterações ao GitHub e implementar automaticamente no Azure 

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório GitHub inicia uma compilação no DevOps do Azure e um pipeline de CD de DevOps do Azure executa uma implementação no Azure.

1.  Faça uma alteração na aplicação, e **consolide** a alteração no seu repositório GitHub.
2.  Dentro de momentos, uma compilação é iniciada nos Serviços DevOps do Azure.  Pode monitorizar o estado da compilação com o dashboard do Projeto de DevOps do Azure ou no browser com a sua organização de Serviços DevOps do Azure.
3.  Assim que a compilação estiver concluída, **atualize a aplicação** no browser para verificar a ver as alterações.

## <a name="examine-the-azure-devops-services-cicd-pipeline"></a>Examinar o pipeline de CI/CD dos Serviços DevOps do Azure

O Projeto de DevOps do Azure configurou automaticamente um pipeline de CI/CD dos Serviços DevOps do Azure na sua organização dos Serviços DevOps do Azure.  Explore e personalize o pipeline, conforme necessário.  Siga os passos abaixo para se familiarizar com os pipelines de compilação e de lançamento dos Serviços DevOps do Azure.

1. Selecione **Pipelines de Compilação** na **parte superior** do dashboard do Projeto de DevOps do Azure.  Esta ligação abre um separador do browser e abre o pipeline de compilação dos Serviços DevOps do Azure para o novo projeto.

1. Mova o cursor do rato para o lado direito do pipeline de compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode iniciar várias atividades, como colocar uma nova compilação em fila, colocar uma compilação em pausa e editar o pipeline de compilação.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** do pipeline de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git, restaurar dependências e publicar as saídas utilizadas para implementações.

1. Na parte superior do pipeline de compilação, selecione o **nome do pipeline de compilação**.

1. Altere o **nome** do pipeline de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome do pipeline de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  Os Serviços DevOps do Azure mantêm um registo de todas as alterações efetuadas no pipeline de compilação e permitem-lhe comparar versões.

1. Selecione **Acionadores**.  O Projeto de DevOps do Azure criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O Projeto de DevOps do Azure criou um pipeline de lançamento dos Serviços DevOps do Azure para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto ao pipeline de lançamento e, em seguida, escolha **Editar**.

1. O pipeline de lançamento contém um **pipeline**, que define o processo de lançamento.  Em **Artefactos**, selecione **Remover**.  O pipeline de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **Acionador de implementação contínua**.  Este pipeline de lançamento tem um acionador de CD ativado, que executa uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado esquerdo do browser, selecione **Tarefas**.  As tarefas são as atividades que o processo de implementação executa.  Neste exemplo, foi criada uma tarefa para implementar no **Serviço de Aplicações do Azure**.

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Estão disponíveis vários menus para explorar nesta vista, como um resumo de versões, itens de trabalho associados e testes.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. 

1. Selecionar **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar a monitorização do Azure Application Insights

Com o Azure Application Insights, pode monitorizar facilmente o desempenho e a utilização da aplicação.  O Projeto de DevOps do Azure configurou automaticamente um recurso do Application Insights para a sua aplicação.  Pode configurar vários alertas e capacidades de monitorização, conforme necessário.

1. Navegue para o dashboard do **Projeto de DevOps do Azure** no portal do Azure.  No canto inferior direito do dashboard, escolha a ligação ao **Application Insights** para a sua aplicação.

1. O painel **Application Insights** abre no portal do Azure.  Esta vista contém informações sobre a monitorização da utilização, do desempenho e da disponibilidade da aplicação.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Selecione **Intervalo de tempo** e, em seguida, escolha **Última hora**.  Selecione **Atualizar** para filtrar os resultados.  Pode agora ver todas as atividades dos últimos 60 minutos.  Selecione o **x** para sair do intervalo de tempo.

1. Selecione **Alertas** e, em seguida, selecione **+ Adicionar alerta de métrica**.  

1. Introduza um **Nome** para o alerta.

1. Na lista pendente, selecione **Alterar Origem em**.  Selecione o **recurso do Serviço de Aplicações.**
<!-- Could you please confirm if this should be "Source Alter on" instead of "Source Alert on"? -->
1. O alerta predefinido é para um **tempo de resposta de servidor superior a 1 segundo**.  Selecione o menu pendente **Métrica** para examinar as várias métricas de alerta.  Pode configurar facilmente vários alertas, para melhorar as capacidades de monitorização da aplicação.

1. Selecione a caixa de verificação para **Notificar através de proprietários de e-mail, contribuidores e leitores**.  Opcionalmente, pode realizar ações adicionais quando um alerta é acionado, executando uma aplicação lógica do Azure.

1. Escolha **Ok** para criar o alerta.  Dentro de momentos, o alerta aparece como ativo no dashboard.  **Saia** da área Alertas e navegue de volta para o **painel do Application Insights**.

1. Selecione **Disponibilidade** e, em seguida, selecione **+ Adicionar teste**. 

1. Introduza o **Nome do teste** e, em seguida, escolha **Criar**.  Um teste de ping simples é criado para verificar a disponibilidade da sua aplicação.  Após alguns minutos, os resultados do teste ficam disponíveis e o dashboard do Application Insights apresenta um estado de disponibilidade.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o serviço de Aplicações do Azure e os recursos relacionados que criou neste início rápido, utilizando a funcionalidade **Eliminar** do dashboard do projeto de DevOps do Azure.

## <a name="next-steps"></a>Passos seguintes

Quando configurou o processo de CI/CD neste tutorial, um pipeline de compilação e um pipeline de lançamento foram automaticamente criados no Projeto de DevOps do Azure. Pode modificar estes pipelines de compilação e de lançamento para satisfazer as necessidades da sua equipa. Aprendeu a:

> [!div class="checklist"]
> * Criar um Projeto de DevOps do Azure
> * Configurar o acesso ao seu repositório GitHub e escolher uma arquitetura
> * Configurar os Serviços DevOps do Azure e uma subscrição do Azure 
> * Consolidar as alterações ao GitHub e implementar automaticamente no Azure
> * Examinar o pipeline de CI/CD dos Serviços DevOps do Azure
> * Configurar a monitorização do Azure Application Insights

Para saber mais sobre o pipeline de CI/CD dos Serviços DevOps do Azure, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
