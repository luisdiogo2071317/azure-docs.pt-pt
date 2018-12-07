---
title: Resolver problemas de erros com Runbooks de automatização do Azure
description: Saiba como resolver problemas com os runbooks de automatização do Azure
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 41eb31ecabb20ec9eec3db13d5eda9f9cfbe6c69
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015471"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Resolver problemas de erros com runbooks

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Erros de autenticação ao trabalhar com runbooks de automatização do Azure

### <a name="sign-in-failed"></a>Cenário: Inicie sessão na conta do Azure falhou

#### <a name="issue"></a>Problema

Receber o erro seguinte ao trabalhar com o `Add-AzureAccount` ou `Connect-AzureRmAccount` cmdlets.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Causa

Este erro ocorre se o nome de recurso de credencial não é válido ou se o nome de utilizador e palavra-passe que utilizou para configurar o recurso de credencial de automatização não são válidas.

#### <a name="resolution"></a>Resolução

Para determinar qual é o problema, siga os passos seguintes:  

1. Certifique-se de que não tem carateres especiais, incluindo o **@** caráter no nome do elemento de credencial de automatização que está a utilizar para ligar ao Azure.  
2. Verifique o que pode usar o nome de utilizador e palavra-passe que são armazenados na credencial da automatização do Azure no seu editor do ISE do PowerShell local. Pode fazê-Verifique o nome de utilizador e palavra-passe estão corretos, executando os seguintes cmdlets no ISE do PowerShell:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Se a autenticação falhar localmente, significa que não configurou as suas credenciais do Azure Active Directory corretamente. Consulte a [a autenticação no Azure com o Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) mensagem de blogue para obter a conta do Azure Active Directory configurada corretamente.  

4. Se ele se parece com um erro transitório, tente adicionar a lógica de repetição para sua rotina de autenticação para fazer a autenticação mais robusto.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
   $LogonAttempt++
   # Logging in to Azure...
   $connectionResult = Connect-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $servicePrincipalConnection.TenantId `
      -ApplicationId $servicePrincipalConnection.ApplicationId `
      -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

   Start-Sleep -Seconds 30
   }
   ```

### <a name="unable-to-find-subscription"></a>Cenário: Não é possível encontrar a subscrição do Azure

#### <a name="issue"></a>Problema

Receber o erro seguinte ao trabalhar com o `Select-AzureSubscription` ou `Select-AzureRmSubscription` cmdlets:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Erro

Este erro ocorre se o nome da subscrição não é válido ou se o utilizador do Azure Active Directory que está a tentar obter os detalhes da subscrição não está configurado como um administrador da subscrição.

#### <a name="resolution"></a>Resolução

Para determinar se já autenticados apropriadamente para o Azure e ter acesso à subscrição que está a tentar selecionar, siga os passos seguintes:  

1. Teste o seu script fora da automatização do Azure para garantir que funciona autónomo.
2. Certifique-se de que executa o **Add-AzureAccount** cmdlet antes de executar o **Select-AzureSubscription** cmdlet.  
3. Se continuar a ver esta mensagem de erro, modifique o código, adicionando a **- AzureRmContext** seguinte parâmetro a **Add-AzureAccount** cmdlet e, em seguida, executar o código.

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Cenário: Autenticação no Azure falhou porque a autenticação multifator está ativada

#### <a name="issue"></a>Problema

Receber o erro seguinte durante a autenticação do Azure com o seu nome de utilizador do Azure e a palavra-passe:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Causa

Se tiver de autenticação multifator na sua conta do Azure, não é possível utilizar um utilizador do Azure Active Directory para autenticar para o Azure. Em vez disso, tem de utilizar um certificado ou um principal de serviço para autenticar para o Azure.

#### <a name="resolution"></a>Resolução

Para utilizar um certificado com os cmdlets do modelo de implementação clássica do Azure, veja [criação e adição de um certificado para gerir serviços do Azure.](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Para utilizar um principal de serviço com os cmdlets do Azure Resource Manager, veja [criar através do portal do Azure principal de serviço](../../active-directory/develop/howto-create-service-principal-portal.md) e [autenticar um principal de serviço com o Azure Resource Manager.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="common-errors-when-working-with-runbooks"></a>Erros comuns ao trabalhar com runbooks

### <a name="task-was-cancelled"></a>Cenário: O runbook falhar com o erro: uma tarefa foi cancelada

#### <a name="issue"></a>Problema

O runbook falhar com um erro semelhante ao seguinte exemplo:

```
Exception: A task was canceled.
```

#### <a name="cause"></a>Causa

Este erro pode dever-se através da utilização de módulos do Azure desatualizados.

#### <a name="resolution"></a>Resolução

Este erro pode ser resolvido através da atualização de seus módulos do Azure para a versão mais recente.

Na sua conta de automatização, clique em **módulos**e clique em **módulos do Azure de atualização**. A atualização demora aproximadamente 15 minutos, uma vez concluídos volte a executar o runbook que era a falhar. Para saber mais sobre a atualização de seus módulos, consulte [módulos do Azure de atualização na automatização do Azure](../automation-update-azure-modules.md).

### <a name="child-runbook-auth-failure"></a>Cenário: O runbook subordinado Falha ao lidar com várias subscrições

#### <a name="issue"></a>Problema

Ao executar runbooks subordinados com `Start-AzureRmRunbook`, o runbook subordinado não consegue gerir recursos do Azure.

#### <a name="cause"></a>Causa

O runbook subordinado não está a utilizar o contexto correto quando em execução.

#### <a name="resolution"></a>Resolução

Ao trabalhar com várias subscrições o contexto da subscrição poderão perder-se ao invocar runbooks subordinados. Para garantir que o contexto da subscrição é transferido para os runbooks subordinados, adicione o `AzureRmContext` parâmetro para o cmdlet e passe o contexto para o mesmo.

```azurepowershell-interactive
# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

