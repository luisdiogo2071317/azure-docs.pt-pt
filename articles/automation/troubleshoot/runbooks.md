---
title: Resolver erros com Runbooks de automatização do Azure
description: Saiba como resolver problemas com runbooks de automatização do Azure
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: bb340b8439927f191bc4a22f385d85d4e21b1cdb
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063947"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Resolver erros com runbooks

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Erros de autenticação quando trabalhar com runbooks de automatização do Azure

### <a name="sign-in-failed"></a>Cenário: Início de sessão na conta do Azure falhou

#### <a name="issue"></a>Problema

Receber o erro seguinte ao trabalhar com o `Add-AzureAccount` ou `Connect-AzureRmAccount` cmdlets.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Causa

Este erro ocorre se o nome de recurso de credencial não é válido ou se o nome de utilizador e palavra-passe que utilizou para configurar o recurso de credencial de automatização não são válidos.

#### <a name="resolution"></a>Resolução

Para determinar o que é o problema, siga os passos seguintes:  

1. Certifique-se de que não tem quaisquer carateres especiais, incluindo o **@** caráter no nome do recurso de credencial de automatização que está a utilizar para ligar ao Azure.  
2. Certifique-se de que pode utilizar o nome de utilizador e palavra-passe que são armazenadas na credencial de automatização do Azure no seu editor local do ISE do PowerShell. Pode fazê-lo executando os seguintes cmdlets no ISE do PowerShell:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Se a autenticação falhar localmente, isto significa que ainda não configurou as credenciais do Azure Active Directory corretamente. Consulte [autenticação no Azure utilizando o Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogue para obter a conta do Azure Active Directory corretamente configurado.  

### <a name="unable-to-find-subscription"></a>Cenário: Não foi possível encontrar a subscrição do Azure

#### <a name="issue"></a>Problema

Receber o erro seguinte ao trabalhar com o `Select-AzureSubscription` ou `Select-AzureRmSubscription` cmdlets.:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Erro

Este erro ocorre se o nome da subscrição não é válido ou se o utilizador do Azure Active Directory que está a tentar obter os detalhes da subscrição não está configurado como um administrador da subscrição.

#### <a name="resolution"></a>Resolução

Para determinar se corretamente ter autenticado para o Azure e tem acesso à subscrição que está a tentar a selecionar, siga os passos seguintes:  

1. Certifique-se de que executa o **Add-AzureAccount** antes de executar o **Select-AzureSubscription** cmdlet.  
2. Se continuar a ver esta mensagem de erro, modificar o seu código ao adicionar o **Get-AzureSubscription** seguinte cmdlet a **Add-AzureAccount** cmdlet e, em seguida, execute o código. Agora, verifique se a saída de Get-AzureSubscription contém os detalhes da sua subscrição.  

   * Se não vir quaisquer detalhes da subscrição na saída, isto significa que a subscrição não está inicializada.  
   * Se vir os detalhes da subscrição na saída, confirme que está a utilizar o nome de subscrição correta ou o ID com a **Select-AzureSubscription** cmdlet.

### <a name="auth-failed-mfa"></a>Cenário: Autenticação no Azure falhou porque a autenticação multifator é ativada

#### <a name="issue"></a>Problema

Recebe o erro seguinte durante a autenticação do Azure com o nome de utilizador do Azure e a palavra-passe:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Causa

Se tiver a autenticação multifator na sua conta do Azure, não é possível utilizar um utilizador do Azure Active Directory para autenticar para o Azure. Em vez disso, terá de utilizar um certificado ou um principal de serviço para autenticar para o Azure.

#### <a name="resolution"></a>Resolução

Para utilizar um certificado com os cmdlets do modelo de implementação clássico do Azure, consulte [criação e adição de um certificado para gerir os serviços do Azure.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Para utilizar um principal de serviço com os cmdlets do Azure Resource Manager, consulte [criar serviço principal através do portal do Azure](../../azure-resource-manager/resource-group-create-service-principal-portal.md) e [autenticar um principal de serviço com o Azure Resource Manager.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Erros comuns ao trabalhar com runbooks

### <a name="job-attempted-3-times"></a>Cenário: O início da tarefa de runbook foi tentado três vezes, mas não foi possível a cada hora de início

#### <a name="issue"></a>Problema

O runbook falha com o erro:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Causa

Este erro pode dever-se pelos seguintes motivos:

1. Limite de memória. Existem limites indicados na quantidade de memória atribuída a uma Sandbox [limites de serviço de automatização](../../azure-subscription-service-limits.md#automation-limits) para uma tarefa o poderá falhar se estiver a utilizar mais de 400 MB de memória.

2. Módulo incompatível. Isto pode ocorrer se as dependências de módulo não estão corretas e se não forem, o runbook devolve, normalmente, um "comando não foi encontrado" ou "Não é possível vincular o parâmetro" mensagem.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções corrigir o problema:

* Métodos sugeridos para trabalhar dentro do limite de memória são dividir a carga de trabalho entre vários runbooks, não processar como a quantidade de dados na memória, não ao escrever a saída desnecessária a partir dos seus runbooks ou considere quantos pontos de verificação escrever para o seu fluxo de trabalho do PowerShell runbooks.  

* Atualizar os módulos do Azure, seguindo os passos [como atualizar os módulos do Azure PowerShell na automatização do Azure](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Cenário: O Runbook falhar devido ao objeto de serialização anulado

#### <a name="issue"></a>Problema

O runbook falha com o erro:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Causa

Se o runbook for um fluxo de trabalho do PowerShell, armazena objetos complexos num formato de serialização anulado para manter o estado de runbook se o fluxo de trabalho é suspenso.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes três soluções corrigir este problema:

1. Se estiver a encaminhando os objetos complexos de um cmdlet para outro, molde estes cmdlets num InlineScript.
2. Transmita o nome ou valor que precisa do objeto complexo em vez de transmitir todo o objeto.
3. Utilize um runbook do PowerShell em vez de um runbook de fluxo de trabalho do PowerShell.

### <a name="quota-exceeded"></a>Cenário: A tarefa de Runbook falhou porque a quota alocada excedido

#### <a name="issue"></a>Problema

Falha da tarefa de runbook com o erro:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Causa

Este erro ocorre quando a execução da tarefa excede a quota livre 500 minuto para a sua conta. Esta quota é aplicada a todos os tipos de tarefas de execução da tarefa como uma tarefa de teste, inicie uma tarefa a partir do portal, executar uma tarefa utilizando webhooks e agendar uma tarefa para executar utilizando o portal do Azure ou no seu centro de dados. Para obter mais informações sobre preços para automatização, consulte [automatização preços](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Resolução

Se pretender utilizar mais do que 500 minutos de processamento por mês, terá de alterar a sua subscrição do gratuito camada para o escalão básico. Pode atualizar para o escalão básico, efetuando os seguintes passos:  

1. Inicie sessão na sua subscrição do Azure  
2. Selecione a conta de Automatização que quer atualizar  
3. Clique em **definições** > **preços**.
4. Clique em **ativar** na parte inferior da página para atualizar a sua conta para o **básico** camada.

### <a name="cmdlet-not-recognized"></a>Cenário: Cmdlet não reconhecido ao executar um runbook

#### <a name="issue"></a>Problema

Falha da tarefa de runbook com o erro:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Causa

Este erro é causado quando o motor do PowerShell não é possível encontrar o cmdlet que está a utilizar no runbook. Isto pode dever-se ao facto do módulo que contém o cmdlet está em falta da conta do, existe um conflito de nomes com um nome de runbook, ou o cmdlet também existe no outro módulo e automatização não é possível resolver o nome.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções corrigir o problema:  

* Verifique se introduziu o nome do cmdlet corretamente.  
* Certificar-se de que o cmdlet existe na sua conta de automatização e de que existem não está em conflito. Para verificar se o cmdlet está presente, abra um runbook no modo de edição e procure o cmdlet que pretende localizar na biblioteca ou executar `Get-Command <CommandName>`. Depois de validar que o cmdlet está disponível para a conta e de que não existem nenhum nome entra em conflito com outros cmdlets ou runbooks, adicioná-lo para a tela e certifique-se de que está a utilizar um parâmetro válido definido no runbook.  
* Se tiver um conflito de nomes e o cmdlet está disponível em dois módulos diferentes, pode resolver isto, utilizando o nome completamente qualificado para o cmdlet. Por exemplo, pode utilizar **ModuleName\CmdletName**.  
* Se estiver a executar o runbook no local num grupo de trabalho híbrida, em seguida, certifique-se de que o módulo/cmdlet está instalado no computador que aloja o worker híbrido.

### <a name="evicted-from-checkpoint"></a>Cenário: Um runbook de execução longa consistentemente falha com a exceção: "a tarefa não é possível continuar a ser executada porque este foi expulso repetidamente do ponto de verificação mesmo"

#### <a name="issue"></a>Problema

Este comportamento é propositado devido a monitorização de "Quota justa" dos processos dentro da automatização do Azure, que suspenda automaticamente um runbook se a ser executada mais do que três horas. No entanto, a mensagem de erro devolvida não fornece "que seguinte" opções.

#### <a name="cause"></a>Causa

Um runbook pode ser suspensa para uma série de razões. Suspende acontecer principalmente devido a erros. Por exemplo, uma exceção não identificada no runbook, uma falha de rede ou uma falha na função de trabalho de Runbook executar o runbook, todos os fazer com que o runbook seja suspenso e iniciar a partir do último ponto de verificação quando retomado.

#### <a name="resolution"></a>Resolução

A solução documentada para evitar este problema é utilizar pontos de verificação num fluxo de trabalho. Para obter mais informações, consulte [Learning fluxos de trabalho do PowerShell](../automation-powershell-workflow.md#checkpoints). Pode encontrar uma explicação mais detalhada de "Quota justa" e o ponto de verificação neste artigo do blogue [utilizando pontos de verificação em Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Erros comuns ao importar os módulos

### <a name="module-fails-to-import"></a>Cenário: Ocorre uma falha ao importar o módulo ou os cmdlets não pode ser executados após a importação

#### <a name="issue"></a>Problema

Um módulo falha importar ou importa com êxito, mas não os cmdlets são extraídos.

#### <a name="cause"></a>Causa

Algumas razões comuns que um módulo não pode importar com êxito para a automatização do Azure são:

* A estrutura não corresponde a estrutura de automatização necessita que seja na.
* O módulo está depende de outro módulo que não tenha sido implementado a sua conta de automatização.
* O módulo está a faltar as respetivas dependências na pasta.
* O `New-AzureRmAutomationModule` cmdlet está a ser utilizado para carregar o módulo, e não atribuiu o caminho de armazenamento completo ou não carregou o módulo utilizando um URL acessível publicamente.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções corrigir o problema:

* Certifique-se de que o módulo de acordo com o seguinte formato: ModuleName.Zip **->** ModuleName ou um número de versão **->** (ModuleName.psm1, ModuleName.psd1)
* Abra o ficheiro. psd1 e se o módulo tem quaisquer dependências. Se existir, carregar estes módulos para a conta de automatização.
* Certifique-se de que qualquer .dlls referenciado estão presentes na pasta do módulo.

## <a name="next-steps"></a>Passos Seguintes

Se não foi possível ver o seu problema ou não conseguir resolver o problema, visite uma das seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporta**.