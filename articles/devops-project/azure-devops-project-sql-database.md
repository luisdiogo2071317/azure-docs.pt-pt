---
title: Implementar a Aplicação ASP.NET e a Base de Dados SQL do Azure com o Azure DevOps Project | Tutorial dos Serviços de DevOps do Azure
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. O Azure DevOps Project facilita a implementação de um ASP.NET na Base de Dados SQL do Azure em alguns passos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: d9c7c94e344daee5af87ce40ddf4dcb686696ded
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297338"
---
# <a name="tutorial--deploy-your-aspnet-app-and-azure-sql-database-with-the-azure-devops-project"></a>Tutorial: Implementar a Aplicação ASP.NET e a Base de Dados SQL do Azure com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada com a qual pode utilizar o seu código existente e o repositório de Git, ou escolher um dos exemplos de aplicação, para criar um pipeline de integração contínua (CI) e entrega contínua (CD) para o Azure.  O DevOps Project cria automaticamente recursos do Azure, como a Base de Dados SQL do Azure, um pipeline de versão no Azure DevOps, que inclui um pipeline de compilação para CI, configura um pipeline de versão para CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Irá:

> [!div class="checklist"]
> * Criar um Azure DevOps Project para uma Aplicação ASP.NET e Base de Dados SQL do Azure
> * Configurar os Serviços de DevOps do Azure e uma subscrição do Azure 
> * Examinar o pipeline de CI dos Serviços de DevOps do Azure
> * Examinar o pipeline de CD dos Serviços de DevOps do Azure
> * Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure
> * Ligar à Base de Dados do SQL Server do Azure 
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app-and-azure-sql-database"></a>Criar um Azure DevOps Project para uma Aplicação ASP.NET e Base de Dados SQL do Azure

O Azure DevOps Project cria um pipeline de CI/CD no Azure.  Pode criar uma organização **nova dos Serviços de DevOps do Azure** ou utilizar uma **organização existente**.  O Projeto de DevOps do Azure também cria **recursos do Azure**, como uma Base de Dados SQL do Azure na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Selecione o ícone **Criar um recurso** na barra de navegação esquerda e, em seguida, procure **DevOps Project**.  Escolha **Criar**.

    ![A Iniciar a Entrega Contínua](_img/azure-devops-project-github/fullbrowser.png)

1. Selecione **.NET** e escolha **Seguinte**.

1. Em **Escolha uma arquitetura de aplicação**, selecione **ASP.NET**.

1. Selecione **Adicionar uma base de dados** e, em seguida, escolha **Seguinte**.  

1. A arquitetura de aplicações que escolheu nos passos anteriores dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Selecione **Seguinte**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configurar os Serviços de DevOps do Azure e uma subscrição do Azure

1. Crie uma organização **nova** dos Serviços de DevOps do Azure ou utilize uma organização **existente**.  Escolha um **nome** para o seu Azure DevOps Project.  

1. Selecione a sua **subscrição do Azure**.

1. Opcionalmente, selecione a ligação **Alterar** para ver as definições de configuração do Azure adicionais e identifique o **nome de utilizador** na secção **Detalhes do Início de Sessão do Servidor da Base de Dados**.  **Guarde** o **nome de utilizador** para os passos seguintes deste tutorial.
 
1. Caso tenha realizado o passo acima, saia da área de configuração do Azure e escolha **Concluído**.  Caso contrário, basta selecionar **Concluído**.

1. Irá demorar alguns minutos até que o processo esteja concluído.  Depois de concluído, o **dashboard do Projeto** do Azure DevOps é carregado no portal do Azure.  Também pode navegar para o **Dashboard do Projeto de DevOps do Azure** diretamente a partir de **Todos os recursos** no **portal do Azure**.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Examinar o pipeline de CI dos Serviços de DevOps do Azure

O Azure DevOps Project configura automaticamente um pipeline de CI/CD completo do Azure na sua organização dos Serviços DevOps do Azure.  Pode explorar e personalizar o pipeline.  Siga os passos abaixo para familiarizar-se com o pipeline de compilação dos Serviços de DevOps do Azure.

1. Navegue para o **dashboard do Projeto de DevOps do Azure**.

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

O Azure DevOps Project cria e configura automaticamente os passos necessários para implementar da sua organização de Serviços de DevOps do Azure para a sua subscrição do Azure.  Estes passos incluem a configuração de uma ligação ao serviço do Azure para autenticar os Serviços de DevOps do Azure para a sua subscrição do Azure.  A automatização também cria uma Definição de Versão dos Serviços de DevOps do Azure e a versão disponibiliza a CD para o Azure.  Siga os passos abaixo para examinar mais sobre a Definição de Versão dos Serviços de DevOps do Azure.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O Azure DevOps Project criou um pipeline de versão dos Serviços de DevOps do Azure para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto ao pipeline de versão e, em seguida, selecione **Editar**.

1. O pipeline de versão contém um **pipeline**, que define o processo da versão.  Em **Artefactos**, selecione **Remover**.  O pipeline de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **ícone** **Acionador de implementação contínua** (que é apresentado como um relâmpago.)  Este pipeline de versão tem um acionador de CD ativado.  O acionador inicia uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. O Azure DevOps Project configura uma palavra-passe SQL aleatória e utiliza-a para o pipeline de versão.  No lado esquerdo do browser, selecione **Variáveis**. 