### <a name="not-recognized-as-cmdlet"></a>Cenário: O runbook falhar devido a um cmdlet em falta

#### <a name="issue"></a>Problema

O runbook falhar com um erro semelhante ao seguinte exemplo:

```
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. O módulo que contém o cmdlet não é importado para a conta de automatização
2. O módulo que contém o cmdlet é importado, mas está desatualizado

#### <a name="resolution"></a>Resolução

Este erro pode ser resolvido, efetuando uma das seguintes tarefas:

Se o módulo é um módulo do Azure, veja [como atualizar módulos do Azure PowerShell na automatização do Azure](../automation-update-azure-modules.md) para saber como atualizar os módulos na conta de automatização.

Se for um módulo separado, certifique-se o módulo no importou na sua conta de automatização.

### <a name="job-attempted-3-times"></a>Cenário: O início de tarefa de runbook foi tentado a três vezes, mas ocorreu uma falha para cada hora de início

#### <a name="issue"></a>Problema

O runbook falhar com o erro:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. Limite de memória. Existem limites indicados na quantidade de memória atribuída a uma área de segurança [limites de serviço de automatização](../../azure-subscription-service-limits.md#automation-limits) para que uma tarefa o poderá falhar se estiver a utilizar mais de 400 MB de memória.

1. Soquetes de rede. Áreas de segurança do Azure estão limitadas a soquetes de rede em simultâneo de 1000, conforme descrito em [limites de serviço de automatização](../../azure-subscription-service-limits.md#automation-limits).

1. Módulo incompatível. Este erro pode ocorrer se as dependências do módulo não estão corretas e se não forem, o runbook devolve normalmente um "comando não encontrado" ou "Não é possível vincular o parâmetro" mensagem.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções resolver o problema:

* Os métodos sugeridos para funcionar dentro do limite de memória são para dividir a carga de trabalho entre vários runbooks, não processar a quantidade de dados na memória, não para escrever a saída desnecessária de seus runbooks ou considere quantos pontos de verificação escrever em seu fluxo de trabalho do PowerShell runbooks. Pode utilizar o método clear, tal como `$myVar.clear()` para limpar a variável e utilize `[GC]::Collect()` para executar a coleta de lixo imediatamente, isto irá reduzir a superfície de memória do seu runbook durante o tempo de execução.

* Atualizar os módulos do Azure ao seguir os passos [como atualizar módulos do Azure PowerShell na automatização do Azure](../automation-update-azure-modules.md).  

* Outra solução é executar o runbook num [Runbook Worker híbrido](../automation-hrw-run-runbooks.md). Funções de trabalho híbridas não estão limitadas pelos limites de memória e rede que são áreas de segurança do Azure.

### <a name="fails-deserialized-object"></a>Cenário: O Runbook falhar devido ao objeto de serialização anulado

#### <a name="issue"></a>Problema

O runbook falhar com o erro:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Causa

Se o runbook for um fluxo de trabalho do PowerShell, ele armazena objetos complexos num formato de serialização anulado para manter o estado do runbook se o fluxo de trabalho é suspenso.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes três soluções corrigir este problema:

1. Se está encaminhando objetos complexos, de um cmdlet para outro, encapsule esses cmdlets num InlineScript.
2. Passe o nome ou o valor que precisa do objeto complexo em vez de transmitir o objeto inteiro.
3. Utilize um runbook do PowerShell em vez de um runbook de fluxo de trabalho do PowerShell.

### <a name="quota-exceeded"></a>Cenário: A tarefa do Runbook falhou porque excedeu a quota alocada

#### <a name="issue"></a>Problema

A tarefa de runbook falha com o erro:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Causa

Este erro ocorre quando a execução da tarefa excede a quota gratuita de 500 minutos para a sua conta. Esta quota aplica-se a todos os tipos de tarefas de execução de tarefa como uma tarefa de teste, a partir de uma tarefa no portal, executar uma tarefa através de webhooks e agendar uma tarefa para executar com o portal do Azure ou no seu datacenter. Para saber mais sobre os preços para a automatização, veja [os preços de automatização](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Resolução

Se pretender utilizar mais de 500 minutos de processamento por mês, terá de alterar a sua subscrição do gratuito camada para o escalão básico. Pode atualizar para o escalão básico, efetuando os seguintes passos:  

1. Inicie sessão na sua subscrição do Azure  
2. Selecione a conta de Automatização que quer atualizar  
3. Clique em **configurações** > **preços**.
4. Clique em **habilitar** na parte inferior da página para atualizar a conta para o **básica** escalão.

### <a name="cmdlet-not-recognized"></a>Cenário: Cmdlet não reconhecido ao executar um runbook

#### <a name="issue"></a>Problema

A tarefa de runbook falha com o erro:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Causa

Este erro é causado quando o motor do PowerShell não é possível encontrar o cmdlet que está a utilizar no runbook. Isto pode ser porque o módulo que contém o cmdlet está em falta da conta, existe um conflito de nome com um nome de runbook, ou o cmdlet também existe outro módulo e automatização não é possível resolver o nome.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções resolver o problema:  

* Verifique se introduziu o nome do cmdlet corretamente.  
* Certifique-se de que o cmdlet existe na conta de automatização e que não existam conflitos. Para verificar se o cmdlet está presente, abra um runbook no modo de edição e de pesquisa para o cmdlet que pretende localizar na biblioteca ou executar `Get-Command <CommandName>`. Depois de ter confirmado que o cmdlet está disponível para a conta, e que não há nenhum nome entra em conflito com outros cmdlets ou runbooks, adicioná-lo para a tela e certifique-se de que está a utilizar um parâmetro válido definido no runbook.  
* Se tiver um conflito de nomes e o cmdlet está disponível em dois módulos diferentes, pode resolver este problema ao utilizar o nome completamente qualificado para o cmdlet. Por exemplo, pode usar **ModuleName\CmdletName**.  
* Se estiver a executar o runbook no local num grupo de trabalho híbrida, em seguida, certifique-se de que o módulo e o cmdlet está instalado no computador que aloja a função de trabalho híbrida.

### <a name="long-running-runbook"></a>Cenário: Um runbook de longa execução não consegue concluir

#### <a name="issue"></a>Problema

O runbook mostra num **parado** Estado após a execução durante 3 horas. Também poderá receber o erro:

```
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Este comportamento é propositado em áreas de segurança do Azure devido a "Justa" monitorização de processos dentro da automatização do Azure, que automaticamente para um runbook, se for executado durante mais de três horas. O estado de um runbook que vai passado o limite de tempo de partilha de fair difere por tipo de runbook. Runbooks do PowerShell e Python estão definidas para uma **parado** estado. Runbooks de fluxo de trabalho do PowerShell estão definidas como **falhada**.

