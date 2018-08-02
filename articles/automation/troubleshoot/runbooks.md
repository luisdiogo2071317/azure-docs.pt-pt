---
title: Resolver problemas de erros com Runbooks de automatização do Azure
description: Saiba como resolver problemas com os runbooks de automatização do Azure
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 07/13/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 48b2aab9d2a3937fb53a2e63efa26efc18a894f8
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413863"
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

Este erro ocorre se o nome de recurso de credencial não é válido ou se o nome de utilizador e palavra-passe que utilizou para configurar o recurso de credencial de automatização não são válidos.

#### <a name="resolution"></a>Resolução

Para determinar qual é o problema, siga os passos seguintes:  

1. Certifique-se de que não tem carateres especiais, incluindo o **@** caráter no nome do elemento de credencial de automatização que está a utilizar para ligar ao Azure.  
2. Verifique o que pode usar o nome de utilizador e palavra-passe que são armazenados na credencial da automatização do Azure no seu editor do ISE do PowerShell local. Pode fazê-lo ao executar os seguintes cmdlets no ISE do PowerShell:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Se a autenticação falhar localmente, isso significa que não configurou as suas credenciais do Azure Active Directory corretamente. Consulte a [a autenticação no Azure com o Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) mensagem de blogue para obter a conta do Azure Active Directory configurada corretamente.  

### <a name="unable-to-find-subscription"></a>Cenário: Não é possível encontrar a subscrição do Azure

#### <a name="issue"></a>Problema

Receber o erro seguinte ao trabalhar com o `Select-AzureSubscription` ou `Select-AzureRmSubscription` cmdlets.:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Erro

Este erro ocorre se o nome da subscrição não é válido ou se o utilizador do Azure Active Directory que está a tentar obter os detalhes da subscrição não está configurado como um administrador da subscrição.

#### <a name="resolution"></a>Resolução

Para determinar se corretamente autenticado para o Azure e ter acesso à subscrição que está a tentar selecionar, siga os passos seguintes:  

1. Certifique-se de que executa o **Add-AzureAccount** antes de executar o **Select-AzureSubscription** cmdlet.  
2. Se continuar a ver esta mensagem de erro, modifique o código, adicionando a **Get-AzureSubscription** seguinte cmdlet a **Add-AzureAccount** cmdlet e, em seguida, executar o código. Agora, verifique se a saída de Get-AzureSubscription contém os detalhes da sua subscrição.  

   * Se não vir quaisquer detalhes da subscrição na saída, isso significa que a subscrição não está inicializada ainda.  
   * Se ver os detalhes da subscrição na saída, confirme que está a utilizar o nome da subscrição correta ou o ID com o **Select-AzureSubscription** cmdlet.

### <a name="auth-failed-mfa"></a>Cenário: Autenticação no Azure falhou porque a autenticação multifator está ativada

#### <a name="issue"></a>Problema

Receber o erro seguinte durante a autenticação do Azure com o seu nome de utilizador do Azure e a palavra-passe:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Causa

Se tiver de autenticação multifator na sua conta do Azure, não é possível utilizar um utilizador do Azure Active Directory para autenticar para o Azure. Em vez disso, tem de utilizar um certificado ou um principal de serviço para autenticar para o Azure.

#### <a name="resolution"></a>Resolução

