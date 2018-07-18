---
title: Como agendar o runtime de integração de SSIS do Azure | Documentos da Microsoft
description: Este artigo descreve como agendar a iniciar e parar de um runtime de integração de SSIS do Azure ao utilizar a automatização do Azure e o Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/16/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f83715d2a382db271686210d9df285c255c09216
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113999"
---
# <a name="how-to-start-and-stop-the-azure-ssis-integration-runtime-on-a-schedule"></a>Como iniciar e parar o runtime de integração de SSIS do Azure com base numa agenda
Este artigo descreve como agendar a iniciar e parar de um runtime de integração de SSIS do Azure (IR) ao utilizar a automatização do Azure e Azure Data Factory. Executar um runtime de integração do Azure SSIS (SQL Server Integration Services) (IR) tem um custo associado ele. Portanto, normalmente pretende executar a resposta a incidentes apenas quando precisar de executar pacotes do SSIS no Azure e parar o runtime de integração, quando não precisa dela. Pode utilizar a IU do Data Factory ou o Azure PowerShell para [manualmente iniciar ou parar um runtime de integração de SSIS do Azure](manage-azure-ssis-integration-runtime.md)).

Por exemplo, pode criar atividades Web com webhooks para um runbook do PowerShell da automatização do Azure e da cadeia de uma atividade de executar o pacote do SSIS entre eles. As atividades de Web podem iniciar e parar o runtime de integração Azure-SSIS just-in-time antes e após a execução de seu pacote. Para mais informações sobre a atividade de executar o pacote do SSIS, veja [executar um pacote do SSIS usando a atividade de SSIS no Azure Data Factory](how-to-invoke-ssis-package-ssis-activity.md).

## <a name="overview-of-the-steps"></a>Descrição geral dos passos

Aqui estão as etapas de alto nível descritas neste artigo:

1. **Criar e testar um runbook da automatização do Azure.** Neste passo, vai criar um runbook do PowerShell com o script que inicia ou para um ir de SSIS do Azure. Em seguida, testar o runbook em cenários de início e fim e confirme que o runtime de integração inicia ou para. 
2. **Crie duas agendas para o runbook.** Para a primeira agenda, configurar o runbook com o início como a operação. Para o agendamento da segunda, configure o runbook com STOP como a operação. Para ambas as agendas, especifique a cadência em que o runbook é executado. Por exemplo, pode querer agendar aquela primeira para ser executado às 8:00, todos os dias e o segundo para ser executado às 23:00 todos os dias. Quando o primeiro runbook é executado, ele começa a ir de SSIS do Azure. Quando executa o runbook segundo, para a ir de SSIS do Azure. 
3. **Criar dois webhooks para o runbook**, um para a operação de início e outro para a operação de paragem. Utilize os URLs destes webhooks quando configurar atividades web no pipeline do Data Factory. 
4. **Criar um pipeline do Data Factory**. O pipeline que criar consiste em três atividades. A primeira **Web** atividade invoca o webhook primeiro para iniciar a ir de SSIS do Azure. O **Stored Procedure** atividade executa um script SQL que executa o pacote do SSIS. A segunda **Web** atividade interrompe a ir de SSIS do Azure. Para obter mais informações sobre como invocar um pacote do SSIS partir de um pipeline do Data Factory através da atividade de procedimento armazenado, consulte [invocar um pacote do SSIS](how-to-invoke-ssis-package-stored-procedure-activity.md). Depois, vai criar um acionador de agenda para agendar o pipeline para executar a cadência que especificar.

