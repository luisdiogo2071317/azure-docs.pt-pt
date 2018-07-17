---
title: Implementar a Aplicação ASP.NET e a Base de Dados SQL do Azure com o Projeto de DevOps do Azure | Tutorial do VSTS
description: O Projeto de DevOps permite começar a utilizar o Azure mais facilmente. O projeto de DevOps do Azure facilita a implementação de um ASP.NET na Base de Dados SQL do Azure em alguns passos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: ba15a57f92ad58a46732444890d9e670fcc75cf3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967359"
---
# <a name="tutorial--deploy-your-aspnet-app-and-azure-sql-database-with-the-azure-devops-project"></a>Tutorial: Implementar a Aplicação ASP.NET e a Base de Dados SQL do Azure com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada com a qual pode utilizar o seu código existente e o repositório de Git, ou escolher um dos exemplos de aplicação, para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.  O projeto de DevOps cria automaticamente recursos do Azure, como a Base de Dados SQL do Azure, um pipeline de versão no VSTS, que inclui uma definição de compilação para CI, configura uma definição de versão para CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Irá:

> [!div class="checklist"]
> * Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET e Base de Dados SQL do Azure
> * Configurar o VSTS e uma subscrição do Azure 
> * Examinar a definição de compilação de CI do VSTS
> * Examinar a definição de Gestão de Versões de CD do VSTS
> * Consolidar as alterações ao VSTS e implementar automaticamente no Azure
> * Ligar à Base de Dados do SQL Server do Azure 
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app-and-azure-sql-database"></a>Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET e Base de Dados SQL do Azure

O Projeto de DevOps do Azure cria um pipeline de CI/CD no VSTS.  Pode criar uma **nova conta VSTS** ou utilizar uma **conta existente**.  O Projeto de DevOps do Azure também cria **recursos do Azure**, como uma Base de Dados SQL do Azure na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **Criar um recurso** na barra de navegação esquerda e, em seguida, procure **Projeto DevOps**.  Escolha **Criar**.

    ![Iniciar a Entrega Contínua](_img/azure-devops-project-github/fullbrowser.png)

1. Selecione **.NET** e escolha **Seguinte**.

1. Em **Escolha uma arquitetura de aplicação**, selecione **ASP.NET**.

1. Selecione **Adicionar uma base de dados** e, em seguida, escolha **Seguinte**.  

1. A arquitetura de aplicações que escolheu nos passos anteriores dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Selecione **Seguinte**.

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurar o VSTS e uma subscrição do Azure

1. Crie uma **nova** conta VSTS ou escolha uma conta **existente**.  Escolha um **nome** para o seu projeto VSTS.  

1. Selecione a sua **subscrição do Azure**.

1. Opcionalmente, selecione a ligação **Alterar** para ver as definições de configuração do Azure adicionais e identifique o **nome de utilizador** na secção **Detalhes do Início de Sessão do Servidor da Base de Dados**.  **Guarde** o **nome de utilizador** para os passos seguintes deste tutorial.
 
1. Caso tenha realizado o passo acima, saia da área de configuração do Azure e escolha **Concluído**.  Caso contrário, basta selecionar **Concluído**.

1. Irá demorar alguns minutos até que o processo esteja concluído.  Depois de concluído, o **dashboard do projeto** do Azure DevOps é carregado no portal do Azure.  Também pode navegar para o **Dashboard do Projeto de DevOps do Azure** diretamente a partir de **Todos os recursos** no **portal do Azure**.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.
    
## <a name="examine-the-vsts-ci-build-definition"></a>Examinar a definição de compilação de CI do VSTS

O Projeto de DevOps do Azure configura automaticamente um pipeline VSTS CI/CD completo na sua conta do VSTS.  Pode explorar e personalizar o pipeline.  Siga os passos abaixo para familiarizar-se com a definição de compilação do VSTS.

1. Navegue para o **dashboard do Projeto de DevOps do Azure**.

1. Selecione **Criar Pipelines** na **parte superior** do **dashboard do projeto de DevOps do Azure**.  Esta ligação abre um separador do browser e abre a definição de compilação do VSTS para o novo projeto.

