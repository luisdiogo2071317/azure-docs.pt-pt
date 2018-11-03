---
title: Publicação de um serviço de nuvem usando as ferramentas do Azure | Documentos da Microsoft
description: Saiba mais sobre como publicar projetos de serviço cloud do Azure com o Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: a60f00e1df994988bfeae112db10bffa43f81642
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969497"
---
# <a name="publishing-a-cloud-service-using-visual-studio"></a>Publicação de um serviço cloud com o Visual Studio

Visual Studio pode publicar uma aplicação diretamente para o Azure, com suporte para ambientes de teste e produção de um serviço cloud. Ao publicar, selecione o ambiente de implantação e uma conta de armazenamento que é utilizada temporariamente para o pacote de implementação.

Quando programar e testar uma aplicação do Azure, pode utilizar o Web Deploy para publicar as alterações de forma incremental para suas funções da web. Depois de publicar seu aplicativo para um ambiente de implantação, Web Deploy permite-lhe implementar alterações diretamente à máquina virtual que está a executar a função da web. Não é necessário que empacotar e publicar a aplicação do Azure inteira sempre que pretende atualizar a função da web para testar as alterações. Com esta abordagem, pode ter as alterações de função da web disponíveis na cloud para fins de teste sem ter de esperar para que seu aplicativo publicado para um ambiente de implantação.

Para publicar a aplicação do Azure e atualizar uma função da web usando o Web Deploy, utilize os seguintes procedimentos:

- Publicar ou pacote de aplicação do Azure a partir do Visual Studio
- Atualizar uma função da web como parte do ciclo de testes e desenvolvimento

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publicar ou pacote de aplicação do Azure a partir do Visual Studio

Quando publica a aplicação do Azure, pode efetuar uma das seguintes tarefas:

