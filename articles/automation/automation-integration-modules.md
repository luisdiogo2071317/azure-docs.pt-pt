---
title: Criar um módulo de integração da automatização do Azure
description: Tutorial que o orienta através da criação, teste e exemplo de utilização dos módulos de integração na Automatização do Azure.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 579be1f240f5661503caea16f7a7e42ee31a2fdd
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495603"
---
# <a name="azure-automation-integration-modules"></a>Módulos de Integração da Automatização do Azure
O PowerShell é a tecnologia fundamental por trás da Automatização do Azure. Uma vez que a Automatização do Azure é baseada no PowerShell, os módulos do PowerShell são essenciais para a extensibilidade da Automatização do Azure. Neste artigo, iremos guiá-lo os detalhes de utilização da automatização do Azure de módulos do PowerShell, conhecido como "Módulos de integração" e as melhores práticas para criar seus próprios módulos do PowerShell para se certificar de que funcionam como módulos de integração no Azure Automatização. 

## <a name="what-is-a-powershell-module"></a>O que é um Módulo do PowerShell?
Um módulo do PowerShell é um grupo de cmdlets do PowerShell como **Get-Data** ou **Copy-Item**, que pode ser utilizado a partir da consola do PowerShell, scripts, fluxos de trabalho, runbooks e recursos do PowerShell DSC como WindowsFeature ou um Ficheiro, que podem ser utilizados a partir de configurações do PowerShell DSC. Todas as funcionalidades do PowerShell estão expostas através de cmdlets e recursos do DSC e todos os recursos do DSC/cmdlets estão protegido por um módulo do PowerShell, muitos dos quais são enviados juntamente com o PowerShell. Por exemplo, o cmdlet **Get-Date** faz parte do módulo do PowerShell Microsoft.PowerShell.Utility, o cmdlet **Copy-Item** faz parte do módulo do PowerShell Microsoft.PowerShell.Management e o recurso do Pacote DSC faz parte do módulo do PowerShell PSDesiredStateConfiguration. Ambos estes módulos são enviados com o PowerShell. Porém, muitos módulos do PowerShell não são enviados como parte do PowerShell e, em vez disso, são distribuídos com produtos proprietários ou de terceiros como o System Center 2012 Configuration Manager ou pela grande comunidade do PowerShell em locais como a Galeria do PowerShell. Os módulos são úteis porque tornam as tarefas complexas mais simples através da funcionalidade encapsulada.  Pode saber mais sobre os [módulos do PowerShell no MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>O que é um Módulo de Integração da Automatização do Azure?
Um módulo de integração não é diferente de um módulo do PowerShell. É, simplesmente, um módulo do PowerShell que, opcionalmente, contém um ficheiro adicional - um ficheiro de metadados que especifica um tipo de ligação da Automatização do Azure a utilizar com os cmdlets do módulo em runbooks. Um ficheiro opcional ou não, estes módulos do PowerShell podem ser importados para a Automatização do Azure para tornar os seus cmdlets disponíveis para utilização nos runbooks e os respetivos recursos de DSC disponíveis para utilização nas configurações de DSC. Nos bastidores, a Automatização do Azure armazena estes módulos e, no tempo de execução da tarefa de compilação de DSC e da tarefa de runbook, carrega-os para as sandboxes da Automatização do Azure, onde os runbooks são executados e as configurações de DSC são compiladas. Quaisquer recursos de DSC nos módulos também são automaticamente colocados no servidor de solicitação do Automation DSC para que possam ser solicitados pelas máquinas que tentam aplicar configurações de DSC.  

Enviamos um número de módulos do Azure PowerShell na Automatização do Azure para que possa utilizar e começar imediatamente a automatizar a gestão do Azure, mas pode importar módulos do PowerShell para qualquer sistema, serviço ou ferramenta com o qual pretende integrar. 

> [!NOTE]
> Determinados módulos são fornecidos como "módulos globais" no serviço de automatização. Estes módulos globais estão disponíveis para si quando cria uma conta de automatização e atualizamos por vezes, que automaticamente envia-os à sua conta de automatização. Se não quiser que eles sejam atualizados automaticamente, pode sempre importar o módulo por conta própria e que tem precedência sobre a versão do módulo global desse módulo que incluímos no serviço. 

O formato no qual importa um pacote do Módulo de Integração é um ficheiro comprimido com o mesmo nome que o módulo e uma extensão .zip. Contém o módulo do Windows PowerShell e quaisquer ficheiros de suporte, incluindo um ficheiro de manifesto (. psd1) se o módulo tiver um.

Se o módulo tiver de ter um tipo de ligação da Automatização do Azure, também tem de conter um ficheiro com o nome `<ModuleName>-Automation.json` que especifique as propriedades do tipo de ligação. Este é um ficheiro json colocado dentro da pasta do módulo do seu ficheiro. zip comprimido e contém os campos de uma "ligação" que é necessário para ligar ao sistema ou serviço que o módulo representa. Isso acaba criando um tipo de ligação na automatização do Azure. Ao utilizar este ficheiro, pode definir os nomes de campo, os tipos e se os campos devem ser encriptados e/ou opcionais para o tipo de ligação do módulo. Segue-se um modelo no formato de ficheiro json:

```json
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Se tiver implementado o Service Management Automation e criado pacotes de módulos de integração para os runbooks de automatização, isso lhe parecerá familiar para. 

## <a name="authoring-best-practices"></a>Melhores Práticas de Criação
Apesar de módulos de integração serem essencialmente módulos do PowerShell, ainda há um número de elementos que recomendamos que considere ao criar um módulo do PowerShell, para tornar mais útil na automatização do Azure. Alguns deles são específicos da automatização do Azure e alguns deles são úteis apenas para que os módulos funcionem bem no fluxo de trabalho do PowerShell, independentemente de estar ou não a utilizar a automatização. 

1. Inclua um resumo, uma descrição e um URI de ajuda para cada cmdlet no módulo. No PowerShell, pode definir determinadas informações de ajuda para os cmdlets para permitir ao utilizador receber ajuda sobre a utilização dos mesmos com o cmdlet **Get-Help**. Por exemplo, eis como pode definir uma sinopse e URI de ajuda para um módulo do PowerShell escrito num ficheiro. psm1.<br>  
   
    ```powershell
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='https://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> Disponibilizar estas informações não só apresentará esta ajuda relativa à utilização do cmdlet **Get-Help** na consola do PowerShell, como também expõe esta funcionalidade de ajuda dentro da Automatização do Azure.  Por exemplo, quando inserir atividades durante a criação de um runbook. Clicar em "Ver ajuda detalhada", abrirá o URI de ajuda noutro separador do browser que estiver a utilizar para aceder a automatização do Azure.<br>![Ajuda do Módulo de Integração](media/automation-integration-modules/automation-integration-module-activitydesc.png)
1. Se o módulo for executado relativamente a um sistema remoto,

    a. Deve conter um ficheiro de metadados do Módulo de Integração que defina as informações necessárias para estabelecer ligação a esse sistema remoto, ou seja, o tipo de ligação.  
    b. Cada cmdlet no módulo deve ser capaz de considerar um objeto de ligação (uma instância desse tipo de ligação) como um parâmetro.  

    Os cmdlets no módulo tornam-se mais fáceis de utilizar na Automatização do Azure se permitir a passagem de um objeto com os campos do tipo de ligação como um parâmetro para o cmdlet. Esta forma como os utilizadores não têm de mapear os parâmetros do recurso de ligação para parâmetros correspondentes do cmdlet sempre que chamarem um cmdlet. Com base no exemplo de runbook acima, utiliza um recurso de ligação do Twilio denominado CorpTwilio para aceder ao Twilio e devolver todos os números de telefone na conta.  Repara como se mapeia os campos da ligação para os parâmetros do cmdlet?<br>
   
    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Uma abordagem melhor e mais fácil consiste na passagem direta do objeto de ligação para o cmdlet -
   
    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    Pode ativar o comportamento como este para os cmdlets ao permitir que aceitem um objeto de ligação diretamente como um parâmetro, em vez de apenas os campos de ligação para os parâmetros. Normalmente, quer um parâmetro definido para cada um, para que um usuário não utilizar a automatização do Azure possa chamar os seus cmdlets sem construir uma tabela de hash para atuar como o objeto de ligação. O conjunto de parâmetros **SpecifyConnectionFields** abaixo é utilizado para a passagem das propriedades do campo de ligação individualmente. **UseConnectionObject** permite a passagem direta da ligação. Como pode ver, o cmdlet Send-TwilioSMS no [módulo Twilio do PowerShell](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) permite a passagem de qualquer uma das formas: 
   
    ```powershell
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='https://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
1. Defina o tipo de saída para todos os cmdlets no módulo. Definir um tipo de saída para um cmdlet permite que o IntelliSense, no momento da conceção, o ajude a determinar as propriedades de saída do cmdlet para uma utilização durante a criação. É especialmente útil durante a criação gráfica de runbooks na Automatização, onde o conhecimento no momento da conceção é fundamental para uma experiência de utilizador fácil com o módulo.<br><br> ![Tipo de Saída Gráfica do Runbook](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Esta situação é semelhante a funcionalidade de "escrita antecipada" da saída de um cmdlet no ISE do PowerShell sem ter de o executar.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
1. Os cmdlets no módulo não devem considerar tipos de objetos complexos como parâmetros. O Fluxo de Trabalho do PowerShell é diferente do PowerShell no sentido em que armazena tipos complexos no formato de serialização anulada. Tipos primitivos são como primitivos, mas tipos complexos são convertidos em suas versões de serialização anuladas, o que são, essencialmente, matrizes de propriedades. Por exemplo, se tiver utilizado o cmdlet **Get-Process** num runbook (ou um Fluxo de Trabalho do PowerShell), iria devolver um objeto do tipo [Deserialized.System.Diagnostic.Process] e não o tipo esperado [System.Diagnostic.Process]. Este tipo tem as mesmas propriedades que o tipo sem serialização anulada, mas nenhum dos métodos. E se tentar passar este valor como um parâmetro para um cmdlet, onde o cmdlet espera um valor de [Diagnostic] para este parâmetro, receberá o erro seguinte: *Não é possível processar a transformação do argumento no parâmetro "processo". Erro: "Não é possível converter o valor de"Process (CcmExec)"do tipo"Deserialized"no tipo"Diagnostics".*   Isto acontece porque existe um erro de correspondência entre o tipo esperado [System.Diagnostic.Process] e o tipo [Deserialized.System.Diagnostic.Process] especificado. Para contornar este problema, certifique-se de que os cmdlets do seu módulo não consideram tipos complexos como parâmetros. Eis a forma errada de o fazer.
   
    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    E eis a forma correta, considerando um primitivo que pode ser utilizado internamente pelo cmdlet para captar o objeto complexo e utilizá-lo. Uma vez que os cmdlets são executados no contexto do PowerShell, e não no Fluxo de Trabalho do PowerShell, dentro do cmdlet $process torna-se o tipo [System.Diagnostic.Process] correto.  
   
    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   Recursos de ligação nos runbooks são tabelas hash, que é um tipo complexo, e ainda que estas tabelas hash parecem conseguir passar para os cmdlets para seus - parâmetro de ligação com o perfeitamente, sem exceção de transmissão. Tecnicamente, alguns tipos de PowerShell conseguem transmitir corretamente a partir do respetivo formato serializado para o formato de serialização anulada e, por conseguinte, podem passar para os cmdlets para os parâmetros que aceitam o tipo sem serialização anulada. A tabela hash também está incluída. É possível para tipos definidos de um autor de módulos sejam implementados de forma a poderem corretamente anular a serialização, bem, mas existem algumas desvantagens a serem considerados. O tipo tem de ter um construtor predefinido, ter todas as suas propriedades públicas e ter um PSTypeConverter. No entanto, para tipos já definidos que o autor do módulo não é proprietário, não é possível para "corrigir", daí a recomendação para evitar tipos complexos para todos os parâmetros. Sugestão de criação de Runbooks: Se por algum motivo, que os cmdlets têm de executar um tipo complexo parâmetro ou estiver a utilizar de outra pessoa módulo que requer um parâmetro de tipo complexo, a solução nos runbooks do fluxo de trabalho do PowerShell e fluxos de trabalho do PowerShell no local PowerShell, é moldar o cmdlet que gera o tipo complexo e o cmdlet que consome o tipo complexo na mesma atividade do InlineScript. Uma vez que o InlineScript executa o respetivo conteúdo como o PowerShell e não como o Fluxo de Trabalho do PowerShell, o cmdlet que gera o tipo complexo produziria esse tipo correto e não o tipo complexo no formato de serialização anulada.
1. Crie todos os cmdlets no módulo sem monitorização de estado. O Fluxo de Trabalho do PowerShell executa cada cmdlet chamado no fluxo de trabalho numa sessão diferente. Isto significa que qualquer cmdlet que dependa do estado da sessão criado/modificado por outros cmdlets no mesmo módulo não funcionará nos runbooks do Fluxo de Trabalho do PowerShell.  Eis um exemplo de o que não deve fazer.
   
    ```powershell
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
1. O módulo deve estar totalmente incluído num pacote compatível com Xcopy. Uma vez que os módulos da Automatização do Azure estão distribuídos nas sandboxes da Automatização quando os runbooks precisam de executar, têm de funcionar independentemente do anfitrião no qual estão a ser executados. Isso significa que deve ser capaz de zipar se o pacote do módulo, movê-lo para qualquer outro anfitrião com o PowerShell versão igual ou mais recente e tê-lo a funcionar normalmente quando importados para o ambiente de PowerShell desse anfitrião. Para que isso aconteça, o módulo não deve depender de quaisquer ficheiros fora da pasta do módulo (a pasta que é zipada ao importar para a Automatização do Azure) ou de qualquer definição de registo única num anfitrião, como as definidas pela instalação de um produto. Se esta melhor prática não for seguida, o módulo não será utilizável na Automatização do Azure.  

## <a name="next-steps"></a>Passos Seguintes

* Para começar com runbooks do fluxo de trabalho do PowerShell, consulte o artigo [O meu primeiro runbook do fluxo de trabalho do PowerShell](automation-first-runbook-textual.md)
* Para saber mais sobre a criação de Módulos do PowerShell, veja o artigo [Escrever um Módulo do Windows PowerShell](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)