## <a name="prerequisites"></a>Pré-requisitos
Se ainda não aprovisionou um runtime de integração de SSIS do Azure já, aprovisioná-lo ao seguir as instruções no [tutorial](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-test-an-azure-automation-runbook"></a>Criar e testar um runbook da automatização do Azure
Nesta secção, execute os seguintes passos: 

1. Crie uma conta de automatização do Azure.
2. Crie um runbook do PowerShell na conta de automatização do Azure. O script do PowerShell associado com o runbook inicia ou termina um runtime de integração de SSIS do Azure com base no comando que especificar para o parâmetro de operação. 
3. Testar o runbook em ambos os iniciar e parar cenários para confirmar que funciona. 

### <a name="create-an-azure-automation-account"></a>Crie uma conta de Automatização do Azure
Se não tiver uma conta de automatização do Azure, siga as instruções neste passo. Para obter passos detalhados, consulte [criar uma conta de automatização do Azure](../automation/automation-quickstart-create-account.md). Como parte deste passo, vai criar uma **Run As do Azure** conta (um principal de serviço no Azure Active Directory) e adicioná-lo para o **contribuinte** função da sua subscrição do Azure. Certifique-se de que é a mesma que a subscrição que contém a fábrica de dados que tenha a ir de SSIS do Azure. A automatização do Azure utiliza esta conta para autenticar para o Azure Resource Manager e operar nos seus recursos. 

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. Inicie sessão no [Portal do Azure](https://portal.azure.com/).    
3. Selecione **New** no menu da esquerda, selecione **monitorização + gestão**e selecione **automatização**. 

    ![Novo -> monitorização + gestão -> a automatização](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. Na **adicionar conta de automatização** janela, siga os passos seguintes: 

    1. Especifique um **nome** da conta de automatização. 
    2. Selecione o **subscrição** que tenha a fábrica de dados com ir de SSIS do Azure. 
    3. Para **grupo de recursos**, selecione **criar nova** para criar um novo grupo de recursos ou selecionar **utilizar existente** para selecionar um grupo de recursos existente. 
    4. Selecione um **localização** da conta de automatização. 
    5. Confirme que **criar conta Run as** está definida como **Sim**. É criado um principal de serviço no Azure Active Directory. Ela é adicionada para o **contribuinte** função da sua subscrição do Azure
    6. Selecione **afixar ao dashboard** para que o vê no dashboard do portal. 
    7. Selecione **Criar**. 

        ![Novo -> monitorização + gestão -> a automatização](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. Verá o **estado de implementação** no dashboard e nas notificações. 
    
    ![Implementar a automatização](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. Consulte a home page da conta de automatização depois de ser criado com êxito. 

    ![Página inicial de automatização](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Importar módulos do Data Factory

1. Selecione **módulos** no **recursos PARTILHADOS** secção no menu da esquerda e verifique se tem **azurerm. profile** e **AzureRM.DataFactoryV2** na lista de módulos.

    ![Verifique se os módulos necessários](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  Vá para a galeria do PowerShell para o [AzureRM.DataFactoryV2 módulo](https://www.powershellgallery.com/packages/AzureRM.DataFactoryV2/), selecione **implementar na automatização do Azure**, selecione a sua conta de automatização e, em seguida, selecione **OK**. Regresse para ver **módulos** no **recursos PARTILHADOS** secção no menu da esquerda e aguarde até ver a **estado** do **AzureRM.DataFactoryV2** módulo alteração **disponível**.

    ![Verifique se o módulo de fábrica de dados](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  Vá para a galeria do PowerShell para o [módulo do azurerm. profile](https://www.powershellgallery.com/packages/AzureRM.profile/), clique em **implementar na automatização do Azure**, selecione a sua conta de automatização e, em seguida, selecione **OK**. Regresse para ver **módulos** no **recursos PARTILHADOS** secção no menu da esquerda e aguarde até ver a **estado** do **azurerm. profile**módulo alteração **disponível**.

    ![Verifique se o módulo de perfil](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-a-powershell-runbook"></a>Criar runbook do PowerShell
O procedimento seguinte fornece passos para criar um runbook do PowerShell. O script associada ao runbook ou é iniciada/interrompida um runtime de integração de SSIS do Azure com base no comando que especificar para o **operação** parâmetro. Esta secção não fornece todos os detalhes para a criação de um runbook. Para obter mais informações, consulte [criar um runbook](../automation/automation-quickstart-create-runbook.md) artigo.

1. Mude para o **Runbooks** separador e selecione **+ adicionar um runbook** da barra de ferramentas. 

    ![Adicione um botão de runbook](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Selecione **criar um runbook novo**e execute os seguintes passos: 

    1. Para **Name**, tipo **StartStopAzureSsisRuntime**.
    2. Para **tipo de Runbook**, selecione **PowerShell**.
    3. Selecione **Criar**.
    
        ![Adicione um botão de runbook](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Copiar/colar o seguinte script para a janela de script do runbook. Guardar e, em seguida, publicar o runbook utilizando o **salvar** e **Publish** botões na barra de ferramentas. 

    ```powershell
    Param
    (
          [Parameter (Mandatory= $true)]
          [String] $ResourceGroupName,
    
          [Parameter (Mandatory= $true)]
          [String] $DataFactoryName,
    
          [Parameter (Mandatory= $true)]
          [String] $AzureSSISName,
    
          [Parameter (Mandatory= $true)]
          [String] $Operation
    )
    
    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
    
        "Logging in to Azure..."
        Connect-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
        if (!$servicePrincipalConnection)
        {
            $ErrorMessage = "Connection $connectionName not found."
            throw $ErrorMessage
        } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
        }
    }
    
    if($Operation -eq "START" -or $operation -eq "start")
    {
        "##### Starting #####"
        Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

    ![Editar runbook do PowerShell](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
5. Testar o runbook, selecionando **iniciar** botão na barra de ferramentas. 

    ![Botão de runbook de iniciar](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
6. Na **iniciar Runbook** janela, execute os seguintes passos: 

    1. Para **nome do grupo de recursos**, introduza o nome do grupo de recursos com a fábrica de dados que tenha a ir de SSIS do Azure. 
    2. Para **nome da fábrica de dados**, introduza o nome da fábrica de dados que tenha a ir de SSIS do Azure. 
    3. Para **AZURESSISNAME**, introduza o nome da ir de SSIS do Azure. 
    4. Para **operação**, introduza **iniciar**. 
    5. Selecione **OK**.  

        ![Iniciar a janela de runbook](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. Na janela de tarefas, selecione **saída** mosaico. Na janela de saída da tarefa, aguarde até ver a mensagem **# # # concluído # # #** depois de confirmar **# # # inicial # # #**. A partir de um runtime de integração de SSIS do Azure demora, aproximadamente 20 minutos. Fechar o **tarefa** janela e voltar para o **Runbook** janela.

    ![Azure SSIS IR - iniciada](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Repita os dois passos anteriores, mas através da utilização **parar** como o valor para o **operação**. Iniciar o runbook novamente ao selecionar o **iniciar** botão na barra de ferramentas. Especifique o nome do grupo de recursos, o nome da fábrica de dados e o nome do IR de SSIS do Azure. Para **operação**, introduza **parar**. 

    Na janela de saída da tarefa, aguarde até ver mensagem **# # # concluído # # #** depois de confirmar **# # # parar # # #**. A parar um runtime de integração de SSIS do Azure não demorar até a iniciar a ir de SSIS do Azure. Fechar o **tarefa** janela e voltar para o **Runbook** janela.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Criar agendas do runbook iniciar/parar o runtime de integração de SSIS do Azure
Na secção anterior, criou um runbook da automatização do Azure que pode iniciar ou parar um ir de SSIS do Azure. Nesta secção, vai criar duas agendas para o runbook. Ao configurar o primeiro agendamento, especifique o início para o parâmetro de operação. Da mesma forma, quando configurar a agenda segundo, especifique STOP para a operação. Para obter passos detalhados para a criação de agendas, consulte [criar uma agenda](../automation/automation-schedules.md#creating-a-schedule).

1. Na **Runbook** janela, selecione **agendas**e selecione **+ adicionar uma agenda** na barra de ferramentas. 

    ![Azure SSIS IR - iniciada](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. Na **agenda Runbook** janela, execute os seguintes passos: 

    1. Selecione **ligar uma agenda ao runbook**. 
    2. Selecione **criar uma nova agenda**.
    3. Na **nova agenda** janela, escreva **iniciar IR diariamente** para **nome**. 
    4. Na **inicia a seção**, para o tempo, especificar uma hora de alguns minutos depois da hora atual. 
    5. Para **periodicidade**, selecione **periódico**. 
    6. Na **Repetir cada** secção, selecione **dia**. 
    7. Selecione **Criar**. 

        ![Agenda de início de IR de SSIS do Azure](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Mude para o **parâmetros e definições de execução** separador. Especifique o nome do grupo de recursos, o nome da fábrica de dados e o nome do IR de SSIS do Azure. Para **operação**, introduza **iniciar**. Selecione **OK**. Selecione **OK** novamente para ver a agenda sobre o **agendas** página do runbook. 

    ![Agenda de olhar o runtime de integração de SSIS do Azure](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Repita os dois passos anteriores para criar uma agenda com o nome **IR parar diariamente**. Desta vez, especifique o tempo, pelo menos, 30 minutos após a hora que especificou para o **iniciar o runtime de integração diariamente** agenda. Para **operação**, especifique **parar**. 
5. Na **Runbook** janela, selecione **tarefas** no menu da esquerda. Deverá ver as tarefas criadas pelas agendas especificado em horas e os respetivos Estados. Pode ver detalhes sobre a tarefa, como o respetivo resultado semelhante ao que tem visto quando testou o runbook. 

    ![Agenda de olhar o runtime de integração de SSIS do Azure](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. Depois de terminar de teste, desative as agendas a edição dos mesmos e selecionando **não** para **ativado**. Selecione **agendas** no menu da esquerda, selecione a **iniciar o runtime de integração diariamente/parar IR diariamente**e selecione **não** para **ativado**. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Criar webhooks para iniciar e parar o runtime de integração de SSIS do Azure
Siga as instruções em [criar um webhook](../automation/automation-webhooks.md#creating-a-webhook) para criar dois webhooks para o runbook. Para o primeiro, especificar o início como a operação de e para o segundo cenário, especifique STOP como a operação. Guarde os URLs para ambos os webhooks em algum lugar (como um arquivo de texto ou um bloco de notas do OneNote). Utilizar estes URLs quando configurar atividades Web no pipeline do Data Factory. A imagem seguinte mostrada um exemplo de criação de um webhook que inicia o runtime de integração de SSIS do Azure:

1. Na **Runbook** janela, selecione **Webhooks** no menu à esquerda e selecione **+ adicionar Webhook** na barra de ferramentas. 

    ![Webhooks -> Adicionar Webhook](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. Na **adicionar Webhook** janela, selecione **criar novo webhook**, e fazer as seguintes ações: 

    1. Para **Name**, introduza **StartAzureSsisIR**. 
    2. Confirme que **Enabled** está definida como **Sim**. 
    3. Copiar o **URL** e guardá-lo em algum lugar. Este passo é importante. Não vir o URL mais tarde. 
    4. Selecione **OK**. 

        ![Nova janela de Webhook](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Mude para o **parâmetros e definições de execução** separador. Especifique o nome do grupo de recursos, o nome da fábrica de dados e o nome do IR de SSIS do Azure. Para **operação**, introduza **iniciar**. Clique em **OK**. Em seguida, clique em **Criar**. 

    ![Webhook - parâmetros e definições de execução](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Repita os três passos anteriores para criar outro webhook com o nome **StopAzureSsisIR**. Não se esqueça de copiar o URL. Ao especificar os parâmetros e definições de execução, introduza **parar** para **operação**. 

Deve ter dois URLs, um para o **StartAzureSsisIR** webhook e outro para o **StopAzureSsisIR** webhook. Pode enviar um pedido HTTP POST para estas URLs para iniciar/parar a ir de SSIS do Azure. 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Criar e agendar um pipeline de fábrica de dados que é iniciada/interrompida a resposta a incidentes
Esta secção mostra como utilizar uma atividade Web para invocar os webhooks que criou na secção anterior.

O pipeline que criar consiste em três atividades. 

1. A primeira **Web** atividade invoca o webhook primeiro para iniciar a ir de SSIS do Azure. 
2. O **executar o pacote do SSIS** atividade ou o **Stored Procedure** execuções de atividades do pacote do SSIS.
3. A segunda **Web** atividade invoca o webhook para parar a ir de SSIS do Azure. 

Depois de criar e testar o pipeline, pode criar um acionador de agenda e associa o pipeline. O acionador de agenda define uma agenda para o pipeline. Suponha, vai criar um acionador que é agendado para ser executada diariamente às 23:00. O acionador executa o pipeline às 23:00 todos os dias. O pipeline é iniciado o runtime de integração de SSIS do Azure, é executado o pacote do SSIS e, em seguida, pára a ir de SSIS do Azure. 

### <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).    
2. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Na página **Nova fábrica de dados**, introduza **MyAzureSsisDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se receber o seguinte erro, altere o nome da fábrica de dados (por exemplo, oseunomeMyAzureSsisDataFactory) e tente criá-la novamente. Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
       `Data factory name �MyAzureSsisDataFactory� is not available`
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
      Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Só são mostradas na lista as localizações que são suportadas para a criação de fábricas de dados.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Clique em **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Data Factory num separador à parte.

### <a name="create-a-pipeline"></a>Criar um pipeline

1. Na **começar** página, selecione **criar pipeline**. 

   ![Página Introdução](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. Na **atividades** caixa de ferramentas, expanda **gerais**, arraste e largue o **Web** atividade para a superfície de desenho do pipeline. Na **gerais** separador da **propriedades** janela, altere o nome da atividade como **StartIR**.

   ![Primeira atividade Web - separador Geral](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Mude para o **definições** separador a **propriedades** janela, e fazer as seguintes ações: 

    1. Para **URL**, cole o URL do webhook que começa a ir de SSIS do Azure. 
    2. Para **método**, selecione **POST**. 
    3. Para **corpo**, introduza `{"message":"hello world"}`. 
   
        ![Primeira atividade Web - separador de definições](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)

4. Arraste e largue a atividade de executar o pacote do SSIS ou a atividade de procedimento armazenado do **gerais** secção a **atividades** caixa de ferramentas. Defina o nome da atividade como **RunSSISPackage**. 

5. Se selecionar a atividade de executar o pacote do SSIS, siga as instruções em [executar um pacote do SSIS usando a atividade SSIS no Azure Data Factory](how-to-invoke-ssis-package-ssis-activity.md) para concluir a criação de atividade.  Certifique-se de que especifica um número suficiente de tentativas de repetição são suficientemente frequente para aguardar a disponibilidade do Runtime de integração Azure-SSIS, uma vez que demora até 30 minutos para começar. 

    ![Definições de repetição](media/how-to-schedule-azure-ssis-integration-runtime/retry-settings.png)

6. Se selecionar a atividade de procedimento armazenado, siga as instruções em [invocar um pacote do SSIS com a atividade de procedimento armazenado no Azure Data Factory](how-to-invoke-ssis-package-stored-procedure-activity.md) para concluir a criação de atividade. Certifique-se de que inserir um script de Transact-SQL que aguarda que a disponibilidade do Runtime de integração Azure-SSIS, uma vez que demora até 30 minutos para começar.
    ```sql
    DECLARE @return_value int, @exe_id bigint, @err_msg nvarchar(150)

    -- Wait until Azure-SSIS IR is started
    WHILE NOT EXISTS (SELECT * FROM [SSISDB].[catalog].[worker_agents] WHERE IsEnabled = 1 AND LastOnlineTime > DATEADD(MINUTE, -10, SYSDATETIMEOFFSET()))
    BEGIN
        WAITFOR DELAY '00:00:01';
    END

    EXEC @return_value = [SSISDB].[catalog].[create_execution] @folder_name=N'YourFolder',
        @project_name=N'YourProject', @package_name=N'YourPackage',
        @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [SSISDB].[catalog].[start_execution] @execution_id = @exe_id, @retry_count = 0

    -- Raise an error for unsuccessful package execution, check package execution status = created (1)/running (2)/canceled (3)/
    -- failed (4)/pending (5)/ended unexpectedly (6)/succeeded (7)/stopping (8)/completed (9) 
    IF (SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id = @exe_id) <> 7 
    BEGIN
        SET @err_msg=N'Your package execution did not succeed for execution ID: '+ CAST(@execution_id as nvarchar(20))
        RAISERROR(@err_msg, 15, 1)
    END
    ```

7. Ligar o **Web** atividade para o **Execute o SSIS Package** ou o **Stored Procedure** atividade. 

    ![Ligar as atividades Web e o procedimento armazenado](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-sproc.png)

8. Arraste e largue outra **Web** atividade à direita do **Execute o SSIS Package** atividade ou o **Stored Procedure** atividade. Defina o nome da atividade como **StopIR**. 
9. Mude para o **definições** separador a **propriedades** janela, e fazer as seguintes ações: 

    1. Para **URL**, cole o URL do webhook que pára a ir de SSIS do Azure. 
    2. Para **método**, selecione **POST**. 
    3. Para **corpo**, introduza `{"message":"hello world"}`.  
10. Ligar o **executar o pacote do SSIS** atividade ou o **Stored Procedure** atividade para a última **Web** atividade.

    ![Pipeline completo](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
11. Validar as definições do pipeline ao clicar em **Validate** na barra de ferramentas. Fechar o **relatório de validação de Pipeline** clicando **>>** botão. 

    ![Validar o pipeline](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>Testar a execução do pipeline

1. Selecione **execução de teste** na barra de ferramentas para o pipeline. Consulte a saída no **saída** janela no painel inferior. 

    ![Execução de teste](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. Na **Runbook** página da sua conta de automatização do Azure, pode verificar se as tarefas executaram para iniciar e parar a ir de SSIS do Azure. 

    ![Tarefas de Runbook](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. Inicie o SQL Server Management Studio. Na **ligar ao servidor** janela, fazer as seguintes ações: 

    1. Para **nome do servidor**, especifique  **&lt;sua base de dados SQL do Azure&gt;. database.windows.net**.
    2. Selecione **Opções >>**.
    3. Para **ligar à base de dados**, selecione **SSISDB**.
    4. Selecione **Ligar**. 
    5. Expanda **catálogos de serviços de integração** -> **SSISDB** -> sua pasta -> **projetos** -> projeto do Your SSIS -> **pacotes** . 
    6. O pacote do SSIS com o botão direito e selecione **relatórios** -> **relatórios padrão** -> **todas as execuções**. 
    7. Certifique-se de que o pacote do SSIS foi executada. 

        ![Certifique-se a execução de pacotes do SSIS](./media/how-to-schedule-azure-ssis-integration-runtime/verfiy-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>Agendar o pipeline 
Agora que o pipeline funciona conforme esperado, pode criar um acionador para ser executado este pipeline numa cadência especificada. Para obter detalhes sobre a associação de um acionador de agenda com um pipeline, veja [acionar o pipeline com base numa agenda](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. Na barra de ferramentas do pipeline, selecione **acionador**e selecione **novo/editar**. 

    ![Acionador -> Novo/Editar](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. Na **adicionar Acionadores** janela, selecione **+ novo**.

    ![Adicionar Acionadores - novo](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. Na **novo acionador**, fazer as seguintes ações: 

    1. Para **nome**, especifique um nome para o acionador. No exemplo a seguir **ser executada diariamente,** é o nome do acionador. 
    2. Para **tipo**, selecione **agenda**. 
    3. Para **data de início**, selecione uma data de início e hora. 
    4. Para **periodicidade**, especificar a cadência para o acionador. No exemplo a seguir, única diária. 
    5. Para **final**, pode especificar a data e hora ao selecionar o **na data** opção. 
    6. Selecione **ativado**. O acionador é ativado imediatamente depois de publicar a solução no Data Factory. 
    7. Selecione **Seguinte**.

        ![Acionador -> Novo/Editar](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. Na **parâmetros da execução do acionador** página, leia o aviso e selecione **concluir**. 
5. Publicar a solução no Data Factory, selecionando **publicar tudo** no painel esquerdo. 

    ![Publicar Tudo](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)

### <a name="monitor-the-pipeline-and-trigger-in-the-azure-portal"></a>Monitorizar o pipeline e o acionador no portal do Azure

1. Para monitorizar as execuções de Acionador e execuções de pipeline, utilize o **Monitor** separador no lado esquerdo. Para obter passos detalhados, consulte [monitorizar o pipeline](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![Execuções de pipeline](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
2. Para ver as execuções de atividades associadas com uma execução de pipeline, selecione a primeira ligação (**ver execuções de atividades**) na **ações** coluna. Ver as execuções de três atividade associadas com cada atividade no pipeline (primeiro Web atividade, atividade de procedimento armazenado e a segunda atividade Web). Para mudar novamente para ver as execuções de pipeline, selecione **Pipelines** link na parte superior.

    ![Execuções de atividade](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
3. Também pode ver as execuções de Acionador, selecionando **acionar execuções** na lista pendente junto à **execuções do Pipeline** na parte superior. 

    ![Execuções de acionador](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

### <a name="monitor-the-pipeline-and-trigger-with-powershell"></a>Monitorizar o pipeline e o acionador com o PowerShell

Utilize scripts como nos exemplos seguintes para monitorizar o pipeline e o acionador.

1. Obtenha o estado de uma execução de pipeline.

  ```powershell
  Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $myPipelineRun
  ```

2. Obter informações sobre o acionador.

  ```powershell
  Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name  "myTrigger"
  ```

3. Obtenha o estado de uma execução de Acionador.

  ```powershell
  Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "myTrigger" -TriggerRunStartedAfter "2018-07-15" -TriggerRunStartedBefore "2018-07-16"
  ```

## <a name="next-steps"></a>Passos Seguintes
Consulte a seguinte mensagem de blogue:
-   [Modernize e expandir os fluxos de trabalho ETL/ELT com atividades SSIS no ADF pipelines](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

Veja os artigos seguintes da documentação do SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Implementar, executar e monitorizar pacotes do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Connect to SSIS catalog on Azure (Ligar ao catálogo do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Agendar a execução de pacotes no Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Ligar a origens de dados no local com a autenticação do Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)