#### <a name="cause"></a>Causa

O runbook foi executado sobre o limite de duração de 3 horas permitido pelo justa numa área de segurança do Azure.

#### <a name="resolution"></a>Resolução

Um recomendado a solução é executar o runbook num [Runbook Worker híbrido](../automation-hrw-run-runbooks.md).

Funções de trabalho híbridas não estão limitadas pela [justa](../automation-runbook-execution.md#fair-share) limite de runbook de duração de 3 horas que são áreas de segurança do Azure. Embora os Runbook Workers híbridos não estão limitados pelo limite de cota razoável de duração de 3 horas, os runbooks foi executado nos Runbook Workers híbridos ainda deve ser desenvolvidos para oferecer suporte a comportamentos de reinício em caso de problemas de infraestrutura local inesperado.

Outra opção é otimizar o runbook criando [runbooks subordinados](../automation-child-runbooks.md). Se o seu runbook percorre a mesma função num número de recursos, como uma operação de base de dados em várias bases de dados, pode mover essa função para um runbook subordinado. Cada um destes runbooks subordinados é executado em paralelo, em processos separados, diminuindo a quantidade total de tempo do runbook principal para conclusão.

Os cmdlets do PowerShell que permitem o cenário de runbook subordinado são:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) -este cmdlet permite-lhe iniciar um runbook e passar parâmetros para o runbook

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) -este cmdlet permite-lhe verificar o estado da tarefa para cada filho, se existem operações que precisam ser executadas após a conclusão do runbook subordinado.