- Criar um pacote de serviço: pode utilizar este pacote e o ficheiro de configuração de serviço para publicar a sua aplicação para um ambiente de implantação a partir da [portal do Azure](https://portal.azure.com).

- Publicar o seu projeto do Azure a partir do Visual Studio: para publicar a sua aplicação diretamente para o Azure, utilize o Assistente de publicação. Para obter informações, consulte [publicar Assistente da aplicação Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Para criar um pacote de serviço a partir do Visual Studio

1. Quando estiver pronto para publicar a sua aplicação, abra o Explorador de soluções, abra o menu de atalho para o projeto do Azure com suas funções e clicar em publicar.

1. Para criar um pacote de serviço apenas, siga estes passos:

   a. No menu de atalho para o projeto do Azure, escolha **pacote**.

   b. Na **empacotar a aplicação Azure** caixa de diálogo, escolha a configuração do serviço para o qual pretende criar um pacote e, em seguida, escolha a configuração de compilação.

   c. (Opcional) Para ativar o ambiente de trabalho remoto para o serviço em nuvem depois de publicar, selecione **ativar o ambiente de trabalho remoto para todas as funções**e, em seguida, selecione **definições** para configurar as credenciais de ambiente de trabalho remoto. Para obter mais informações, consulte [ativar a ligação de ambiente de trabalho remoto para uma função nos serviços Cloud do Azure com o Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

      Se quiser depurar seu serviço cloud, depois de publicar, ativar a depuração remota, selecionando **ativar o depurador remoto para todas as funções**.

   d. Para criar o pacote, escolha o **pacote** ligação.

      Explorador de ficheiros mostra a localização do ficheiro do pacote criado recentemente. Pode copiar esta localização para que pode usá-lo do portal do Azure.

   e. Para publicar este pacote para um ambiente de implantação, tem de utilizar esta localização como a localização do pacote quando criar um serviço cloud e implementar este pacote num ambiente com o portal do Azure.

1. (Opcional) Para cancelar o processo de implantação, no menu de atalho para o item de linha no registo de atividades, escolha **Cancelar e remover**. Este comando interrompe o processo de implantação e elimina o ambiente de implantação do Azure. Para remover o ambiente após a implementação, utilize o portal do Azure.

1. (Opcional) Depois de iniciaram as instâncias de função, o Visual Studio mostra automaticamente o ambiente de implantação no **serviços Cloud** nó no Explorador de servidores. A partir daqui, pode ver o estado das instâncias de função individuais. Ver [recursos de gerir o Azure com o Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md). A ilustração seguinte mostra as instâncias de função enquanto estão ainda no estado de inicialização:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Atualizar uma função da web como parte do ciclo de testes e desenvolvimento

Se a infraestrutura de back-end da sua aplicação esteja estável, mas as funções da web precisam de mais de atualização frequente, pode utilizar o Web Deploy para atualizar apenas uma função da web no seu projeto. Web Deploy é útil quando não deseja reconstruir e voltar a implementar as funções de trabalho back-end, ou se tiver várias funções da web e que pretende atualizar apenas uma das funções do web.

### <a name="requirements-for-using-web-deploy"></a>Requisitos para utilizar o Web Deploy

- **Para desenvolvimento e teste apenas a fins**: das alterações serem efetuadas diretamente para a máquina virtual em que está a executar a função da web. Se esta máquina virtual tem de ser reciclados, as alterações são perdidas porque o pacote original que publicou é utilizado para recriar a máquina virtual para a função. Voltar a publicar seu aplicativo para obter as alterações mais recentes para a função da web.

- **Funções da web só podem ser atualizadas**: não não possível atualizar funções de trabalho. Além disso, não é possível atualizar o `RoleEntryPoint` em `web role.cs`.

- **Só pode suportar uma única instância de uma função da web**: não pode ter várias instâncias de qualquer função da web no seu ambiente de implantação. No entanto, são suportadas várias funções da web cada com apenas uma instância.

- **Ativar as ligações de ambiente de trabalho remotas**: Este requisito permite a implementação Web para utilizar o utilizador e palavra-passe para ligar à máquina virtual para implementar as alterações no servidor que está a executar serviços de informação Internet (IIS). Além disso, poderá ter de ligar à máquina virtual para adicionar um certificado fidedigno para o IIS nesta máquina virtual. (Este certificado garante que a ligação remota para o IIS, que é utilizado pelo Web Deploy é segura).

O procedimento seguinte assume que está a utilizar o **publicar a aplicação do Azure** assistente.

### <a name="enable-web-deploy-when-you-publish-your-application"></a>Ativar o Web Deploy quando publicar a sua aplicação

1. Para ativar a **ativar o Web Deploy para todas as funções de web** opção, primeiro tem de configurar ligações de ambiente de trabalho remotas. Selecione **ativar o ambiente de trabalho remoto** para todas as funções e, em seguida, forneça as credenciais que é utilizada para ligar remotamente no **configuração de ambiente de trabalho remoto** caixa que aparece. Ver [ativar a ligação de ambiente de trabalho remoto para uma função nos serviços Cloud do Azure com o Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

1. Para ativar o Web Deploy para todas as funções da web na sua aplicação, selecione **ativar o Web Deploy para todas as funções de web**.

    Um triângulo amarelo de aviso é apresentado. Web Deploy utiliza um certificado autoassinado e não fidedigno por predefinição, o que não é recomendada para carregar os dados confidenciais. Se precisar de proteger este processo para dados confidenciais, pode adicionar um certificado SSL para ser utilizado nas ligações Web Deploy. Este certificado tem de ser um certificado fidedigno. Para obter mais informações, consulte [tornar web implementar seguro](#make-web-deploy-secure).

1. Escolher **próxima** para mostrar o **resumo** ecrã e, em seguida, escolha **publicar** para implementar o serviço em nuvem.

    O serviço em nuvem é publicado. A máquina virtual que é criada tem ligações remotas ativadas para o IIS para que o Web Deploy podem ser utilizado para atualizar suas funções da web sem republicá-los.

   > [!NOTE]
   > Se tiver mais do que uma instância configurada para uma função da web, é apresentada uma mensagem de aviso informando que cada função da web está limitada a uma instância apenas no pacote do que é criado para publicar a sua aplicação. Selecione **OK** para continuar. Conforme indicado na secção de requisitos, pode ter mais do que uma função da web, mas apenas uma instância de cada função.

### <a name="update-your-web-role-by-using-web-deploy"></a>Atualizar a função da web com o Web Deploy

1. Para utilizar o Web Deploy, fazer alterações de código ao projeto para qualquer uma das suas funções da web no Visual Studio que deseja publicar e, em seguida, clique com o botão direito este nó do projeto na sua solução e aponte para **publicar**. O **publicar Web** é apresentada a caixa de diálogo.

1. (Opcional) Se tiver adicionado um certificado SSL fidedigno para utilizar para ligações remotas para o IIS, pode desmarcar a **certificado não confiável de permitir** caixa de verificação. Para obter informações sobre como adicionar um certificado para tornar o Web Deploy seguro, consulte a secção **para tornar Web implementar seguro** mais adiante neste artigo.

1. Para utilizar o Web Deploy, o mecanismo de publicação tem do nome de utilizador e palavra-passe que configurou para a sua ligação de ambiente de trabalho remoto quando o pacote é publicada pela primeira vez.

   a. Na **nome de utilizador**, introduza o nome de utilizador.

   b. Na **palavra-passe**, introduza a palavra-passe.

   c. (Opcional) Se pretender guardar esta palavra-passe neste perfil, escolha **guardar palavra-passe**.

1. Para publicar as alterações para a função da web, escolha **publicar**.

    A linha de estado apresenta **publicar iniciado**. Quando a publicação estiver concluída, **publicação foi efetuada com êxito** aparece. Agora que foram implementadas as alterações para a função da web na sua máquina virtual. Agora pode iniciar a aplicação do Azure no ambiente do Azure para testar as suas alterações.

### <a name="make-web-deploy-secure"></a>Tornar as web implementar seguro

1. Web Deploy utiliza um certificado autoassinado e não fidedigno por predefinição, o que não é recomendada para carregar os dados confidenciais. Se precisar de proteger este processo para dados confidenciais, pode adicionar um certificado SSL para ser utilizado nas ligações Web Deploy. Este certificado tem de ser um certificado fidedigno, o que é obter a partir de uma autoridade de certificação (AC).

    Para tornar o Web Deploy seguro para cada máquina virtual para cada uma das suas funções da web, tem de carregar o certificado fidedigno que pretende utilizar para a web implementar no portal do Azure. Este certificado torna-se de que o certificado é adicionado à máquina virtual que foi criada para a função da web, quando publicar a sua aplicação.

1. Para adicionar um certificado SSL fidedigno para o IIS para utilizar para ligações remotas, siga estes passos:

   a. Para ligar à máquina virtual que está a executar a função da web, selecione a instância da função da web em **Cloud Explorer** ou **Explorador de servidores**e, em seguida, escolha o **ligar através do ambiente de trabalho remoto**  comando. Para obter passos detalhados sobre como ligar à máquina virtual, consulte [ativar a ligação de ambiente de trabalho remoto para uma função nos serviços Cloud do Azure com o Visual Studio](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md). O browser pede-lhe para transferir um `.rdp` ficheiro.

   b. Para adicionar um certificado SSL, abra o serviço de gestão no Gerenciador do IIS. No Gestor de IIS, ativar o SSL, abra a **enlaces** ligação na **ação** painel. O **Adicionar enlace de Site** é apresentada a caixa de diálogo. Escolher **Add**e, em seguida, escolha o HTTPS no **tipo** na lista pendente. Na **certificado SSL** lista, escolha o certificado SSL que tivesse assinado por uma AC e que carregou para o portal do Azure. Para obter mais informações, consulte [configurar definições de ligação para o serviço de gestão](http://go.microsoft.com/fwlink/?LinkId=215824).

      > [!NOTE]
      > Se adicionar um certificado SSL fidedigno, o triângulo de aviso amarelo deixa de aparecer no **Assistente de publicação**.

## <a name="include-files-in-the-service-package"></a>Incluir ficheiros no pacote de serviço

Poderá ter de incluir ficheiros específicos em seu pacote de serviço para que fiquem disponíveis na máquina virtual que é criada para uma função. Por exemplo, pode querer adicionar uma `.exe` ou um `.msi` ficheiro que é utilizado por um script de inicialização ao seu pacote de serviço. Ou poderá ter de adicionar um assembly que necessita de um projeto de função de worker ou da função da web. Incluir ficheiros, tem de ser adicionados à solução para a sua aplicação do Azure.

1. Para adicionar um assembly para um pacote de serviço, utilize os seguintes passos:

   a. Na **Explorador de soluções**, abra o nó do projeto para o projeto que está em falta a assemblagem referenciada.
   b. Para adicionar o assembly ao projeto, abra o menu de atalho para o **referências** pasta e, em seguida, escolha **Add Reference**. É apresentada a caixa de diálogo Adicionar referência.
   c. Escolha a referência de que pretende adicionar e, em seguida, escolha **OK**. A referência é adicionada à lista sob a **referências** pasta.
   d. Abrir o menu de atalho para a assemblagem que adicionou e escolha **propriedades**. O **propriedades** é apresentada a janela.

      Para incluir esse assembly no pacote de serviço, na **lista Local de cópia** escolha **verdadeiro**.
1. Na **Explorador de soluções** abrir no nó do projeto para o projeto que está em falta a assemblagem referenciada.

1. Para adicionar o assembly ao projeto, abra o menu de atalho para o **referências** pasta e, em seguida, escolha **Add Reference**. O **Add Reference** é apresentada a caixa de diálogo.

1. Escolha a referência de que pretende adicionar e, em seguida, escolha o **OK** botão.

    A referência é adicionada à lista sob a **referências** pasta.

1. Abrir o menu de atalho para a assemblagem que adicionou e escolha **propriedades**. É apresentada a janela de propriedades.

1. Para incluir esse assembly no pacote de serviço, o **Copy Local** lista, escolha **verdadeiro**.

1. Para incluir os ficheiros do pacote de serviço que foram adicionados ao seu projeto de função da web, abrir o menu de atalho para o ficheiro e, em seguida, escolha **propriedades**. Do **propriedades** janela, escolha **conteúdo** partir o **Build Action** caixa de listagem.

1. Para incluir os ficheiros do pacote de serviço que foram adicionados ao seu projeto de função de trabalho, abra o menu de atalho para o ficheiro e, em seguida, escolha **propriedades**. Do **propriedades** janela, escolha **copiar se for mais recente** partir o **copiar para diretório de saída** caixa de listagem.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a publicação para o Azure a partir do Visual Studio, veja [publicar Assistente da aplicação Azure](vs-azure-tools-publish-azure-application-wizard.md).