1. Mova o cursor do rato para o lado direito da definição da compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode executar várias atividades, como **colocar uma nova compilação em fila**, **colocar uma compilação em pausa** e **editar a definição de compilação**.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** da sua definição de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git do VSTS, restaurar dependências e publicar saídas utilizadas para implementações.

1. Na parte superior da definição de compilação, selecione o **nome da definição de compilação**.

1. Altere o **nome** da sua definição de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome da definição de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  O VSTS mantém um registo de todas as alterações realizadas na definição de compilação e permite-lhe comparar versões.

1. Selecione **Acionadores**.  O projeto de DevOps do Azure criou automaticamente um acionador de CI, e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Examinar a definição de Gestão de Versões de CD do VSTS

O Projeto de DevOps do Azure cria e configura automaticamente os passos que são preciso para implementar da sua conta VSTS para a sua subscrição do Azure.  Estes passos incluem a configuração de uma ligação de serviço do Azure para autenticar o VSTS para a sua subscrição do Azure.  A automatização também cria uma Definição de Versão do VSTS e a versão disponibiliza a CD para o Azure.  Siga os passos abaixo para examinar mais sobre a Definição de Versão do VSTS.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O projeto de DevOps do Azure criou uma definição de versão do VSTS para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto à definição de versão e, em seguida, escolha **Editar**.

1. A definição de versão contém um **pipeline**, que define o processo da versão.  Em **Artefactos**, selecione **Remover**.  A definição de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **ícone** **Acionador de implementação contínua** (que é apresentado como um relâmpago.)  Esta definição de versão tem um acionador de CD ativado.  O acionador inicia uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. O projeto de DevOps do Azure configura uma palavra-passe SQL aleatória e utiliza-a para a definição de versão.  No lado esquerdo do browser, selecione **Variáveis**. 

1. **Execute este passo apenas se tiver alterado a palavra-passe do SQL Server.**  Existe uma única variável de **Palavra-passe**.  À direita da caixa de texto **Valor**, selecione o ícone de **cadeado**.  **Introduza** a nova palavra-passe e selecione **Guardar**.

1. No lado esquerdo do browser, selecione **Tarefas** e escolha o seu **ambiente**.  

1. As tarefas são as atividades que o seu processo de implementação realiza e estão agrupadas em **Fases**.  Existe uma fase única para esta definição de versão.  A fase contém uma tarefa de **implementação de Serviço de Aplicações do Azure** e de **Implementação de Base de Dados SQL do Azure**.

1. Selecione a tarefa **Executar SQL do Azure** e examine as diversas propriedades utilizadas para a implementação de SQL.  Sob o aviso **Pacote de Implementação**, observe que a tarefa utiliza um **ficheiro SQL DACPAC**.

1. No lado direito do navegador, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Existem vários menus para explorar a partir desta vista, como um **resumo da versão**, **itens relacionados com trabalho** e **Testes**.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. Pode comparar versões para ver as diferenças de consolidação entre implementações.

1. Selecionar **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Consolidar as alterações ao VSTS e implementar automaticamente no Azure 

 > [!NOTE]
 > Os passos abaixo testam o pipeline de CI/CD com uma alteração de texto simples.  Opcionalmente, pode fazer uma alteração ao Esquema do SQL Server para a tabela testar o processo de implementação SQL.

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório git do VSTS inicia uma compilação no VSTS e uma definição de Gestão de Versões do VSTS executa uma implementação no Azure.  Siga os passos abaixo ou utilize outras técnicas para consolidar as alterações ao seu repositório.  As alterações de código iniciam o processo de CI/CD e implementam automaticamente as novas alterações no Azure.

1. Selecione **Código** no menu do VSTS e navegue para o seu repositório.

1. Navegue para o diretório **SampleWebApplication\Vistas\Home**, selecione as **reticências** junto ao ficheiro **Index.cshtml** e, em seguida, escolha **Editar**.

1. Faça uma alteração no ficheiro, como algum texto dentro de uma das **etiquetas div**.  No canto superior direito, selecione **Consolidar**.  Selecione **Consolidar** novamente para emitir a alteração. 

