---
title: "VMs de início/paragem durante a solução de off-hours (pré-visualização) | Microsoft Docs"
description: "Esta solução de gestão de VM inicia, interrompe as máquinas virtuais do Azure Resource Manager com base numa agenda e proativamente monitoriza da análise de registos."
services: automation
documentationCenter: 
authors: eslesar
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: magoedte
ms.openlocfilehash: 4424cbb83bdb31c60e15d62f9387b4050611a98d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>VMs de início/paragem durante a solução de off-hours (pré-visualização) na automatização do Azure

As VMs de início/paragem durante off-hours solução é iniciado e interrompe as máquinas virtuais do Azure agendas definido pelo utilizador, fornece informações através de Log Analytics do Azure e envia mensagens de correio eletrónico opcionais utilizando [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Suporta o Azure Resource Manager e VMs clássicas para a maioria dos cenários. 

Esta solução oferece uma opção de automatização descentralizada para os utilizadores que pretende reduzir os custos através da utilização de recursos sem servidor, de baixo custo. Com esta solução, pode:

* Agendar as VMs para iniciar e parar.
* Agendar as VMs para iniciar e parar por ordem ascendente através da utilização de etiquetas do Azure (não suportado para VMs clássicas).
* VMs de paragem automática com base na utilização de CPU baixa.

## <a name="prerequisites"></a>Pré-requisitos

- Os runbooks funcionam com uma [conta Run As do Azure](automation-offering-get-started.md#authentication-methods).  A conta Run As é o método de autenticação preferencial porque utiliza a autenticação de certificado em vez de uma palavra-passe que poderá expirar ou alterado frequentemente.  

- Esta solução gere apenas as VMs que estão na mesma subscrição, como a sua conta de automatização do Azure.  

- Esta solução é implementada apenas para as seguintes regiões do Azure: Sudeste da Austrália, Canadá Central, Índia Central, EUA leste, leste do Japão, Sudeste asiático, sul do RU e Europa Ocidental.  
    
    > [!NOTE]
    > Os runbooks gerir a agenda VM pode visar VMs em qualquer região.  

- Para enviar notificações por e-mail ao concluir os runbooks de início e a paragem da VM, durante a integração do Azure Marketplace, selecione **Sim** implementar SendGrid. 

    > [!IMPORTANT]
    > SendGrid é um serviço de terceiros. Para obter suporte, contacte [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Limitações com SendGrid são:

    * Um máximo de uma conta de SendGrid por utilizador por subscrição.
    * Um máximo de duas contas SendGrid por subscrição.

Se tiver implementado uma versão anterior desta solução, tem primeiro de eliminá-lo da sua conta antes de implementar esta versão.  

## <a name="solution-components"></a>Componentes da solução

Esta solução inclui pré-configurada runbooks, as agendas e integração com a análise de registos, pelo que pode personalizar o arranque e encerramento das máquinas virtuais de acordo com as suas necessidades empresariais. 

### <a name="runbooks"></a>Runbooks

A tabela seguinte lista os runbooks implementados à sua conta de automatização.  Não deve efetuar alterações ao código de runbook. Em vez disso, escreva o seu próprio runbook para a nova funcionalidade.

> [!IMPORTANT]
> Não execute diretamente qualquer runbook com "subordinados" anexado ao respetivo nome.
>

Todos os runbooks principais incluem o *WhatIf* parâmetro. Quando definido como **verdadeiro**, *WhatIf* suporta com detalhes sobre o comportamento exato runbook demora quando executar sem o *WhatIf* parâmetro e valida o correto VMs estão a ser visados.  Um runbook só efetua as ações definidas quando o *WhatIf* parâmetro está definido como **falso**. 

|**Runbook** | **Parâmetros** | **Descrição**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Chamado apenas a partir do runbook principal. Cria alertas numa base por recurso para o cenário de AutoStop.| 
|AutoStop_CreateAlert_Parent | WhatIf: VERDADEIRO ou FALSO <br> VMList | Cria ou atualiza as regras de alertas do Azure em VMs nos grupos direcionados subscrição ou recurso. <br> VMList: Lista separada por vírgulas de VMs.  Por exemplo, *vm1 vm2, vm3*.| 
|AutoStop_Disable | nenhum | Desativa AutoStop alertas e a agenda predefinida.| 
|AutoStop_StopVM_Child | WebHookData | Chamado apenas a partir do runbook principal. Regras de alerta de chamar este runbook para parar a VM.|  
|Bootstrap_Main | nenhum | Utilizado uma vez para definir configurações de arranque do sistema, tais como webhookURI, que normalmente não estão acessíveis a partir do Azure Resource Manager. Este runbook é automaticamente removido após a implementação com êxito.|  
|ScheduledStartStop_Child | VMName <br> Ação: Parar ou iniciar <br> resourceGroupName | Chamado apenas a partir do runbook principal. Executa parar ou iniciar de paragem da agendada.|  
|ScheduledStartStop_Parent | Ação: Parar ou iniciar <br> WhatIf: VERDADEIRO ou FALSO | Este procedimento afeta todas as VMs na subscrição. Editar o **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames** a executar apenas nestes grupos de recursos de destino. Também pode excluir VMs específicas atualizando o **External_ExcludeVMNames** variável. *WhatIf* comporta-se os mesmos que outros runbooks.|  
|SequencedStartStop_Parent | Ação: Parar ou iniciar <br> WhatIf: VERDADEIRO ou FALSO | Criar etiquetas com o nome **SequenceStart** e **SequenceStop** em cada VM para o qual pretende que a atividade iniciar/parar de sequência. O valor da etiqueta deve ser um número inteiro positivo (1, 2, 3) que corresponde à ordem na qual pretende iniciar ou parar. *WhatIf* comporta-se os mesmos que outros runbooks. <br> **Tenha em atenção**: VMs tem de ser em grupos de recursos definidos como External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames e External_ExcludeVMNames em variáveis de automatização do Azure. Têm de ter as etiquetas adequadas para ações entre em vigor.|

### <a name="variables"></a>Variáveis

A tabela seguinte lista as variáveis criadas na sua conta de automatização.  Só deve modificar variáveis de prefixo **externo**. Modificar variáveis de prefixo **interno** faz com que os efeitos indesejáveis.  

|**Variável** | **Descrição**|
---------|------------|
|External_AutoStop_Condition | O operador condicional necessário para configurar a condição antes de acionar um alerta. Os valores aceitáveis são **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, e **LessThanOrEqual**.|  
|External_AutoStop_Description | O alerta para parar a VM se a percentagem de CPU exceder o limiar.|  
|External_AutoStop_MetricName | O nome da métrica de desempenho para o qual a regra de alerta do Azure está a ser configurado.| 
|External_AutoStop_Threshold | O limiar para a regra de alerta do Azure especificado na variável *External_AutoStop_MetricName*. Percentagem valores podem variar entre 1 e 100.|  
|External_AutoStop_TimeAggregationOperator | O operador de agregação hora, qual é aplicado ao tamanho da janela selecionados para avaliar a condição. Os valores aceitáveis são **médio**, **mínimo**, **máximo**, **Total**, e **último**.|  
|External_AutoStop_TimeWindow | O tamanho da janela durante os quais o Azure analisa as métricas selecionadas para acionar um alerta. Este parâmetro aceita entrada no formato timespan. Os valores possíveis são de 5 minutos para 6 horas.|  
|External_EmailFromAddress | Especifica o remetente do e-mail.|  
|External_EmailSubject | Especifica o texto para a linha do assunto do e-mail.|  
|External_EmailToAddress | Especifica os destinatários do e-mail. Separar os nomes com uma vírgula.|  
|External_ExcludeVMNames | Introduza os nomes VM a serem excluídos, separando os nomes com uma vírgula sem espaços.|  
|External_IsSendEmail | Especifica a opção para enviar notificações de e-mail após a conclusão.  Especifique **Sim** ou **não** para não enviar correio eletrónico.  Esta opção deve ser **não** se não tiver ativado as notificações de e-mail durante a implementação inicial.|  
|External_Start_ResourceGroupNames | Especifica um ou mais grupos de recursos, separando os valores com uma vírgula, direcionada para ações de início.|  
|External_Stop_ResourceGroupNames | Especifica um ou mais grupos de recursos, separando os valores com uma vírgula, direcionada para ações de paragem.|  
|Internal_AutomationAccountName | Especifica o nome da conta de automatização.|  
|Internal_AutoSnooze_WebhookUri | Especifica o que URI de Webhook chamado para o cenário de AutoStop.|  
|Internal_AzureSubscriptionId | Especifica o ID de subscrição do Azure.|  
|Internal_ResourceGroupName | Especifica o nome de grupo de recursos da conta de automatização.|  
|Internal_SendGridAccountName | Especifica o nome de conta do SendGrid.|  
|Internal_SendGridPassword | Especifica a palavra-passe de conta do SendGrid.|  

<br>

Em todos os cenários, o **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, e **External_ExcludeVMNames** variáveis são necessárias para filtragem VMs, à exceção de fornecer uma lista separada por vírgulas de VMs para o **AutoStop_CreateAlert_Parent** runbook. Ou seja, as suas VMs tem de residir em grupos de recursos de destino para iniciar e parar ações para ocorrer. A lógica um pouco funciona da seguinte forma de política do Azure, na medida em que pode o subscrição ou grupo de recursos de destino e ter ações herdadas por VMs recentemente criadas. Esta abordagem evita a necessidade de manter uma agenda separada para cada VM e gerir inicia e deixa de escala.

### <a name="schedules"></a>Agendas

A tabela seguinte lista cada as agendas predefinidas criadas na sua conta de automatização.  Pode modificá-las ou criar os seus próprios agendamentos personalizados.  Por predefinição, cada um destes estão desativadas, exceto para **Scheduled_StartVM** e **Scheduled_StopVM**.

Não deve ativar todas as agendas, porque pode criar ações de agenda sobrepostos. É melhor determinar quais as otimizações de que pretende executar e modificar em conformidade.  Ver os cenários de exemplo na secção Descrição geral para obter explicações mais.   

|**Nome da agenda** | **Frequência** | **Descrição**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Cada 8 horas | Executa o runbook de AutoStop_CreateAlert_Parent a cada 8 horas, por sua vez interrompe baseados em VM valores existentes na External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames e External_ExcludeVMNames em variáveis de automatização do Azure.  Em alternativa, pode especificar uma lista separada por vírgulas de VMs utilizando o parâmetro VMList.|  
|Scheduled_StopVM | Utilizador definido, diariamente | Executa o runbook Scheduled_Parent com um parâmetro de *parar* diariamente à hora especificada.  Para automaticamente todas as VMs que cumpram as regras definidas pelo variáveis de recurso. Deve ativar o agendamento relacionado, **agendada-StartVM**.|  
|Scheduled_StartVM | Utilizador definido, diariamente | Executa o runbook Scheduled_Parent com um parâmetro de *iniciar* diariamente à hora especificada.  Inicia automaticamente todas as VMs que cumpram as regras definidas pelas variáveis adequadas.  Deve ativar o agendamento relacionado, **agendada StopVM**.|
|StopVM sequenciado | 1:00:00 (UTC), cada sexta-feira | Executa o runbook Sequenced_Parent com um parâmetro de *parar* cada sexta-feira no momento especificado.  Sequencialmente (ascendente) deixa de todas as VMs com uma etiqueta de **SequenceStop** definidos pelas variáveis adequadas.  Consulte a secção de Runbooks para obter mais detalhes sobre as variáveis de recurso e valores de etiqueta.  Deve ativar o agendamento relacionado, **Sequenced-StartVM**.|
|Sequenciado-StartVM | 1:00 PM (UTC), cada segunda-feira | Executa o runbook Sequenced_Parent com um parâmetro de *iniciar* cada segunda-feira no momento especificado. Sequencialmente (descendente) inicia todas as VMs com uma etiqueta de **SequenceStart** definidos pelas variáveis adequadas.  Consulte a secção de Runbooks para obter mais detalhes sobre as variáveis de recurso e valores de etiqueta.  Deve ativar o agendamento relacionado, **Sequenced StopVM**.|

<br>

## <a name="configuration"></a>Configuração

Execute os seguintes passos para adicionar as VMs de início/paragem durante a solução de off-hours à sua conta de automatização e, em seguida, configure as variáveis para personalizar a solução.

1. No portal do Azure, clique em **Novo**.<br> ![Portal do Azure](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. No painel de Marketplace, escreva uma palavra-chave como **iniciar** ou **iniciar/parar**. À medida que começa a escrever, a lista filtra com base na sua entrada. Em alternativa, pode escrever um ou mais palavras-chave do nome completo da solução e, em seguida, prima Enter.  Selecione **VMs de início/paragem durante off-hours [pré-visualização]** de resultados da pesquisa.  
3. No **VMs de início/paragem durante off-hours [pré-visualização]** painel para a solução selecionada, reveja as informações de resumo e, em seguida, clique em **criar**.  
4. O **Adicionar solução** painel aparece. Lhe for pedido para configurar a solução antes de importá-lo na sua subscrição de automatização.<br><br> ![Painel Adicionar Solução de Gestão de VMs](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  No **Adicionar solução** painel, selecione **área de trabalho**. Selecione uma área de trabalho do OMS que está ligada à mesma subscrição do Azure que está a ser a conta de automatização. Se não tiver uma área de trabalho, selecione **criar nova área de trabalho**. No **área de trabalho OMS** painel, efetue o seguinte: 
   - Especifique um nome para a **Área de Trabalho do OMS**.
   - Selecione um **subscrição** para ligar a selecionando a partir da lista pendente, se a predefinição selecionada não é adequada.
   - Para **grupo de recursos**, pode criar um novo grupo de recursos ou selecione um existente.  
   - Selecione uma **Localização**.  Atualmente, são as únicas localizações disponíveis **Sudeste da Austrália**, **Canadá Central**, **Índia Central**, **EUA Leste**, **Este do Japão**, **Sudeste asiático**, **sul do RU**, e **Europa Ocidental**.
   - Selecione um **Escalão de preço**.  A solução oferece duas camadas: gratuitas e pagas OMS.  O escalão gratuito tem um limite na quantidade de dados recolhidos por dia, o período de retenção e os minutos de tempo de execução da tarefa de runbook.  O escalão pago do OMS não tem limite quanto à quantidade de dados recolhidos diariamente.  

        > [!NOTE]
        > Embora a camada por GB (autónomo) paga é apresentada como uma opção, não é aplicável.  Se selecioná-lo e continuar com a criação desta solução na sua subscrição, falhará.  Esta opção será contemplada quando a solução for lançada oficialmente.<br>Minutos de tarefa de automatização solução apenas utiliza e deste ingestão de registo.  Não adiciona nós OMS adicionais ao seu ambiente.  

6. Depois de fornecer as informações necessárias sobre o **área de trabalho OMS** painel, clique em **criar**.  Pode controlar o progresso em **notificações** no menu, que devolve ao **Adicionar solução** painel quando terminar.  
7. No **Adicionar solução** painel, selecione **conta de automatização**.  Se estiver a criar uma nova área de trabalho do OMS, terá de criar também uma nova conta de automatização ser associadas a ele.  Selecione **criar uma conta de automatização**e o **conta de automatização adicionar** painel, fornece o seguinte: 
  - No campo **Nome**, introduza o nome da conta de Automatização.

    Todas as outras opções automaticamente são povoadas com base na área de trabalho OMS selecionada. Estas opções não podem ser modificadas.  O método de autenticação predefinido para os runbooks incluídos nesta solução é a conta Run As do Azure.  Depois de clicar em **OK**, as opções de configuração são validadas e a conta de automatização é criada.  Pode acompanhar o progresso em **Notificações**, no menu. 

    Caso contrário, pode selecionar uma conta Run As de Automatização existente.  Tenha em atenção que a conta que selecionou já não pode ser ligada a outra área de trabalho do OMS. Se já estiver ligado, recebe uma mensagem e tem de selecionar uma conta Run As de automatização diferente ou crie um novo.<br><br> ![Conta de Automatização Já Associada à Área de Trabalho do OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Por fim, no **Adicionar solução** painel, selecione **configuração**. O **parâmetros** painel aparece.<br><br> ![Painel de parâmetros para a solução](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  Aqui, é-lhe pedido que:  
   - Especifique o **nomes ResourceGroup de destino**. Estes são os nomes de grupo de recursos que contêm as VMs para ser gerido por esta solução.  Pode introduzir mais do que um nome e separe cada um com uma vírgula (valores não são sensíveis a maiúsculas e minúsculas).  Se quiser segmentar todas as VMs em todos os grupos de recursos da subscrição, a utilização de um caráter universal é suportada.
   - Especifique o **VM excluir lista (cadeia)**. Este é o nome de um ou mais máquinas virtuais do grupo de recursos de destino.  Pode introduzir mais do que um nome e separe cada um com uma vírgula (valores não são sensíveis a maiúsculas e minúsculas).  Utilizar um caráter universal é suportado.
   - Selecione um **agenda**. Este é um periódica data e hora para iniciar e parar as VMs nos grupos de recurso de destino.  Por predefinição, a agenda está configurada para o fuso horário UTC. Selecionar uma região diferente não está disponível.  Para configurar a agenda para o seu fuso horário específico depois de configurar a solução, consulte [modificar a agenda de arranque e encerramento](#modifying-the-startup-and-shutdown-schedule).
   - Para receber **notificações por correio eletrónico** do SendGrid, aceite o valor predefinido de **Sim** e forneça um endereço de correio eletrónico válido. Se selecionar **não** mas decidir numa data posterior que pretende receber notificações por e-mail, pode atualizar o **External_EmailToAddress** variável com endereços de correio eletrónico válidos separados por vírgulas e, em seguida, modificar a variável **External_IsSendEmail** com o valor **Sim**.  

9. Depois de ter configurado as definições iniciais necessárias para a solução, selecione **criar**.  Depois de todas as definições serem validadas, a solução for implementada para a sua subscrição.  Este processo pode demorar vários segundos a concluir e pode controlar o progresso em **notificações** no menu. 

## <a name="collection-frequency"></a>Frequência da recolha

Automatização registo e a tarefa de fluxo dados da tarefa é ingeridos para o repositório de análise de registos a cada 5 minutos.  

## <a name="using-the-solution"></a>Utilizar a solução

Quando adiciona a solução de gestão de VM, o **StartStopVMView** mosaico é adicionado ao seu dashboard na sua área de trabalho de análise de registos do portal do Azure.  Este mosaico mostra uma contagem e uma representação gráfica das tarefas de runbook para a solução que foi iniciada e concluída com êxito.<br><br> ![Mosaico Vista StartStopVM da Gestão de VMs](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

Na sua conta de automatização, pode aceder e gerir a solução, selecionando o **área de trabalho** opção. Na página de análise de registos, selecione **soluções** no painel esquerdo.  No **soluções** página, selecione a solução **Start-Stop-VM [área de trabalho]** da lista.<br><br> ![Lista de soluções na análise de registos](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

A seleção de solução apresenta o **Start-Stop-VM [área de trabalho]** painel de solução. Aqui pode rever detalhes importantes, tais como o **StartStopVM** mosaico. Como a sua área de trabalho de análise de registos, este mosaico mostra uma contagem e uma representação gráfica das tarefas de runbook para a solução de iniciar e terminar com êxito.<br><br> ![Página de solução de gestão de atualizações de automatização](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Aqui, pode efetuar mais análise de registos de tarefa clicando no mosaico de anel. O dashboard de solução mostra o histórico de tarefas e previamente definido consultas de pesquisa de registo. Mudar para o portal de registo avançadas de análise para procurar com base nas suas consultas de pesquisa.  

Todos os runbooks principais incluem o *WhatIf* parâmetro. Quando definido como **verdadeiro**, este suporta com detalhes sobre o comportamento exato runbook executa quando executar sem o *WhatIf* parâmetro e valida que estão a ser direcionadas as VMs corretas. Os Runbooks só efetuar as ações definidas quando o *WhatIf* parâmetro está definido como **falso**.  


### <a name="scenario-1-daily-startstop-vms-across-a-subscription-or-target-resource-groups"></a>Cenário 1: Diariamente iniciar/parar VMs através de uma subscrição ou grupos de recursos de destino 

Esta é a configuração predefinida quando implementar a solução pela primeira vez.  Por exemplo, pode configurá-la para parar todas as VMs através de uma subscrição quando deixe o trabalho no evening e iniciá-los da manhã quando estiver novamente no escritório. Quando configurar as agendas **agendada-StartVM** e **agendada StopVM** durante a implementação, iniciar e parar a VMs de destino.
>[!NOTE]
>O fuso horário é o seu fuso horário atual quando configura o parâmetro da hora de agendamento. No entanto, é armazenada em formato UTC na automatização do Azure.  Não é necessário efetuar qualquer conversão do fuso horário, como isto é processado durante a implementação.

Pode controlar que as VMs estão no âmbito ao configurar as seguintes variáveis: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, e **External_ ExcludeVMNames**.  

>[!NOTE]
> O valor para **nomes ResourceGroup de destino** são armazenadas como o valor para ambos **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**. Para obter mais granularidade, pode modificar cada uma destas variáveis para visar grupos de recursos diferentes.  Para a ação de iniciação, utilize **External_Start_ResourceGroupNames**e para a ação de parar, utilize **External_Stop_ResourceGroupNames**. VMs são adicionadas automaticamente no início e parar agendas.

Para testar e validar a configuração, inicie manualmente o **ScheduledStartStop_Parent** runbook e defina o parâmetro de ação **iniciar** ou **parar** e o WHATIF o parâmetro para **verdadeiro**.<br><br> ![Configurar os parâmetros do runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)


 A ação agendada de pré-visualização e efetue as alterações necessárias antes de implementar em produção VMs.  Pode executar manualmente o runbook com o parâmetro definido como **falso**, ou pode permitir que a agenda de automatização **agenda-StartVM** e **agenda StopVM** executar a agenda prescrita automaticamente os seguintes.

### <a name="scenario-2-sequence-the-startstop-vms-across-a-subscription-by-using-tags"></a>Cenário 2: A iniciar/parar VMs através de uma subscrição de sequência utilizando etiquetas
Num ambiente que inclua dois ou mais componentes em várias VMs suportar uma carga de trabalho distribuída, é importante que suportam a sequência em que os componentes são iniciados e parados por ordem.  Para tal, efetuando os seguintes passos:

1. Adicionar um **SequenceStart** e um **SequenceStop** tag com um valor de número inteiro positivo e VMs que são direcionadas na **External_Start_ResourceGroupNames** e  **External_Stop_ResourceGroupNames** variáveis.  As ações de início e paragem são efetuadas por ordem ascendente.  Para saber como marcar uma VM, consulte [marcar a Máquina Virtual do Windows no Azure](../virtual-machines/windows/tag.md) e [Tag uma Máquina Virtual Linux no Azure](../virtual-machines/linux/tag.md).
2. Modifique as agendas **Sequenced-StartVM** e **Sequenced StopVM** para a data e hora que satisfazem os requisitos e ativar o agendamento.  

Para testar e validar a configuração, inicie manualmente o **SequencedStartStop_Parent** runbook. Defina o parâmetro de ação **iniciar** ou **parar** e o parâmetro WHATIF para **verdadeiro**.<br><br> ![Configurar os parâmetros do runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> A ação de pré-visualização e efetue as alterações necessárias antes de implementar em produção VMs.  Quando preparada, manualmente executar o runbook com o parâmetro definido como **falso**, ou permitir que a agenda de automatização **Sequenced-StartVM** e **Sequenced StopVM** executar a agenda prescrita automaticamente os seguintes.  

### <a name="scenario-3-automate-startstop-vms-across-a-subscription-based-on-cpu-utilization"></a>Cenário 3: Automatizar iniciar/parar VMs através de uma subscrição com base na utilização da CPU
Esta solução pode ajudá-lo a gerir os custos de máquinas virtuais em execução na sua subscrição ao avaliar as VMs do Azure que não são utilizados durante períodos de pico, tal como após horas, e automaticamente encerrar-se a utilização do processador é less than x %.  

Por predefinição, a solução está pré-configurada para avaliar a métrica de CPU de percentagem para ver se a utilização média é 5 porcento ou menos.  Este é controlado pelas seguintes variáveis e pode ser modificada se os valores predefinidos não cumprem os requisitos:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Pode ativar a filtragem a ação contra uma subscrição e o grupo de recursos ou que seja direcionada para uma lista específica de VMs, mas não ambos.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>A ação de paragem em relação a um grupo de recursos e de subscrição de destino

1. Configurar o **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames** variáveis para especificar o destino VMs.  
2. Ativar e atualizar o **Schedule_AutoStop_CreateAlert_Parent** agenda.
3. Execute o **AutoStop_CreateAlert_Parent** runbook com o parâmetro de ação definido como **iniciar** e o parâmetro WHATIF definido como **verdadeiro** para pré-visualizar as suas alterações.

#### <a name="target-the-stop-action-by-vm-list"></a>A ação de paragem de destino pela lista VM

1. Execute o **AutoStop_CreateAlert_Parent** runbook com o parâmetro de ação definido como **iniciar**, adicione uma lista separada por vírgulas de VMs no *VMList* parâmetro e, em seguida, defina o O parâmetro WHATIF para **verdadeiro**. Pré-visualize as suas alterações.  
2. Configurar o **External_ExcludeVMNames** parâmetro com uma lista separada por vírgulas de VMs (VM1, VM2, VM3).
3. Este cenário não honrar o **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupnames** variáveis.  Para este cenário, terá de criar a sua própria agenda de automatização. Para obter mais informações, consulte [agendar um runbook na automatização do Azure](../automation/automation-schedules.md).

Agora que tem uma agenda para parar as VMs com base na utilização de CPU, tem de ativar um dos seguintes agendamentos iniciá-las.

* Ação de iniciação de destino através da subscrição e grupo de recursos.  Consulte os passos em [cenário 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) para teste e ativação **agendada-StartVM** agendas.
* Destino iniciar ação por subscrição, o grupo de recursos e etiqueta.  Consulte os passos em [cenário 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) para teste e ativação **Sequenced-StartVM** agendas.


### <a name="configuring-email-notifications"></a>Configurar notificações por e-mail

Para configurar notificações por e-mail depois de implementar a solução, modifique as seguintes três variáveis:

* External_EmailFromAddress: Especifique o endereço de correio eletrónico do remetente.
* External_EmailToAddress: Especifique uma lista separada por vírgulas de endereços de e-mail (user@hotmail.com, user@outlook.com) para receber notificações por e-mail.
* External_IsSendEmail: Definido como **Sim** para receber e-mails. Para desativar notificações por e-mail, defina o valor como **não**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Modificar as agendas de arranque e encerramento

Gerir as agendas de arranque e encerramento nesta solução segue os mesmos passos conforme descritos em [agendar um runbook na automatização do Azure](automation-schedules.md).     

## <a name="log-analytics-records"></a>Registos do Log Analytics

Automatização cria dois tipos de registos no repositório de OMS: registos de tarefa e os fluxos da tarefa.

### <a name="job-logs"></a>Registos de trabalhos

Propriedade | Descrição|
----------|----------|
Autor da chamada |  Quem iniciou a operação.  Os valores possíveis são um endereço de e-mail ou o sistema para trabalhos agendados.|
Categoria | Classificação do tipo de dados.  Para a Automatização, o valor é JobLogs.|
CorrelationId | GUID que é o Id de Correlação do trabalho do runbook.|
JobId | GUID que é o Id do trabalho do runbook.|
operationName | Especifica o tipo de operação efetuada no Azure.  Para automatização, o valor é a tarefa.|
resourceId | Especifica o tipo de recurso no Azure.  Para a Automatização, o valor é a conta de Automatização associada ao runbook.|
ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
ResourceProvider | Especifica o serviço do Azure que fornece os recursos que pode implementar e gerir.  Para a Automatização, o valor é Automatização do Azure.|
ResourceType | Especifica o tipo de recurso no Azure.  Para a Automatização, o valor é a conta de Automatização associada ao runbook.|
resultType | O estado do trabalho do runbook.  Os valores possíveis são:<br>- Iniciado<br>- Parado<br>- Suspenso<br>- Falhado<br>- Bem-sucedido|
resultDescription | Descreve o estado do resultado do trabalho do runbook.  Os valores possíveis são:<br>- Trabalho iniciado<br>- Trabalho falhado<br>- Trabalho Concluído|
RunbookName | Especifica o nome do runbook.|
SourceSystem | Especifica o sistema de origem dos dados submetidos.  Para automatização, o valor é OpsManager|
StreamType | Especifica o tipo de evento. Os valores possíveis são:<br>- Verboso<br>- Saída<br>- Erro<br>- Aviso|
SubscriptionId | Especifica o ID de subscrição do trabalho.
Hora | Data e hora da execução do trabalho do runbook.|


### <a name="job-streams"></a>Fluxos de trabalho

Propriedade | Descrição|
----------|----------|
Autor da chamada |  Quem iniciou a operação.  Os valores possíveis são um endereço de e-mail ou o sistema para trabalhos agendados.|
Categoria | Classificação do tipo de dados.  Para a Automatização, o valor é JobStreams.|
JobId | GUID que é o Id do trabalho do runbook.|
operationName | Especifica o tipo de operação efetuada no Azure.  Para automatização, o valor é a tarefa.|
ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
resourceId | Especifica o Id de recurso no Azure.  Para a Automatização, o valor é a conta de Automatização associada ao runbook.|
ResourceProvider | Especifica o serviço do Azure que fornece os recursos que pode implementar e gerir.  Para a Automatização, o valor é Automatização do Azure.|
ResourceType | Especifica o tipo de recurso no Azure.  Para a Automatização, o valor é a conta de Automatização associada ao runbook.|
resultType | O resultado do trabalho do runbook no momento em que o evento foi gerado. Um valor possível é:<br>- InProgress|
resultDescription | Inclui o fluxo de saída do runbook.|
RunbookName | O nome do runbook.|
SourceSystem | Especifica o sistema de origem dos dados submetidos.  Para automatização, o valor é OpsManager.|
StreamType | O tipo de fluxo de trabalho. Os valores possíveis são:<br>-Progresso<br>- Saída<br>- Aviso<br>- Erro<br>- Depuração<br>- Verboso|
Hora | Data e hora da execução do trabalho do runbook.|

Quando efetuar qualquer pesquisa de registo que devolve os registos de categoria do **JobLogs** ou **JobStreams**, pode selecionar o **JobLogs** ou **JobStreams**vista, que apresenta um conjunto de mosaicos resumir as atualizações devolvidas pela pesquisa.

## <a name="sample-log-searches"></a>Pesquisas de registo de exemplo

A tabela seguinte disponibiliza pesquisas de registos de exemplo para registos de trabalhos que esta solução recolhe. 

Consulta | Descrição|
----------|----------|
Localizar a tarefa de runbook ScheduledStartStop_Parent tiver concluído com êxito | Procurar categoria = = "JobLogs" &#124; onde (RunbookName_s = = "ScheduledStartStop_Parent") &#124; onde (ResultType = = "Concluída") &#124; resumir AggregatedValue = existente pelo ResultType, bin (TimeGenerated, 1h) &#124; Ordenar por TimeGenerated desc|
Localizar a tarefa de runbook SequencedStartStop_Parent tiver concluído com êxito | Procurar categoria = = "JobLogs" &#124; onde (RunbookName_s = = "SequencedStartStop_Parent") &#124; onde (ResultType = = "Concluída") &#124; resumir AggregatedValue = existente pelo ResultType, bin (TimeGenerated, 1h) &#124; Ordenar por TimeGenerated desc

## <a name="removing-the-solution"></a>Remover a solução

Se decidir que deixar de precisar utilizar a solução, pode eliminá-la a conta de automatização.  Apenas a eliminar a solução remove os runbooks. Não elimina o agendas ou variáveis que foram criadas quando a solução foi adicionada.  Esses recursos, que tem de eliminar manualmente se não estiver a utilizá-los com outros runbooks.  

Para eliminar a solução, execute os seguintes passos:

1.  A partir da sua conta de automatização, selecione **área de trabalho** no painel esquerdo.  
2.  No **soluções** página, selecione a solução **Start-Stop-VM [área de trabalho]**.  No **VMManagementSolution [área de trabalho]** página, no menu, selecione **eliminar**.<br><br> ![Eliminar a solução de gestão VM](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  No **Eliminar solução** janela, confirme que pretende eliminar a solução.
4.  Verificar as informações e a solução é eliminada, pode controlar o progresso em **notificações** no menu.  É encaminhado para o **soluções** página depois do processo para remover a solução é iniciado.  

A conta de automatização e a área de trabalho de análise de registos não são eliminados como parte deste processo.  Se não pretender manter a área de trabalho de análise de registos, terá de eliminar manualmente.  Isto pode ser conseguido do portal do Azure:

1.    Partir do ecrã principal portal do Azure, selecione **Log Analytics**.
2. No **Log Analytics** painel, selecione a área de trabalho.
3. Selecione **eliminar** no menu no painel de definições da área de trabalho.  
      
## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre como construir consultas de pesquisa diferentes e reveja os registos da tarefa de automatização com a análise de registos, consulte o artigo [pesquisas de registo na análise de registos](../log-analytics/log-analytics-log-searches.md).
- Para saber mais sobre a execução de runbooks, como monitorizar tarefas de runbooks e outros detalhes técnicos, veja [Acompanhar uma tarefa de runbook](automation-runbook-execution.md).
- Para saber mais sobre a análise de registos e as origens de recolha de dados, consulte [dados de armazenamento do Azure recolha na descrição geral da análise de registos](../log-analytics/log-analytics-azure-storage.md).






   

