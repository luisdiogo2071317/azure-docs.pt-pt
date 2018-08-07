---
title: Introdução ao DSC de automatização do Azure
description: Explicação e exemplos, das tarefas mais comuns no Azure Automation Desired State Configuration (DSC)
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 08/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9f2312064e9fb7676d5609ee077d5ed7e02e8f30
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524469"
---
# <a name="getting-started-with-azure-automation-dsc"></a>Introdução ao DSC de automatização do Azure

Este artigo explica como fazer as tarefas mais comuns com o Azure Automation Desired State Configuration (DSC), como criar, importar e compilar configurações, integrar computadores para gerir e visualizar relatórios. Para uma visão geral dos quais DSC de automatização do Azure é, consulte [descrição geral do Azure Automation DSC](automation-dsc-overview.md). Para obter documentação de DSC, veja [Windows PowerShell Desired State Configuration descrição geral](/powershell/dsc/overview).

Este artigo fornece um guia passo a passo para usar o DSC de automatização do Azure. Se pretender que um ambiente de exemplo que já está configurado sem seguir os passos descritos neste artigo, pode utilizar o seguinte modelo do Resource Manager: este modelo configura um ambiente de DSC de automatização do Azure completo, incluindo uma VM do Azure que é gerido pelo Azure Automation DSC.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os exemplos neste artigo, é necessário o seguinte:

