---
title: Criar um pipeline CI/CD para o Ruby on Rails com o Projeto de DevOps do Azure | Início Rápido
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. Ele ajuda-o a iniciar uma aplicação Web Ruby num serviço do Azure em alguns passos rápidos.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 773e77e0965dfb059460faf564d8cc3d80cef3ea
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967011"
---
# <a name="create-a-cicd-pipeline-for-ruby-on-rails-with-the-azure-devops-project"></a>Criar um pipeline CI/CD para o Ruby on Rails com o Projeto de DevOps do Azure

Configure a integração contínua (CI) e a entrega contínua (CD) da sua aplicação Ruby on Rails com o **Projeto de DevOps do Azure**.  O projeto de DevOps do Azure simplifica a configuração inicial de uma compilação VSTS e pipeline de versão.

Se não tiver uma subscrição do Azure, pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

O Projeto de DevOps do Azure cria um pipeline de CI/CD no VSTS.  Pode criar uma **nova conta VSTS** ou utilizar uma **conta existente**.  O projeto de DevOps do Azure também cria **recursos do Azure** na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **Criar um recurso** na barra de navegação esquerda e, em seguida, procure **Projeto DevOps**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-ruby/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Selecione um exemplo de aplicação e serviço do Azure

1. Selecione o exemplo de aplicação **Ruby**.

1. Selecione o framework da aplicação **Ruby on Rails**.  Quando tiver terminado, escolha **Seguinte**.

1. **A Aplicação Web no Linux** é o destino de implementação predefinido.  Opcionalmente, pode escolher aplicação Web para Contentores.  A arquitetura de aplicações que escolheu nos passos anteriores dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Selecione o **serviço de destino** que escolher.  Quando tiver terminado, escolha **Seguinte**.

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurar o VSTS e uma subscrição do Azure 

1. Crie uma **nova** conta gratuita VSTS ou escolha uma conta **existente**.  Escolha um **nome** para o seu projeto VSTS.  Selecione a sua **subscrição do Azure**, a **localização** e escolha um **nome** para a sua aplicação.  Quando tiver terminado, escolha **Concluído**.

    ![Introduzir informações do VSTS](_img/azure-devops-project-ruby/vstsazureinfo.png)

1. Dentro de alguns minutos, o **dashboard do projeto** é carregado no portal do Azure.  Um exemplo de aplicação é configurado num repositório da sua conta VSTS, uma compilação é executada e a sua aplicação é implementada no Azure.  Este dashboard oferece visibilidade para o **repositório de código** do VSTS, o **pipeline CI/CD do VSTS** e a sua **aplicação em execução no Azure**.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.

    ![Vista do Dashboard](_img/azure-devops-project-ruby/dashboardnopreview.png) 

## <a name="commit-code-changes-and-execute-cicd"></a>Consolidar as alterações de código e executar o CI/CD

O projeto de DevOps do Azure criou um repositório Git na sua conta VSTS ou GitHub.  Siga os passos abaixo para ver o repositório e fazer alterações de código na aplicação.

1. No lado esquerdo do dashboard do projeto de DevOps, selecione a ligação para o seu ramo **principal**.  Esta ligação abre uma vista para o repositório Git recentemente criado.

1. Para ver o URL de clone do repositório, selecione **Clone** na parte superior direita do browser. Pode clonar o repositório Git no seu IDE preferido.  Nos próximos passos, pode utilizar o browser para fazer e consolidar alterações de código diretamente no ramo principal.

1. No lado esquerdo do browser, navegue para o ficheiro **app/views/pages/home.html.erb**.

1. Selecione **Editar** e faça uma alteração ao texto.  Por exemplo, modifique o texto dentro de uma das etiquetas div.

1. Escolha **Consolidar** e guarde as alterações.

1. No browser, navegue para o **dashboard do projeto de DevOps do Azure**.  Deverá agora ver que está em curso uma compilação.  As alterações que acabou de fazer são automaticamente criadas e implementadas através de um pipeline de CI/CD do VSTS.

## <a name="examine-the-vsts-cicd-pipeline"></a>Examinar o pipeline de CI/CD do VSTS

O projeto de DevOps do Azure configurou automaticamente um pipeline de CI/CD completo do VSTS na sua conta VSTS.  Explore e personalize o pipeline, conforme necessário.  Siga os passos abaixo para familiarizar-se com as definições de compilação e versão do VSTS.

1. Selecione **Criar Pipelines** na **parte superior** do dashboard do projeto de DevOps do Azure.  Esta ligação abre um separador do browser e abre a definição de compilação do VSTS para o novo projeto.

1. Selecione as **reticências**.  Esta ação abre um menu onde pode iniciar várias atividades, como colocar uma nova compilação em fila, colocar uma compilação em pausa e editar a definição de compilação.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** da sua definição de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git do VSTS, restaurar dependências e publicar saídas utilizadas para implementações.

1. Na parte superior da definição de compilação, selecione o **nome da definição de compilação**.

1. Altere o **nome** da sua definição de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome da definição de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  O VSTS mantém um registo de todas as alterações efetuadas na definição de compilação e permite-lhe comparar versões.

1. Selecione **Acionadores**.  O projeto de DevOps do Azure criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O projeto de DevOps do Azure criou uma definição de versão do VSTS para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto à definição de versão e, em seguida, escolha **Editar**.

1. A definição de versão contém um **pipeline**, que define o processo de lançamento.  Em **Artefactos**, selecione **Remover**.  A definição de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **Acionador de implementação contínua**.  Esta definição de versão tem um acionador de CD ativado, que executa uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado esquerdo do browser, selecione **Tarefas**.  As tarefas são as atividades que o processo de implementação executa.  Neste exemplo, foi criada uma tarefa para implementar no **Serviço de Aplicações do Azure**.

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Estão disponíveis vários menus para explorar nesta vista, como um resumo de versões, itens de trabalho associados e testes.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. 

1. Selecionar **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for preciso, pode eliminar o serviço de Aplicações do Azure e os recursos relacionados que criou neste início rápido, através da função **Eliminar** do dashboard do projeto de DevOps do Azure.

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre como modificar as definições de compilação e versão para satisfazer as necessidades da sua equipa, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
