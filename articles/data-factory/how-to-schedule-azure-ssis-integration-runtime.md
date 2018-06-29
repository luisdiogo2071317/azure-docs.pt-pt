---
title: Como agendar o tempo de execução de integração do Azure SSIS | Microsoft Docs
description: Este artigo descreve como agendar a iniciar e parar um tempo de execução de integração do Azure SSIS através da automatização do Azure e do Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 06/01/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 3758b04fc9b5ecd5dc69c82a8bd07999a9f1074a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050612"
---
# <a name="how-to-start-and-stop-the-azure-ssis-integration-runtime-on-a-schedule"></a>Como iniciar e parar o tempo de execução de integração de SSIS do Azure com base numa agenda
Este artigo descreve como agendar a iniciar e parar um tempo de execução de integração do Azure SSIS (IR) através da utilização da automatização do Azure e do Azure Data Factory. Com um tempo de execução de integração do Azure SSIS (SQL Server Integration Services) (IR) tem um custo associado ao mesmo. Por conseguinte, normalmente, pretende executar a resposta a incidentes apenas quando precisar de executar os pacotes SSIS no Azure e parar a resposta a incidentes quando não precisar dele. Pode utilizar a IU da fábrica de dados ou o Azure PowerShell para [manualmente iniciar ou parar uma resposta a incidentes SSIS Azure](manage-azure-ssis-integration-runtime.md)).

Por exemplo, pode criar atividades Web com webhooks para um runbook do PowerShell de automatização do Azure e encadeiam uma atividade de executar o pacote de SSIS entre eles. As atividades de Web podem iniciar e parar a resposta a incidentes Azure SSIS apenas na hora antes e após a execução do seu pacote. Para obter mais informações sobre a atividade de executar o pacote de SSIS, consulte [executar um pacote SSIS utilizando a atividade de SSIS no Azure Data Factory](how-to-invoke-ssis-package-ssis-activity.md).

## <a name="overview-of-the-steps"></a>Descrição geral dos passos

Eis os passos de alto nível descritos neste artigo:

1. **Criar e testar um runbook de automatização do Azure.** Neste passo, cria um runbook do PowerShell com o script que inicia ou para um IR. SSIS do Azure Em seguida, testar o runbook em cenários de início e paragem e confirme que a resposta a incidentes inicia ou para. 
2. **Crie duas agendas para o runbook.** Para a agenda primeiro, configure o runbook com início como a operação. Para a agenda segundo, configure o runbook com paragem de como a operação. Para ambas as agendas, especificar a cadência em que o runbook é executado. Por exemplo, poderá pretender agendar a uma primeira para ser executada às 8 AM todos os dias e um segundo para ser executada em 23: 00 uso corrente. Quando executa o runbook primeiro, começa a IR. de SSIS do Azure Quando o runbook segundo é executado, interrompe IR. de SSIS do Azure 
3. **Criar dois webhooks para o runbook**, um para a operação de início e outra para a operação de paragem. Utilize os URLs destas webhooks quando configurar atividades web num pipeline fábrica de dados. 
4. **Criar um pipeline do Data Factory**. O pipeline de que criar é constituído por três atividades. O primeiro **Web** atividade invoca o webhook primeiro iniciar IR. de SSIS do Azure O **procedimento armazenado** atividade executa um script de SQL Server que é executado o pacote SSIS. O segundo **Web** deixa de atividade IR. de SSIS do Azure Para obter mais informações sobre como invocar um pacote de SSIS do pipeline fábrica de dados utilizando a atividade de procedimento armazenado, consulte [invocar um pacote SSIS](how-to-invoke-ssis-package-stored-procedure-activity.md). Em seguida, crie um acionador de agenda para agendar o pipeline para serem executados quando a cadência que especificar.