1. Dentro de momentos, uma **é iniciada no compilação VSTS** e, em seguida, é executada uma versão para implementar as alterações.  Pode monitorizar o **estado da compilação** com o dashboard do projeto de DevOps ou no browser com a sua conta VSTS.

1. Assim que a versão estiver concluída, **atualize a aplicação** no browser para verificar as alterações.

## <a name="connect-to-the-azure-sql-server-database"></a>Ligar à Base de Dados do SQL Server do Azure

Precisa de permissões adequadas para ligar à Base de Dados SQL do Azure.

1. No dashboard do Projeto de DevOps do Azure, selecione a **Base de Dados SQL** para navegar para a página de gestão da BD SQL.
   
1. Selecione **Definir firewall do servidor** e, em seguida, selecione **+ Adicionar IP de cliente**.  

1. Selecione **Guardar**.  O IP do cliente tem agora permissão para aceder ao **recurso do SQL Server do Azure**.

1. Navegue novamente para o painel **Base de Dados SQL**. 

1. No lado direito do ecrã, selecione o **Nome do servidor** para navegar para a página de configuração para o **SQL Server**.

1. Selecione **Repor palavra-passe**, introduza uma palavra-passe para o **Início de sessão de administrador do SQL Server** e, em seguida, selecione **Guardar**.  **Mantenha** esta palavra-passe para os passos seguintes deste tutorial.

1. Pode agora, opcionalmente, utilizar ferramentas de cliente, como o **SQL Server Management Studio** ou o **Visual Studio** para ligar ao Servidor e Base de Dados SQL do Azure.  Utilize a propriedade **Nome do servidor** para ligar.

   Se não alterou o nome de utilizador da DB ao configurar inicialmente o projeto do DevOps, o seu nome de utilizador é a parte local do seu endereço de e-mail.  Por exemplo, se o seu endereço de e-mail é johndoe@microsoft.com, o seu nome de utilizador é johndoe.

 > [!NOTE]
 > Se alterar a palavra-passe do início de sessão SQL, tem de alterar a palavra-passe da variável de definição de versão do VSTS, conforme descrito na secção **Examinar a definição de Gestão de Versões de CD do VSTS**

## <a name="clean-up-resources"></a>Limpar recursos

 > [!NOTE]
 > Os passos abaixo irão eliminar permanentemente os recursos.  Utilize esta funcionalidade só depois de ler atentamente as instruções.

Se estiver a testar, pode limpar os recursos para evitar a acumulação de encargos de faturação.  Quando já não for preciso, pode eliminar a Base de Dados SQL do Azure e os recursos relacionados que criou neste tutorial, através da funcionalidade **Eliminar** do dashboard do Projeto de DevOps do Azure.  **Tenha cuidado**, uma vez que a funcionalidade de eliminação destrói os dados criados pelo Projeto de DevOps do Azure, tanto no Azure como no VSTS, e não conseguirá recuperá-los depois.

1. No **portal do Azure**, navegue para o **Projeto de DevOps do Azure**.
2. No **canto superior direito** do dashboard, selecione **Eliminar**.  Depois de ler a instrução, selecione **Sim** para **eliminar permanentemente** os recursos.

## <a name="next-steps"></a>Passos seguintes

Opcionalmente, pode modificar estas definições de compilação e versão para satisfazer as necessidades da sua equipa. Também pode utilizar este padrão de CI/CD como um modelo para outros projetos.  Aprendeu a:

> [!div class="checklist"]
> * Criar um projeto de DevOps do Azure para uma Aplicação ASP.NET e Base de Dados SQL do Azure
> * Configurar o VSTS e uma subscrição do Azure 
> * Examinar a definição de compilação de CI do VSTS
> * Examinar a definição de Gestão de Versões de CD do VSTS
> * Consolidar as alterações ao VSTS e implementar automaticamente no Azure
> * Ligar à Base de Dados do SQL Server do Azure 
> * Limpar recursos

Para saber mais sobre o pipeline do VSTS, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Vídeos

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]