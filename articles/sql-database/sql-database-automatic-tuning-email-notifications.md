---
title: "A otimização automática de e-mail guia de procedimentos notificações - SQL Database do Azure | Microsoft Docs"
description: Base de dados SQL do Azure analisa a consulta SQL e feita automaticamente a carga de trabalho do utilizador.
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
ms.reviewer: carlrab
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/31/2018
ms.author: v-daljep
ms.openlocfilehash: 3598c61634bf039e5c9500cc9a4f6b08e4830ce5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="email-notifications-for-automatic-tuning"></a>Notificações por e-mail para a otimização automática

Base de dados SQL recomendações de otimização são geradas pelo SQL Database do Azure [otimização automática](sql-database-automatic-tuning.md). Esta solução continuamente monitoriza e analisa as cargas de trabalho de fornecer de bases de dados SQL personalizada recomendações para cada base de dados individuais relacionados com a criação de índices, a eliminação de índice e a otimização de planos de execução da consulta de otimização.

SQL Server da base de dados automática otimização recomendações podem ser visualizada no [portal do Azure](sql-database-advisor-portal.md), obtido com [REST API](https://docs.microsoft.com/en-us/rest/api/sql/databaserecommendedactions/listbydatabaseadvisor) chama ou através da utilização [T-SQL](https://azure.microsoft.com/en-us/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) e [ PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaserecommendedaction) comandos. Este artigo baseia-se utilizando um script do PowerShell para obter recomendações de otimização automáticas.

## <a name="automate-email-notifications-for-automatic-tuning-recommendations"></a>Automatizar as notificações por e-mail para recomendações de otimização automáticas

A seguinte solução automatiza o envio de notificações de correio eletrónico que contém recomendações de otimização automáticas. A solução descrita consiste automatizar a execução de um script do PowerShell para obter recomendações de otimização com [da automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro), e através da tarefa de entrega de e-mail de automatização de agendamento [Flow Microsoft ](https://flow.microsoft.com).

## <a name="create-azure-automation-account"></a>Criar conta de automatização do Azure

Para utilizar a automatização do Azure, o primeiro passo é criar uma conta de automatização e configurá-la com recursos do Azure a utilizar para a execução do script do PowerShell. Para saber mais sobre a automatização do Azure e as respetivas capacidades, consulte o artigo [introdução da automatização do Azure](https://docs.microsoft.com/en-us/azure/automation/automation-offering-get-started).

Siga estes passos para criar a conta de automatização do Azure através do método de selecionar e configurar a aplicação de automatização do Marketplace:

- Inicie sessão no portal do Azure
- Clique em "**+ criar um recurso**" no canto superior esquerdo
- Procure "**automatização**" (prima enter)
- Clique na aplicação automatização nos resultados da pesquisa

![A adição da automatização do Azure](./media/sql-database-automatic-tuning-email-notifications/howto-email-01.png)

- Uma vez no interior do painel de "Criar uma conta de automatização", clique em "**criar**"
- Preencher as informações necessárias: introduza um nome para esta conta de automatização, selecione os recursos de ID e o Azure de subscrição do Azure a utilizar para a execução do script do PowerShell
- Para o "**criar Azure conta Run As**" opção, selecione **Sim** para configurar o tipo de conta sob a qual PowerShell o script é executado com a ajuda da automatização do Azure. Para obter mais informações sobre tipos de conta, consulte o artigo [conta Run As](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account)
- Concluir a criação da conta de automatização ao clicar no **criar**

> [!TIP]
> Registe o seu nome de conta de automatização do Azure, o ID de subscrição e recursos (como copiar-colar para um bloco de notas) exatamente tal como foi introduzido ao criar a aplicação de automatização. Terá desta informação mais tarde.
>

Se tiver várias subscrições do Azure para o qual gostaria de criar uma automatização a mesma, terá de repetir este processo para as outras subscrições.

## <a name="update-azure-automation-modules"></a>Atualizar módulos da automatização do Azure

O script do PowerShell para obter automático otimizar a recomendação utiliza [Get-AzureRmResource](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Resources/Get-AzureRmResource) e [Get-AzureRmSqlDatabaseRecommendedAction](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Sql/Get-AzureRmSqlDatabaseRecommendedAction) comandos para que a atualização dos módulos do Azure para o versão 4 e posterior é necessária.

Siga estes passos para atualizar os módulos do Azure PowerShell:

- Aceder ao painel de aplicação de automatização e selecione "**módulos**" no menu do lado esquerdo (desloque para baixo, este item de menu é em recursos partilhados).
- No painel de módulos, clique em "**atualização módulos do Azure**" na parte superior e aguarde até que é apresentada a mensagem "módulos do Azure foi atualizados com". Este processo pode demorar alguns minutos a concluir.

![Atualizar módulos da automatização do Azure](./media/sql-database-automatic-tuning-email-notifications/howto-email-02.png)

Versão necessária do AzureRM.Resources AzureRM.Sql módulos às necessidades e a versão 4 e posterior.

## <a name="create-azure-automation-runbook"></a>Criar o Runbook de automatização do Azure

O passo seguinte consiste em criar um Runbook na automatização do Azure dentro da qual o script do PowerShell para obtenção de otimização recomendações reside.

Siga estes passos para criar um novo runbook de automatização do Azure:

- Aceder à conta de automatização do Azure que criou no passo anterior
- Uma vez no painel de conta de automatização, clique em de "**Runbooks**" item de menu no lado esquerdo para criar um novo runbook de automatização do Azure com o script do PowerShell. Para obter mais informações sobre como criar runbooks de automatização, consulte [criar um novo runbook](../automation/automation-creating-importing-runbook.md).
- Para adicionar um novo runbook, clique em de "**+ adicionar um runbook**" opção de menu e, em seguida, clique em de "**rápido criar – criar um novo runbook**".
- No painel de Runbook, escreva o nome do runbook (para efeitos deste exemplo, "**AutomaticTuningEmailAutomation**" for utilizado), selecione o tipo de runbook como **PowerShell** e escrever uma descrição do Este runbook para descrever o objetivo.
- Clique em de **criar** botão para concluir a criação de um novo runbook

![Adicionar runbook de automatização do Azure](./media/sql-database-automatic-tuning-email-notifications/howto-email-03.png)

Siga estes passos para carregar um script do PowerShell no interior do runbook criado:

- Dentro de "**editar Runbook do PowerShell**"painel, selecione"**RUNBOOKS**" no menu de árvore e expanda a vista até ver o nome do runbook (neste exemplo " **AutomaticTuningEmailAutomation**"). Selecione este runbook.
- Na primeira linha do "Editar Runbook do PowerShell" (começando com o número de 1), copiar-colar o seguinte código de script do PowerShell. Este script do PowerShell é fornecido tal como-está a começar. Modificar o script para suite às suas necessidades.

O cabeçalho do script do PowerShell fornecido, terá de substituir `<SUBSCRIPTION_ID_WITH_DATABASES>` com o ID de subscrição do Azure. Para saber como obter o ID de subscrição do Azure, consulte [ao obter o GUID de subscrição do Azure](https://blogs.msdn.microsoft.com/mschray/2016/03/18/getting-your-azure-subscription-guid-new-portal/).

Em caso de várias subscrições, pode adicioná-los como delimitada por vírgulas para a propriedade "$subscriptions" no cabeçalho do script.

```PowerShell
# PowerShell script to retrieve Azure SQL Database Automatic tuning recommendations.
#
# Provided “as-is” with no implied warranties or support.
# The script is released to the public domain.
#
# Replace <SUBSCRIPTION_ID_WITH_DATABASES> in the header with your Azure subscription ID.
#
# Microsoft Azure SQL Database team, 2018-01-22.

# Set subscriptions : IMPORTANT – REPLACE <SUBSCRIPTION_ID_WITH_DATABASES> WITH YOUR SUBSCRIPTION ID 
$subscriptions = ("<SUBSCRIPTION_ID_WITH_DATABASES>", "<SECOND_SUBSCRIPTION_ID_WITH_DATABASES>", "<THIRD_SUBSCRIPTION_ID_WITH_DATABASES>")

# Get credentials
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Define the resource types
$resourceTypes = ("Microsoft.Sql/servers/databases")
$advisors = ("CreateIndex", "DropIndex");
$results = @()

# Loop through all subscriptions
foreach($subscriptionId in $subscriptions) {    
    Select-AzureRmSubscription -SubscriptionId $subscriptionId    
    $rgs = Get-AzureRmResourceGroup

    # Loop through all resource groups
    foreach($rg in $rgs) {
        $rgname = $rg.ResourceGroupName;

        # Loop through all resource types
        foreach($resourceType in $resourceTypes) {
            $resources = Get-AzureRmResource -ResourceGroupName $rgname -ResourceType $resourceType    

            # Loop through all databases
            # Extract resource groups, servers and databases
            foreach ($resource in $resources) {
                $resourceId = $resource.ResourceId
                if ($resourceId -match ".*RESOURCEGROUPS/(?<content>.*)/PROVIDERS.*") {
                    $ResourceGroupName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*SERVERS/(?<content>.*)/DATABASES.*") {
                    $ServerName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*/DATABASES/(?<content>.*)") {
                    $DatabaseName = $matches['content']
                } else {
                    continue 
                }

                # Skip if master
                if ($DatabaseName -eq "master") {
                    continue
                }

                # Loop through all Automatic tuning recommendation types
                foreach ($advisor in $advisors) {
                    $recs = Get-AzureRmSqlDatabaseRecommendedAction -ResourceGroupName $ResourceGroupName -ServerName $ServerName  -DatabaseName $DatabaseName -AdvisorName $advisor
                    foreach ($r in $recs) {
                        if ($r.State.CurrentValue -eq "Active") {
                            $object = New-Object -TypeName PSObject
                            $object | Add-Member -Name 'SubscriptionId' -MemberType Noteproperty -Value $subscriptionId
                            $object | Add-Member -Name 'ResourceGroupName' -MemberType Noteproperty -Value $r.ResourceGroupName
                            $object | Add-Member -Name 'ServerName' -MemberType Noteproperty -Value $r.ServerName
                            $object | Add-Member -Name 'DatabaseName' -MemberType Noteproperty -Value $r.DatabaseName
                            $object | Add-Member -Name 'Script' -MemberType Noteproperty -Value $r.ImplementationDetails.Script
                            $results += $object
                        }
                    }
                }                
            }
        }
    }
}

# Format and output results for the email
$table = $results | Format-List
Write-Output $table
```

Clique em de "**guardar**" botão no canto superior direito para guardar o script. Quando estiver satisfeito com o script, clique em de "**publicar**" botão publicar este runbook. 

No painel do runbook principal, pode optar por clique em de "**iniciar**" botão para **testar** o script. Clique em de "**saída**" para ver os resultados do script foi executado. Este resultado vai ser o conteúdo do seu correio eletrónico. A saída de exemplo do script pode ser vista na captura de ecrã seguinte.

![Executar vista automática otimização recomendações com a automatização do Azure](./media/sql-database-automatic-tuning-email-notifications/howto-email-04.png)

Certifique-se de ajustar o conteúdo ao personalizar o script do PowerShell para as suas necessidades.

Com os passos acima, o script do PowerShell para obter recomendações de otimização automáticas é carregado na automatização do Azure. O passo seguinte é para automatizar e agendar a tarefa de entrega de correio eletrónico.

## <a name="automate-the-email-jobs-with-microsoft-flow"></a>Automatizar as tarefas de e-mail com o Microsoft Flow

Para concluir a solução, como o passo final, crie um fluxo de automatização Flow Microsoft consiste em três ações (tarefas): 

1. "**Da automatização do azure - criar tarefa**" – utilizado para executar o script do PowerShell para obter automático otimização recomendações dentro do runbook de automatização do Azure
2. "**Da automatização do azure - saída da tarefa Get**" – utilizado para obter o resultado do script do PowerShell executado
3. "**Office 365 Outlook – enviar um e-mail**" – utilizado para enviar correio eletrónico. Correio electrónico é enviados utilizando a conta do Office 365 do indivíduo criar o fluxo.

Para saber mais sobre as funcionalidades do Microsoft Flow, consulte [introdução ao Microsoft Flow](https://docs.microsoft.com/en-us/flow/getting-started).

Pré-requisitos para este passo é a inscrever-se [Microsoft Flow](https://flow.microsoft.com) conta e inicie sessão. Uma vez dentro da solução, siga estes passos para configurar um **novo fluxo**:

- Acesso "**meu fluxos**" item de menu
- Dentro os meus fluxos, selecione o "**+ criar em branco**" ligação na parte superior da página
- Clique na ligação "**procure centenas de conectores e acionadores**" na parte inferior da página
- O tipo de campo de pesquisa "**periodicidade**" e selecione "**agenda - periodicidade**" dos resultados de pesquisa para agendar a tarefa de entrega de e-mail para ser executada.
- No painel de periodicidade no campo frequência, selecione a frequência de agendamento para este fluxo executar, tais como o envio automático de correio eletrónico cada minuto, hora, dia, semana, etc.

O passo seguinte consiste em Adicionar três tarefas (criar, resultados de get e envio de correio eletrónico) para o fluxo periódico recentemente criado. Para realizar a adicionar as tarefas necessárias para o fluxo, siga estes passos:

1. Criar a ação a executar o script do PowerShell para obter recomendações de otimização
- Selecione "**+ novo passo**", seguido"**adicionar uma ação**" no painel de fluxo de periodicidade
- O tipo de campo de pesquisa "**automatização**"e selecione"**automatização do Azure – criar tarefa**" dos resultados de pesquisa
- No painel de tarefas a criar, configure as propriedades da tarefa. Para esta configuração, terá de detalhes da sua subscrição do Azure ID, grupo de recursos e a conta de automatização **anteriormente registados** no **painel de conta de automatização**. Para saber mais sobre as opções disponíveis nesta secção, consulte o artigo [da automatização do Azure - criar tarefa](https://docs.microsoft.com/connectors/azureautomation/#Create_job).
- Concluir a criação esta ação, clicando no "**Guardar fluxo**"

2. Criar a ação para obter o resultado do script do PowerShell executado
- Selecione "**+ novo passo**", seguido"**adicionar uma ação**" no painel de fluxo de periodicidade
- Na pesquisa registada no tipo "**automatização**"e selecione"**automatização do Azure – resultado da tarefa Get**" dos resultados de pesquisa. Para saber mais sobre as opções disponíveis nesta secção, consulte o artigo [automatização do Azure – resultado da tarefa Get](https://docs.microsoft.com/connectors/azureautomation/#Get_job_output).
- Preencher os campos necessários (semelhante ao criar a tarefa anterior) - preencher a sua subscrição do Azure ID, grupo de recursos e a conta de automatização (tal como foi introduzido no painel de conta de automatização)
- Clique em dentro do campo "**ID da tarefa**" para o "**conteúdo dinâmico**" menu apareçam. A partir da dentro deste menu, selecione a opção "**ID da tarefa**".
- Concluir a criação esta ação, clicando no "**Guardar fluxo**"

3. Criar a ação para enviar e-mails utilizando a integração do Office 365
- Selecione "**+ novo passo**", seguido"**adicionar uma ação**" no painel de fluxo de periodicidade
- Na pesquisa registada no tipo "**enviar um e-mail**"e selecione"**Office 365 Outlook – enviar um e-mail**" dos resultados de pesquisa
- No "**para**" campo escreva o endereço de correio eletrónico para o qual terá de enviar o e-mail de notificação
- No "**requerente**" no assunto do seu correio eletrónico, por exemplo "recomendações otimização automáticas notificação por e-mail" de tipo de campo
- Clique em dentro do campo "**corpo**" para o "**conteúdo dinâmico**" menu apareçam. Do dentro deste menu, em "**obter o resultado da tarefa**", selecione"**conteúdo**" 
- Concluir a criação esta ação, clicando no "**Guardar fluxo**"

> [!TIP]
> Para enviar correio eletrónico automatizado para destinatários diferentes, crie fluxos separados. Nestes fluxos adicionais, altere o endereço de e-mail destinatário no campo "A" e a linha de assunto de e-mail no campo "Assunto". Criar novos runbooks na automatização do Azure com personalizadas de scripts do PowerShell (tal como com a alteração do ID de subscrição do Azure) permite que a personalização mais automatizada cenários, por exemplo, tais estão relacionada separados destinatários automatizada de otimização recomendações para subscrições separadas.
>

O procedimento acima conclui os passos necessários para configurar o fluxo de trabalho de tarefa de entrega de correio eletrónico. O fluxo completo consiste em três ações incorporadas é mostrado na imagem seguinte.

![Vista de fluxo de notificações de e-mail de otimização automática](./media/sql-database-automatic-tuning-email-notifications/howto-email-05.png)

Para testar o fluxo, clique em "**executar agora**" no canto superior direito dentro do painel de fluxo.

Estatísticas de tarefas automatizadas, que mostra o êxito de notificações de correio eletrónico enviadas, em execução podem ser vistas a partir do painel de análise de fluxo.

![Executar o fluxo de notificações de e-mail de otimização automática](./media/sql-database-automatic-tuning-email-notifications/howto-email-06.png)

A análise de fluxo é útil para monitorizar o êxito de execuções de tarefa e, se necessário para resolução de problemas.  No caso de resolução de problemas, pode também querer examine o registo de execução de script de PowerShell acessível através da aplicação de automatização do Azure.

O resultado final de mensagem de correio eletrónico automatizada semelhante para o seguinte e-mail recebido depois de criar e executar esta solução:

![Saída de notificações de correio eletrónico de otimização automática do e-mail de exemplo](./media/sql-database-automatic-tuning-email-notifications/howto-email-07.png)

Ao ajustar o script do PowerShell, pode ajustar a saída e a formatação da mensagem de e-mail automatizada para as suas necessidades.

Pode personalizar ainda mais a solução para criar notificações de e-mail com base num evento específico Otimização e para vários destinatários para várias subscrições ou bases de dados, consoante os cenários personalizados. 

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais em como automático de otimização pode ajudar a melhorar o desempenho de base de dados, consulte [otimização automática da base de dados do Azure SQL](sql-database-automatic-tuning.md).
- Para ativar a otimização automática da base de dados do Azure SQL Server para gerir a sua carga de trabalho, consulte [ativar a otimização automática](sql-database-automatic-tuning-enable.md).
- Para rever e aplicar automático otimização recomendações manualmente, consulte o artigo [localizar e aplicar as recomendações de desempenho](sql-database-advisor-portal.md).