1. **Execute este passo apenas se tiver alterado a palavra-passe do SQL Server.**  Existe uma única variável de **Palavra-passe**.  À direita da caixa de texto **Valor**, selecione o ícone de **cadeado**.  **Introduza** a nova palavra-passe e selecione **Guardar**.

1. No lado esquerdo do browser, selecione **Tarefas** e escolha o seu **ambiente**.  

1. As tarefas são as atividades que o seu processo de implementação realiza e estão agrupadas em **Fases**.  Existe uma fase única para este pipeline de versão.  A fase contém uma tarefa de **implementação de Serviço de Aplicações do Azure** e de **Implementação de Base de Dados SQL do Azure**.

1. Selecione a tarefa **Executar SQL do Azure** e examine as diversas propriedades utilizadas para a implementação de SQL.  Sob o aviso **Pacote de Implementação**, observe que a tarefa utiliza um **ficheiro SQL DACPAC**.

1. No lado direito do navegador, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Existem vários menus para explorar a partir desta vista, como um **resumo da versão**, **itens relacionados com trabalho** e **Testes**.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. Pode comparar versões para ver as diferenças de consolidação entre implementações.

1. Selecionar **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure 

 > [!NOTE]
 > Os passos abaixo testam o pipeline de CI/CD com uma alteração de texto simples.  Opcionalmente, pode fazer uma alteração ao Esquema do SQL Server para a tabela testar o processo de implementação SQL.

Agora, está pronto para colaborar com uma equipa na sua aplicação, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.  Cada alteração ao repositório Git dos Serviços de DevOps do Azure inicia uma compilação nos Serviços de DevOps do Azure e um pipeline de CD dos Serviços de DevOps do Azure executa uma implementação no Azure.  Siga os passos abaixo ou utilize outras técnicas para consolidar as alterações ao seu repositório.  As alterações de código iniciam o processo de CI/CD e implementam automaticamente as novas alterações no Azure.

1. Selecione **Código** no menu dos Serviços de DevOps do Azure e navegue para o seu repositório.

1. Navegue para o diretório **SampleWebApplication\Vistas\Home**, selecione as **reticências** junto ao ficheiro **Index.cshtml** e, em seguida, escolha **Editar**.

1. Faça uma alteração no ficheiro, como algum texto dentro de uma das **etiquetas div**.  No canto superior direito, selecione **Consolidar**.  Selecione **Consolidar** novamente para emitir a alteração. 

1. Dentro de momentos, **é iniciada uma compilação nos Serviços de DevOps do Azure** e, em seguida, é executada uma versão para implementar as alterações.  Pode monitorizar o **estado da compilação** com o dashboard do DevOps Project ou no browser com a sua organização de Serviços de DevOps do Azure.

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
 > Se alterar a palavra-passe do início de sessão SQL, tem de alterar a palavra-passe da variável de pipeline de versão dos Serviços de DevOps do Azure, conforme descrito na secção **Examinar o pipeline de CD dos Serviços de DevOps do Azure**

## <a name="clean-up-resources"></a>Limpar recursos

 > [!NOTE]
 > Os passos abaixo irão eliminar permanentemente os recursos.  Utilize esta funcionalidade só depois de ler atentamente as instruções.

Se estiver a testar, pode limpar os recursos para evitar a acumulação de encargos de faturação.  Quando já não for preciso, pode eliminar a Base de Dados SQL do Azure e os recursos relacionados que criou neste tutorial, através da funcionalidade **Eliminar** do dashboard do Projeto de DevOps do Azure.  **Tenha cuidado**, uma vez que a funcionalidade de eliminação destrói os dados criados pelo Azure DevOps Project, tanto no Azure como nos Serviços de DevOps do Azure, e não conseguirá recuperá-los depois.

1. No **portal do Azure**, navegue para o **Projeto de DevOps do Azure**.
2. No **canto superior direito** do dashboard, selecione **Eliminar**.  Depois de ler a instrução, selecione **Sim** para **eliminar permanentemente** os recursos.

## <a name="next-steps"></a>Passos seguintes

Opcionalmente, pode modificar estes pipelines de compilação e versão para satisfazer as necessidades da sua equipa. Também pode utilizar este padrão de CI/CD como um modelo para outros projetos.  Aprendeu a:

> [!div class="checklist"]
> * Criar um Azure DevOps Project para uma Aplicação ASP.NET e Base de Dados SQL do Azure
> * Configurar os Serviços de DevOps do Azure e uma subscrição do Azure 
> * Examinar o pipeline de CI dos Serviços de DevOps do Azure
> * Examinar o pipeline de CD dos Serviços de DevOps do Azure
> * Consolidar as alterações aos Serviços de DevOps do Azure e implementar automaticamente no Azure
> * Ligar à Base de Dados do SQL Server do Azure 
> * Limpar recursos

Para saber mais sobre o pipeline do Azure, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Vídeos

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]