## <a name="prerequisites"></a>Pré-requisitos
Se já que ainda não aprovisionou um tempo de execução de integração de SSIS do Azure, aprovisionar ao seguir as instruções no [tutorial](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-test-an-azure-automation-runbook"></a>Criar e testar um runbook de automatização do Azure
Nesta secção, execute os seguintes passos: 

1. Crie uma conta de automatização do Azure.
2. Crie um runbook do PowerShell na conta de automatização do Azure. O script do PowerShell associado ao runbook inicia ou deixa uma resposta a incidentes Azure SSIS com base no comando que especificar para o parâmetro de operação. 
3. Testar o runbook em ambos os iniciar e parar cenários para confirmar que funciona. 

### <a name="create-an-azure-automation-account"></a>Crie uma conta de Automatização do Azure
Se não tiver uma conta de automatização do Azure, siga as instruções neste passo. Para obter passos detalhados, consulte [criar uma conta de automatização do Azure](../automation/automation-quickstart-create-account.md). Como parte deste passo, cria um **Run As do Azure** conta (um principal de serviço no Azure Active Directory) e adicione-o ao **contribuinte** função da sua subscrição do Azure. Certifique-se de que seja a mesma que a subscrição que contém a fábrica de dados que tenha o IR. de SSIS do Azure A automatização do Azure utiliza esta conta para autenticar para o Azure Resource Manager e a funcionar nos seus recursos. 

1. Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.
2. Inicie sessão no [Portal do Azure](https://portal.azure.com/).    
3. Selecione **novo** no menu da esquerda, selecione **monitorização + gestão**e selecione **automatização**. 

    ![Novo -> monitorização + gestão -> automatização](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. No **adicionar conta de automatização** janela, siga os passos seguintes: 

    1. Especifique um **nome** para a conta de automatização. 
    2. Selecione o **subscrição** que tenha a fábrica de dados com o Azure SSIS IR. 
    3. Para **grupo de recursos**, selecione **criar nova** para criar um novo grupo de recursos ou selecione **utilizar existente** para selecionar um grupo de recursos existente. 
    4. Selecione um **localização** para a conta de automatização. 
    5. Confirme que **criar conta Run As** está definido como **Sim**. Um principal de serviço é criado no Azure Active Directory. É adicionado ao **contribuinte** função da sua subscrição do Azure
    6. Selecione **afixar ao dashboard** para que vê no dashboard do portal. 
    7. Selecione **Criar**. 

        ![Novo -> monitorização + gestão -> automatização](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. Pode ver o **estado da implementação** no dashboard e as notificações. 
    
    ![Implementação de automatização](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. Consulte a página inicial para a conta de automatização depois de criado com êxito. 

    ![Home page da automatização](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Importe os módulos de fábrica de dados

1. Selecione **módulos** no **recursos PARTILHADOS** secção no menu da esquerda e verifique se tem **AzureRM.Profile** e **AzureRM.DataFactoryV2** na lista de módulos.

    ![Verifique os módulos necessários](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  Aceda à galeria do PowerShell para o [AzureRM.DataFactoryV2 módulo](https://www.powershellgallery.com/packages/AzureRM.DataFactoryV2/), selecione **implementar a automatização do Azure**, selecione a sua conta de automatização e, em seguida, selecione **OK**. Retroceda para ver **módulos** no **recursos PARTILHADOS** secção no menu da esquerda e aguarde até ver a **estado** do **AzureRM.DataFactoryV2** módulo alteração **disponível**.

    ![Verifique se o módulo de fábrica de dados](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  Aceda à galeria do PowerShell para o [AzureRM.Profile módulo](https://www.powershellgallery.com/packages/AzureRM.profile/), clique em **implementar a automatização do Azure**, selecione a sua conta de automatização e, em seguida, selecione **OK**. Retroceda para ver **módulos** no **recursos PARTILHADOS** secção no menu da esquerda e aguarde até ver a **estado** do **AzureRM.Profile**módulo alteração **disponível**.

    ![Verifique se o módulo de perfil](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-a-powershell-runbook"></a>Criar runbook do PowerShell
O procedimento seguinte fornece os passos para criar um runbook do PowerShell. O script associada ao runbook está inicia/deixa uma resposta a incidentes Azure SSIS com base no comando especificado para o **operação** parâmetro. Esta secção fornece todos os detalhes para criar um runbook. Para obter mais informações, consulte [criar um runbook](../automation/automation-quickstart-create-runbook.md) artigo.

1. Mudar para o **Runbooks** separador e selecione **+ adicionar um runbook** da barra de ferramentas. 

    ![Adicionar um botão do runbook](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Selecione **criar um novo runbook**e execute os seguintes passos: 

    1. Para **nome**, tipo **StartStopAzureSsisRuntime**.
    2. Para **tipo de Runbook**, selecione **PowerShell**.
    3. Selecione **Criar**.
    
        ![Adicionar um botão do runbook](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Copie/cole o seguinte script para a janela de script do runbook. Guardar e, em seguida, publicar o runbook utilizando o **guardar** e **publicar** botões na barra de ferramentas. 

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

    ![Runbook botão Iniciar](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
6. No **iniciar Runbook** janela, execute os seguintes passos: 

    1. Para **nome do grupo de recursos**, introduza o nome do grupo de recursos com a fábrica de dados que tenha o IR. de SSIS do Azure 
    2. Para **nome da fábrica de dados**, introduza o nome da fábrica de dados que tenha o IR. de SSIS do Azure 
    3. Para **AZURESSISNAME**, introduza o nome do IR. de SSIS do Azure 
    4. Para **operação**, introduza **iniciar**. 
    5. Selecione **OK**.  

        ![Inicie a janela de runbook](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. Na janela da tarefa, selecione **saída** mosaico. Na janela de saída da tarefa, aguarde até ver a mensagem **# # # concluído # # #** depois de confirmar **# # # inicial # # #**. A iniciar uma resposta a incidentes SSIS Azure demora cerca de 20 minutos. Fechar o **tarefa** janela e voltar para o **Runbook** janela.

    ![Azure IR SSIS - iniciado](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Repita os dois passos anteriores, mas através da utilização **parar** como o valor para o **operação**. Iniciar o runbook novo ao selecionar o **iniciar** botão na barra de ferramentas. Especifique o nome do grupo de recursos, o nome da fábrica de dados e o nome de Azure SSIS IR. Para **operação**, introduza **parar**. 

    Na janela de saída da tarefa, aguarde até ver a mensagem **# # # concluído # # #** depois de confirmar **# # # parar # # #**. Parar uma resposta a incidentes SSIS do Azure não demorar até iniciar IR. de SSIS do Azure Fechar o **tarefa** janela e voltar para o **Runbook** janela.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Criar agendas para o runbook iniciar/parar IR de SSIS do Azure
Na secção anterior, criou um runbook de automatização do Azure que pode iniciar ou parar um IR. SSIS do Azure Nesta secção, vai criar duas agendas para o runbook. Quando configurar a agenda primeiro, especifique início para o parâmetro de operação. Da mesma forma, quando configurar o segundo agendamento, especifique parar para a operação. Para obter passos detalhados para criar agendas, consulte [criar uma agenda](../automation/automation-schedules.md#creating-a-schedule).

1. No **Runbook** janela, selecione **agendas**e selecione **+ adicionar uma agenda** na barra de ferramentas. 

    ![Azure IR SSIS - iniciado](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. No **agenda Runbook** janela, execute os seguintes passos: 

    1. Selecione **associar um agendamento ao runbook**. 
    2. Selecione **criar uma nova agenda**.
    3. No **nova agenda** janela, escreva **iniciar IR diariamente** para **nome**. 
    4. No **inicia secção**, durante o período de tempo, especifique uma hora de alguns minutos posterior à hora atual. 
    5. Para **periodicidade**, selecione **periódica**. 
    6. No **Repetir cada** secção, selecione **dia**. 
    7. Selecione **Criar**. 

        ![Agenda de início de IR de SSIS do Azure](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Mudar para o **parâmetros e definições de execução** separador. Especifique o nome do grupo de recursos, o nome da fábrica de dados e o nome de Azure SSIS IR. Para **operação**, introduza **iniciar**. Selecione **OK**. Selecione **OK** novamente para ver a agenda no **agendas** página do runbook. 

    ![Agenda para iniciar a máquina IR de SSIS do Azure](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Repita os dois passos anteriores para criar uma agenda com o nome **IR parar diariamente**. Este tempo, especifique a hora, pelo menos, 30 minutos após a hora que especificou para o **iniciar IR diariamente** agenda. Para **operação**, especifique **parar**. 
5. No **Runbook** janela, selecione **tarefas** no menu da esquerda. Deverá ver as tarefas criadas pelos agendamentos especificado em horas e os respetivos Estados. Pode ver detalhes sobre a tarefa, tais como o respetivo resultado semelhante ao que constatou quando testar o runbook. 

    ![Agenda para iniciar a máquina IR de SSIS do Azure](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. Depois de terminar de teste, desative as agendas editá-los e selecionando **não** para **ativado**. Selecione **agendas** no menu à esquerda, selecione o **iniciar IR diariamente/parar IR diariamente**e selecione **não** para **ativado**. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Criar webhooks para iniciar e parar a resposta a incidentes SSIS do Azure
Siga as instruções em [criar um webhook](../automation/automation-webhooks.md#creating-a-webhook) para criar dois webhooks para o runbook. Para o primeiro, especifique início como a operação de e para um segundo, especifique paragem como a operação. Guarde os URLs para ambos os webhooks algures (como um ficheiro de texto ou um bloco de notas do OneNote). Utilize estes URLs quando configurar atividades Web no pipeline de fábrica de dados. A imagem seguinte mostrada um exemplo de como criar um webhook que começa a resposta a incidentes SSIS do Azure:

1. No **Runbook** janela, selecione **Webhooks** do menu à esquerda e selecione **+ adicionar Webhook** na barra de ferramentas. 

    ![Webhooks -> Adicionar Webhook](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. No **adicionar Webhook** janela, selecione **criar novo webhook**, e efetue as seguintes ações: 

    1. Para **nome**, introduza **StartAzureSsisIR**. 
    2. Confirme que **ativado** está definido como **Sim**. 
    3. Copiar o **URL** e guardá-lo algures. Este passo é importante. Não verá o URL mais tarde. 
    4. Selecione **OK**. 

        ![Nova janela do Webhook](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Mudar para o **parâmetros e definições de execução** separador. Especifique o nome do grupo de recursos, o nome de factory de dados e o nome do Azure SSIS IR. Para **operação**, introduza **iniciar**. Clique em **OK**. Em seguida, clique em **Criar**. 

    ![Webhook - parâmetros e definições de execução](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Repita os três passos anteriores para criar outro webhook com o nome **StopAzureSsisIR**. Não se esqueça de copiar o URL. Ao especificar os parâmetros e definições de execução, introduza **parar** para **operação**. 

Deve ter dois URLs, um para o **StartAzureSsisIR** webhook e outra para o **StopAzureSsisIR** webhook. Pode enviar um pedido POST de HTTP para estes URLs para iniciar/parar o IR. SSIS do Azure 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Criar e agendar um pipeline do Data Factory que inicia/interrompe a resposta a incidentes
Esta secção mostra como utilizar uma atividade de Web para invocar webhooks que criou na secção anterior.

O pipeline de que criar é constituído por três atividades. 

1. O primeiro **Web** atividade invoca o webhook primeiro iniciar IR. de SSIS do Azure 
2. O **executar o pacote de SSIS** atividade ou **procedimento armazenado** o pacote SSIS a atividade é executada.
3. O segundo **Web** atividade invoca o webhook para parar o IR. de SSIS do Azure 

Depois de criar e testar o pipeline, pode criar um acionador de agenda e associa o pipeline. O acionador de agenda define uma agenda para o pipeline. Suponhamos, criar um acionador que esteja agendado para ser executada diariamente às 23: 00. O acionador é executado o pipeline em 23: 00 todos os dias. O pipeline inicia a resposta a incidentes SSIS do Azure, executa o pacote SSIS e, em seguida, interrompe IR. de SSIS do Azure 

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
4. Selecione **V2** para o **versão**.
5. Selecione a **localização** da fábrica de dados. Só são mostradas na lista as localizações que são suportadas para a criação de fábricas de dados.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Clique em **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Data Factory num separador à parte.

### <a name="create-a-pipeline"></a>Criar um pipeline

1. No **começar** página, selecione **criar pipeline**. 

   ![Página Introdução](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. No **atividades** caixa de ferramentas, expanda **geral**, arrastar largar o **Web** atividade para a superfície do designer de pipeline. No **geral** separador do **propriedades** janela, altere o nome da atividade para **StartIR**.

   ![Primeira atividade de Web - separador Geral](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Mudar para o **definições** separador o **propriedades** janela, e efetue as seguintes ações: 

    1. Para **URL**, cole o URL do webhook que começa a IR. de SSIS do Azure 
    2. Para **método**, selecione **POST**. 
    3. Para **corpo**, introduza `{"message":"hello world"}`. 
   
        ![Primeira atividade de Web - separador de definições](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)

4. Arrastar e largar a atividade de executar o pacote de SSIS ou a atividade de procedimento armazenado do **geral** secção o **atividades** caixa de ferramentas. Definir o nome da atividade para **RunSSISPackage**. 

5. Se selecionar a atividade de executar o pacote de SSIS, siga as instruções em [executar um pacote SSIS utilizando a atividade SSIS no Azure Data Factory](how-to-invoke-ssis-package-ssis-activity.md) para concluir a criação de atividade.  Certifique-se de que especificou um número suficiente de tentativas de repetição que são suficientemente frequente para aguardar a disponibilidade de IR SSIS do Azure, uma vez que demora até 30 minutos para iniciar. 

    ![Definições de repetição](media/how-to-schedule-azure-ssis-integration-runtime/retry-settings.png)

6. Se selecionar a atividade de procedimento armazenado, siga as instruções em [invocar um pacote SSIS utilizando a atividade de procedimento armazenado no Azure Data Factory](how-to-invoke-ssis-package-stored-procedure-activity.md) para concluir a criação de atividade. Certifique-se de que insere um script de Transact-SQL que aguarda que a disponibilidade de IR SSIS do Azure, uma vez que demora até 30 minutos para iniciar.
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

7. Ligar o **Web** atividade para o **executar o pacote de SSIS** ou **procedimento armazenado** atividade. 

    ![Ligar atividades Web e o procedimento armazenado](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-sproc.png)

8. Arrastar e largar outro **Web** para a direita da atividade de **executar o pacote de SSIS** atividade ou o **procedimento armazenado** atividade. Definir o nome da atividade para **StopIR**. 
9. Mudar para o **definições** separador o **propriedades** janela, e efetue as seguintes ações: 

    1. Para **URL**, cole o URL do webhook que deixa IR. de SSIS do Azure 
    2. Para **método**, selecione **POST**. 
    3. Para **corpo**, introduza `{"message":"hello world"}`.  
10. Ligar o **executar o pacote de SSIS** atividade ou **procedimento armazenado** para a última atividade **Web** atividade.

    ![Pipeline completa](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
11. Valide as definições de pipeline clicando **validar** na barra de ferramentas. Fechar o **relatório de validação de Pipeline** clicando **>>** botão. 

    ![Validar o pipeline](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>Testar a execução do pipeline

1. Selecione **executar teste** na barra de ferramentas para o pipeline. Consulte os resultados no **saída** janela no painel inferior. 

    ![Execução do teste](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. No **Runbook** página da sua conta de automatização do Azure, pode verificar se as tarefas executaram para iniciar e parar IR. de SSIS do Azure 

    ![Tarefas de Runbook](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. Inicie o SQL Server Management Studio. No **ligar ao servidor** janela, efetue as seguintes ações: 

    1. Para **nome do servidor**, especifique  **&lt;a base de dados SQL do Azure&gt;. database.windows.net**.
    2. Selecione **Opções >>**.
    3. Para **ligar à base de dados**, selecione **SSISDB**.
    4. Selecione **Ligar**. 
    5. Expanda **catálogos de serviços de integração** -> **SSISDB** -> a sua pasta -> **projetos** -> projeto Your SSIS -> **pacotes** . 
    6. O pacote SSIS com o botão direito e selecione **relatórios** -> **relatórios padrão** -> **todas as execuções**. 
    7. Certifique-se de que o pacote SSIS foi executada. 

        ![Certifique-se a execução de pacotes SSIS](./media/how-to-schedule-azure-ssis-integration-runtime/verfiy-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>Agendar o pipeline 
Agora que o pipeline funciona conforme esperado, pode criar um acionador para ser executada neste pipeline com uma cadência especificada. Para obter mais informações sobre como associar um acionador de agendamento de um pipeline, consulte [acionar o pipeline com base numa agenda](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. Na barra de ferramentas para o pipeline, selecione **acionador**e selecione **New/editar**. 

    ![Acionador -> Novo/Editar](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. No **adicionar Acionadores** janela, selecione **+ novo**.

    ![Adicionar novos Acionadores-](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. No **novo acionador**, efetue as seguintes ações: 

    1. Para **nome**, especifique um nome para o acionador. No exemplo seguinte, **ser executada diariamente** é o nome do acionador. 
    2. Para **tipo**, selecione **agenda**. 
    3. Para **data de início**, selecione uma data de início e a hora. 
    4. Para **periodicidade**, especificar a cadência para o acionador. No exemplo seguinte, de diária uma vez. 
    5. Para **final**, pode especificar a data e hora, selecionando o **no data** opção. 
    6. Selecione **ativado**. O acionador é ativado imediatamente depois de publicar a solução à fábrica de dados. 
    7. Selecione **Seguinte**.

        ![Acionador -> Novo/Editar](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. No **parâmetros de execução do acionador** página, consulte o aviso e selecione **concluir**. 
5. Publicar a solução à fábrica de dados selecionando **publicar todas as** no painel esquerdo. 

    ![Publicar todas](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)
6. Para monitorizar a execução do acionador e execução de pipeline, utilize o **Monitor** separador no lado esquerdo. Para obter passos detalhados, consulte [monitorizar o pipeline](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![Execuções de pipeline](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
7. Para ver as execuções de atividade associadas com um pipeline executar, selecione a primeira ligação (**ver a atividade é executada**) no **ações** coluna. Consulte as execuções de três atividade associadas a cada atividade no pipeline (primeiro Web atividade, a atividade de procedimento armazenado e a segunda atividade de Web). Para mudar novamente para ver a execução de pipeline, selecione **Pipelines** ligação na parte superior.

    ![Execuções de atividade](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
8. Também pode ver a execução do acionador selecionando **acionar executa** na lista de lista pendente junto a é o **Pipeline executa** na parte superior. 

    ![Execuções de acionador](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

## <a name="next-steps"></a>Passos Seguintes
Consulte a seguinte mensagem de blogue:
-   [Modernize e expandir os seus fluxos de trabalho ETL/ELT com atividades SSIS em ADF pipelines](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

Veja os artigos seguintes da documentação do SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Implementar, executar e monitorizar pacotes do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Connect to SSIS catalog on Azure (Ligar ao catálogo do SSIS no Azure)](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Agendar a execução de pacotes no Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Ligar a origens de dados no local com a autenticação do Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)