* Uma conta de Automatização do Azure. Para obter instruções sobre como criar uma conta Run As de Automatização do Azure, veja [Conta Run As do Azure](automation-sec-configure-azure-runas-account.md).
* Uma VM do Azure Resource Manager (não clássica) com o Windows Server 2008 R2 ou posterior. Para obter instruções sobre como criar uma VM, veja [Criar a sua primeira máquina virtual do Windows no portal do Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>Criação de uma configuração de DSC

Criar uma simples [configuração do DSC](/powershell/dsc/configurations) que garante a presença ou ausência da **servidor Web** Windows funcionalidade (IIS), dependendo da forma como atribui nós.

1. Inicie o Windows PowerShell ISE (ou qualquer editor de texto).
1. Escreva o seguinte texto:

    ```powershell
    configuration TestConfig
    {
        Node IsWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true

            }
        }

        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'

            }
        }
    }
    ```
1. Guarde o ficheiro como `TestConfig.ps1`.

Esta configuração chama um recurso no bloco de cada nó, o [recurso WindowsFeature](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), que garante a presença ou ausência da **servidor Web** funcionalidade.

## <a name="importing-a-configuration-into-azure-automation"></a>Importar uma configuração para a automatização do Azure

Em seguida, importar a configuração para a conta de automatização.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
1. Sobre o **conta de automatização** página, selecione **configurações de DSC** sob **o gerenciamento de configuração**.
1. Sobre o **configurações de DSC** página, clique em **+ adicionar uma configuração**.
1. Sobre o **importar configuração** página, navegue para o `TestConfig.ps1` ficheiro no seu computador.

    ![Captura de ecrã da * * importar configuração * * página](./media/automation-dsc-getting-started/AddConfig.png)
1. Clique em **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Visualizar uma configuração na automatização do Azure

Após importar uma configuração, pode vê-lo no portal do Azure.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
1. Sobre o **conta de automatização** página, selecione **configurações de DSC** sob **o gerenciamento de configuração**.
1. Sobre o **configurações de DSC** página, clique em **TestConfig** (este é o nome da configuração que importou no procedimento anterior).
1. Sobre o **TestConfig Configuration** página, clique em **Ver origem da configuração**.

    ![Captura de ecrã da página de configuração TestConfig](./media/automation-dsc-getting-started/ViewConfigSource.png)

    R **origem da configuração de TestConfig** é aberta, exibindo o código do PowerShell para a configuração a página.

## <a name="compiling-a-configuration-in-azure-automation"></a>Compilar uma configuração na automatização do Azure

Antes de poder aplicar um estado pretendido para um nó, uma configuração de DSC definir esse Estado deve ser compilada numa ou mais configurações de nó (documento MOF) e colocada no servidor de solicitação do DSC de automatização. Para obter uma descrição mais detalhada de compilar configurações no DSC de automatização do Azure, consulte [compilar configurações no DSC de automatização do Azure](automation-dsc-compile.md). Para obter mais informações sobre como compilar configurações, consulte [configurações de DSC](/powershell/dsc/configurations).

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
1. Na **conta de automatização** página, clique em **configurações de DSC** sob **o gerenciamento de configuração**.
1. Sobre o **configurações de DSC** página, clique em **TestConfig** (o nome da configuração importado anteriormente).
1. Sobre o **TestConfig Configuration** página, clique em **compilar**e, em seguida, clique em **Sim**. Esta ação inicia uma tarefa de compilação.

    ![Captura de ecrã da página de configuração TestConfig realçando o botão de compilação](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Quando compilar uma configuração na automatização do Azure, implementa automaticamente qualquer configuração de nó criado MOFs para o servidor de solicitação.

## <a name="viewing-a-compilation-job"></a>Ver uma tarefa de compilação

Depois de iniciar uma compilação, pode vê-la na **tarefas de compilação** mosaico no **configuração** página. O **tarefas de compilação** mosaico mostra atualmente em execução, foi concluída e tarefas falhadas. Quando abre uma página de tarefa de compilação, mostra informações sobre essa tarefa, incluindo quaisquer erros ou avisos encontrados, parâmetros de entrada utilizada na configuração e compilação registos.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
1. Na **conta de automatização** página, clique em **configurações de DSC** sob **o gerenciamento de configuração**.
1. Sobre o **configurações de DSC** página, clique em **TestConfig** (o nome da configuração importado anteriormente).
1. Sob **tarefas de compilação**, selecione a tarefa de compilação que pretende visualizar. R **tarefa de compilação** é aberta, o nome com a data em que a tarefa de compilação foi iniciada a página.

    ![Captura de ecrã da página de tarefa de compilação](./media/automation-dsc-getting-started/CompilationJob.png)
1. Clique em qualquer mosaico no **tarefa de compilação** página para ver mais detalhes sobre a tarefa.

## <a name="viewing-node-configurations"></a>Configurações de nó de visualização

A conclusão com êxito de uma tarefa de compilação cria uma ou mais configurações de nó novo. Uma configuração de nó é um documento MOF, que é implementado para o servidor de solicitação e pronta para ser extraídos e aplicada por um ou mais nós. Pode ver as configurações de nó na sua conta de automatização no **configurações de nó DSC** página. Uma configuração de nó tem um nome com o formulário *ConfigurationName*. *NodeName*.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No Hub menu, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
1. Sobre o **conta de automatização** página, clique em **State configuration (DSC)**, em seguida, selecione **configurações**.

    ![Captura de ecrã da página de configurações de nó DSC](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Inclusão de uma VM do Azure para gestão com o DSC de automatização do Azure

Pode utilizar o DSC de automatização do Azure para gerir VMs do Azure (clássicas e do Resource Manager), VMs no local, máquinas Linux, AWS VMs e máquinas físicas no local. Neste artigo, ficará a saber como carregar apenas do Azure Resource Manager VMs. Para obter informações sobre a integração de outros tipos de máquinas, consulte [integrar computadores para gestão pelo Azure Automation DSC](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>Para carregar uma VM do Azure Resource Manager para gestão pelo DSC de automatização do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
1. Na **conta de automatização** página, clique em **State configuration (DSC)** sob **o gerenciamento de configuração** e selecione **nós**.
1. Na **nós** página, clique em **+ adicionar**.

    ![Captura de ecrã da página de nós de DSC realçando o botão Adicionar VM do Azure](./media/automation-dsc-getting-started/OnboardVM.png)

1. Na página de máquinas virtuais, selecione a sua VM. **Adicionar VMs do Azure** página, clique em **selecionar máquinas virtuais para carregar**.
1. Clique em **Ligar**.

   > [!IMPORTANT]
   > Tem de ser uma VM do Azure Resource Manager com o Windows Server 2008 R2 ou posterior.

1. No **Registro** página, introduza o nome da configuração do nó que pretende aplicar à VM no **nome da configuração do nó** caixa. Isto deve corresponder exatamente o nome de uma configuração de nó na conta de automatização. Fornecer um nome a esta altura é opcional. Pode alterar a configuração de nó atribuído após a integração do nó.
   Verifique **reiniciar nó, se necessário**e, em seguida, clique em **OK**.

    ![Captura de ecrã da página de registo](./media/automation-dsc-getting-started/RegisterVM.png)

    A configuração do nó que especificou para a VM, são aplicadas em intervalos especificados pela **frequência de modo de configuração**, e a VM verifica a existência de atualizações para a configuração do nó em intervalos especificados pelo **atualizar Frequência**. Para obter mais informações sobre como estes valores são utilizados, consulte [configurar o Gestor de configuração Local](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
1. Na **adicionar VMs do Azure** página, clique em **criar**.

Azure inicia o processo de integração da VM. Quando estiver concluída, a VM aparece no **nós** guia a **State configuration (DSC)** página na conta de automatização.

## <a name="viewing-the-list-of-dsc-nodes"></a>Ver a lista de nós de DSC

Pode ver a lista de todos os computadores que foram carregadas para a gestão na sua conta de automatização no **nós** guia a **State configuration (DSC)** página.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
3. Na **conta de automatização** página, clique em **State configuration (DSC)**, em seguida, clique no **nós** separador.

## <a name="viewing-reports-for-dsc-nodes"></a>Visualização de relatórios para nós de DSC

Sempre que DSC de automatização do Azure realiza uma verificação de consistência num nó gerido, o nó envia um relatório de estado para o servidor de solicitação. Pode ver estes relatórios na página para esse nó.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
3. Sobre o **conta de automatização** página, clique em **State configuration (DSC)** e, em seguida, clique no **nós** separador.
4. Na **nós** , selecione o nó que pretende visualizar.
5. Sobre o **nó** página, clique no relatório que pretende visualizar sob **relatórios**.

    ![Captura de ecrã da página do relatório](./media/automation-dsc-getting-started/NodeReport.png)

Na página para um relatório individual, pode ver as seguintes informações de estado para a verificação de consistência correspondente:

* O estado do relatório — se o nó é "Compatível", a configuração de "Falhado", ou se o nó é "Em não conformidade" (quando o nó está no **applyandmonitor** modo e a máquina não está no estado pretendido).
* A hora de início para a verificação de consistência.
* O tempo de execução total para a verificação de consistência.
* O tipo de verificação de consistência.
* Quaisquer erros, incluindo o código de erro e a mensagem de erro.
* Quaisquer recursos de DSC utilizados na configuração e o estado de cada recurso (se o nó está no estado pretendido para esse recurso), pode clicar em cada recurso para obter informações mais detalhadas para esse recurso.
* O nome, o endereço IP e o modo de configuração do nó.

Também pode clicar **ver o relatório não processado** para ver os dados reais que o nó envia para o servidor. Para obter mais informações sobre como utilizar esses dados, consulte [usando um servidor de relatórios de DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).

Pode demorar algum tempo, após um nó de integração antes do primeiro relatório está disponível. Poderá ter de aguardar até 30 minutos para que o primeiro relatório depois de carregar um nó.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Reatribuição de um nó para uma configuração de nó diferente

Pode atribuir um nó para utilizar uma configuração de nó diferente daquele que inicialmente atribuída.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
3. Sobre o **conta de automatização** página, clique em **State configuration (DSC)** e, em seguida, clique no **nós** separador.
4. Sobre o **nós** separador, clique no nome do nó que pretende reatribuir.
5. Na página para esse nó, clique em **configuração do nó Assign**.

    ![Captura de ecrã da página de nó realçando o botão Atribuir nó](./media/automation-dsc-getting-started/AssignNode.png)
6. Sobre o **atribuir configuração do nó** , selecione a configuração do nó ao qual pretende atribuir o nó e, em seguida, clique em **OK**.

    ![Captura de ecrã da página atribuir configuração do nó](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Anular o registo de um nó

Se já não pretender que um nó para serem geridos pelo Azure Automation DSC, pode cancelar seu registro.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, clique em **todos os recursos** e, em seguida, o nome da sua conta de automatização.
3. Sobre o **conta de automatização** página, clique em **State configuration (DSC)** e, em seguida, clique no **nós** tab.git 
4. Sobre o **nós** separador, clique no nome do nó que pretende anular o registo.
5. Na página para esse nó, clique em **Unregister**.

    ![Captura de ecrã da página de nó, realce o botão de anular o registo](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Artigos relacionados

* [Descrição geral de DSC de automatização do Azure](automation-dsc-overview.md)
* [Integrar computadores para gestão pelo DSC de automatização do Azure](automation-dsc-onboarding.md)
* [Windows PowerShell Desired State Configuration descrição-geral](https://msdn.microsoft.com/powershell/dsc/overview)
* [Cmdlets de DSC de automatização do Azure](/powershell/module/azurerm.automation/#automation)
* [Preços de DSC de automatização do Azure](https://azure.microsoft.com/pricing/details/automation/)