### <a name="expired webhook"></a>Cenário: Estado: 400 pedido inválido ao invocar um webhook

#### <a name="issue"></a>Problema

Quando tentar invocar um webhook para um runbook de automatização do Azure receberá o seguinte erro.

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Causa

O webhook que está a tentar invocar está desabilitado ou expirou.

#### <a name="resolution"></a>Resolução

Se o webhook está desabilitado, pode reativar o webhook através do portal do Azure. Se o webhook expirou, o webhook tem de ser eliminado e recriado. Só é possível [renovar um webhook](../automation-webhooks.md#renew-webhook) se não tiver já expirado.

### <a name="429"></a>Cenário: 429: A taxa de pedidos atualmente é demasiado grande. Tente novamente

#### <a name="issue"></a>Problema

A receber a seguinte mensagem de erro ao executar o `Get-AzureRmAutomationJobOutput` cmdlet:

```
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Causa

Este erro pode ocorrer ao obter o resultado da tarefa a partir de um runbook que tem muitas [fluxos verbosos](../automation-runbook-output-and-messages.md#verbose-stream).

#### <a name="resolution"></a>Resolução

Existem duas formas de resolver este erro:

* Editar o runbook e reduzir o número de fluxos de trabalho que emite.
* Reduza o número de fluxos para ser recuperado quando executar o cmdlet. Para tal, pode especificar a `-Stream Output` parâmetro para o `Get-AzureRmAutomationJobOutput` fluxos de saída do cmdlet para obter apenas. 

## <a name="common-errors-when-importing-modules"></a>Erros comuns ao importar módulos

### <a name="module-fails-to-import"></a>Cenário: Ocorre uma falha do módulo importar ou cmdlets não pode ser executados depois de importar

#### <a name="issue"></a>Problema

Um módulo não consegue importar ou importa com êxito, mas não existem cmdlets são extraídos.

#### <a name="cause"></a>Causa

Algumas razões comuns que um módulo não pode importar com êxito para a automatização do Azure são:

* A estrutura não corresponde a estrutura que precisa de automatização que seja.
* O módulo é dependente de outro módulo que não tenha sido implantado para sua conta de automatização.
* O módulo está em falta as respetivas dependências na pasta.
* O `New-AzureRmAutomationModule` cmdlet está a ser utilizado para carregar o módulo e have't dado o caminho de armazenamento completo ou não tiver carregado o módulo utilizando um URL acessível publicamente.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções resolver o problema:

* Certifique-se de que o módulo segue o seguinte formato: ModuleName.Zip **->** ModuleName ou um número de versão **->** (ModuleName.psm1, ModuleName.psd1)
* Abra o ficheiro. psd1 e ver se o módulo tiver dependências. Se assim for, carregar esses módulos para a conta de automatização.
* Certifique-se de que quaisquer DLLs referenciado estão presentes na pasta do módulo.

## <a name="next-steps"></a>Passos Seguintes

Se não vir o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.