Para utilizar um certificado com os cmdlets do modelo de implementação clássica do Azure, veja [criação e adição de um certificado para gerir serviços do Azure.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Para utilizar um principal de serviço com os cmdlets do Azure Resource Manager, veja [criar através do portal do Azure principal de serviço](../../azure-resource-manager/resource-group-create-service-principal-portal.md) e [autenticar um principal de serviço com o Azure Resource Manager.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

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

Na sua conta de automatização, clique em **módulos**e clique em **módulos do Azure de atualização**. A atualização demora aproximadamente 15 minutos, uma vez concluídos volte a executar o runbook que era a falhar.

### <a name="not-recognized-as-cmdlet"></a>Cenário: O runbook falhar devido a um cmdlet em falta

#### <a name="issue"></a>Problema

O runbook falhar com um erro semelhante ao seguinte exemplo:

```
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. O módulo que contém o cmdlet não é importado para a conta de automatização
2. O módulo containg o cmdlet é importado, mas está desatualizada

#### <a name="resolution"></a>Resolução

Este erro pode ser resolvido, efetuando uma das seguintes tarefas:

Se o módulo é um módulo do Azure, veja [como atualizar módulos do Azure PowerShell na automatização do Azure](../automation-update-azure-modules.md) para saber como atualizar os módulos na conta de automatização.

Se for um módulo separado, certifique-se o módulo no importados na conta de automatização.

### <a name="job-attempted-3-times"></a>Cenário: O início de tarefa de runbook foi tentado a três vezes, mas ocorreu uma falha para cada hora de início

#### <a name="issue"></a>Problema

O runbook falhar com o erro:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. Limite de memória. Existem limites indicados na quantidade de memória atribuída a uma área de segurança [limites de serviço de automatização](../../azure-subscription-service-limits.md#automation-limits) para que uma tarefa o poderá falhar se estiver a utilizar mais de 400 MB de memória.

2. Módulo incompatível. Isto pode ocorrer se as dependências do módulo não estão corretas e se não forem, o runbook devolve normalmente um "comando não encontrado" ou "Não é possível vincular o parâmetro" mensagem.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções resolver o problema:

* Os métodos sugeridos para funcionar dentro do limite de memória são para dividir a carga de trabalho entre vários runbooks, não processar a quantidade de dados na memória, não para escrever a saída desnecessária de seus runbooks ou considere quantos pontos de verificação escrever em seu fluxo de trabalho do PowerShell runbooks.  

* Atualizar os módulos do Azure ao seguir os passos [como atualizar módulos do Azure PowerShell na automatização do Azure](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Cenário: O Runbook falhar devido ao objeto de serialização anulado

#### <a name="issue"></a>Problema

O runbook falhar com o erro:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Causa

Se o runbook for um fluxo de trabalho do PowerShell, ele armazena objetos complexos num formato de serialização anulado para manter o seu estado de runbook se o fluxo de trabalho é suspenso.

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

* Verifique o nome do cmdlet tenha introduzido corretamente.  
* Certifique-se de que o cmdlet existe na conta de automatização e que não existam conflitos. Para verificar se o cmdlet está presente, abra um runbook no modo de edição e de pesquisa para o cmdlet que pretende localizar na biblioteca ou executar `Get-Command <CommandName>`. Depois de validar que o cmdlet está disponível para a conta, e que não há nenhum nome entra em conflito com outros cmdlets ou runbooks, adicioná-lo para a tela e certifique-se de que está a utilizar um parâmetro válido definido no runbook.  
* Se tiver um conflito de nomes e o cmdlet está disponível em dois módulos diferentes, pode resolver este problema ao utilizar o nome completamente qualificado para o cmdlet. Por exemplo, pode usar **ModuleName\CmdletName**.  
* Se está a executar o runbook no local num grupo de trabalho híbrida, em seguida, certifique-se de que o módulo/cmdlet está instalado no computador que aloja a função de trabalho híbrida.

### <a name="evicted-from-checkpoint"></a>Cenário: Um runbook de longa execução consistentemente falhar com a exceção: "a tarefa não é possível continuar a ser executada uma vez que ele foi expulso repetidamente do mesmo ponto de verificação"

#### <a name="issue"></a>Problema

Este comportamento é propositado devido a "Justa" monitorização de processos dentro da automatização do Azure, que suspenda automaticamente um runbook, se for executado durante mais de três horas. No entanto, a mensagem de erro devolvida não fornece "o que se segue" opções.

#### <a name="cause"></a>Causa

Um runbook pode ser suspenso por uma série de motivos. Suspende acontecer em grande parte devido a erros. Por exemplo, uma exceção não identificada num runbook, uma falha de rede ou uma falha na função de trabalho de Runbook em execução o runbook, todos com que o runbook seja suspenso e iniciar a partir do último ponto de verificação quando retomado.

#### <a name="resolution"></a>Resolução

A solução documentada para evitar este problema é usar pontos de verificação num fluxo de trabalho. Para obter mais informações, consulte [Learning fluxos de trabalho do PowerShell](../automation-powershell-workflow.md#checkpoints). Uma explicação mais completa de "Justa" e o ponto de verificação pode ser encontrada neste artigo de blogue [através de pontos de verificação em Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

### <a name="long-running-runbook"></a>Cenário: Um runbook de longa execução não consegue concluir

#### <a name="issue"></a>Problema

Este comportamento é propositado em áreas de segurança do Azure devido a "Justa" monitorização de processos dentro da automatização do Azure, que suspenda automaticamente um runbook, se for executado durante mais de três horas.

#### <a name="cause"></a>Causa

O runbook foi executado sobre o limite de duração de 3 horas permitido pelo justa numa área de segurança do Azure

#### <a name="resolution"></a>Resolução

A solução recomendada é executar o runbook num [Runbook Worker híbrido](../automation-hrw-run-runbooks.md). Funções de trabalho híbridas não estão limitadas pelos [justa](../automation-runbook-execution.md#fair-share) limite de runbook de duração de 3 horas que são áreas de segurança do Azure.

## <a name="common-errors-when-importing-modules"></a>Erros comuns ao importar módulos

### <a name="module-fails-to-import"></a>Cenário: Ocorre uma falha do módulo importar ou cmdlets não pode ser executados depois de importar

#### <a name="issue"></a>Problema

Um módulo não consegue importar ou importa com êxito, mas não existem cmdlets são extraídos.

#### <a name="cause"></a>Causa

Algumas razões comuns que um módulo não pode importar com êxito para a automatização do Azure são:

* A estrutura não corresponde a estrutura que precisa de automatização que seja.
* O módulo é dependente de outro módulo que não tenha sido implementado para a sua conta de automatização.
* O módulo está em falta as respetivas dependências na pasta.
* O `New-AzureRmAutomationModule` cmdlet está a ser utilizado para carregar o módulo, e não tiverem dado o caminho de armazenamento completo ou não ter a carregar o módulo utilizando um URL acessível publicamente.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções resolver o problema:

* Certifique-se de que o módulo segue o seguinte formato: ModuleName.Zip **->** ModuleName ou um número de versão **->** (ModuleName.psm1, ModuleName.psd1)
* Abra o ficheiro. psd1 e ver se o módulo tiver dependências. Se assim for, carregar esses módulos para a conta de automatização.
* Certifique-se de que quaisquer DLLs referenciado estão presentes na pasta do módulo.

## <a name="next-steps"></a>Passos Seguintes

Se não VI o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.