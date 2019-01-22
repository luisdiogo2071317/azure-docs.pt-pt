---
title: Parâmetros de entrada do runbook
description: Parâmetros de entrada do Runbook aumentam a flexibilidade de runbooks, permitindo-lhe transmitir dados a um runbook quando é iniciada. Este artigo descreve os diferentes cenários em que os parâmetros de entrada são utilizados em runbooks.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d22a2de29e170979d9ab5d61c7f21a47d6aee99c
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54433446"
---
# <a name="runbook-input-parameters"></a>Parâmetros de entrada do runbook

Parâmetros de entrada do Runbook aumentam a flexibilidade de runbooks, permitindo-lhe transmitir dados a ele quando é iniciada. Os parâmetros permitem que as ações de runbook, estará direcionada para cenários específicos e ambientes. Neste artigo, é necessário percorrer diferentes cenários em que os parâmetros de entrada são utilizados em runbooks.

## <a name="configure-input-parameters"></a>Configurar parâmetros de entrada

Parâmetros de entrada podem ser configurados no PowerShell, fluxo de trabalho do PowerShell, Python e runbooks gráficos. Um runbook pode ter vários parâmetros com tipos de dados diferentes, ou sem parâmetros de todo. Parâmetros de entrada podem ser obrigatório ou opcional e pode atribuir um valor predefinido para os parâmetros opcionais. É possível atribuir valores para os parâmetros de entrada para um runbook quando o iniciar por meio de um dos métodos disponíveis. Estes métodos incluem a iniciar um runbook a partir do portal ou de um serviço web. Também pode iniciar uma como um runbook subordinado que é chamado inline no outro runbook.

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a>Configurar parâmetros de entrada em runbooks do PowerShell e o fluxo de trabalho do PowerShell

PowerShell e [runbooks de fluxo de trabalho do PowerShell](automation-first-runbook-textual.md) na automatização do Azure suporta parâmetros de entrada que são definidos por meio dos seguintes atributos:  

| **Propriedade** | **Descrição** |
|:--- |:--- |
| Tipo |Necessário. O tipo de dados esperado para o valor do parâmetro. Qualquer tipo .NET é válido. |
| Nome |Necessário. O nome do parâmetro. Isso tem de ser exclusivo dentro do runbook e pode conter apenas letras, números ou carateres de sublinhado. Tem de começar com uma letra. |
| Obrigatório |Opcional. Especifica se deve ser fornecido um valor para o parâmetro. Se definir esta opção como **$true**, em seguida, deve ser fornecido um valor quando o runbook é iniciado. Se definir esta opção como **$false**, em seguida, um valor é opcional. |
| Valor predefinido |Opcional. Especifica um valor que é utilizado para o parâmetro se um valor não é passado quando o runbook é iniciado. Um valor predefinido pode ser definido para cada parâmetro e fará automaticamente o parâmetro opcional, independentemente da definição obrigatória. |

Windows PowerShell oferece suporte a mais atributos de parâmetros de entrada do que aqueles listados aqui, como validação, aliases, e define o parâmetro. No entanto, a automatização do Azure atualmente suporta apenas os parâmetros de entrada anteriores.

Uma definição de parâmetro nos runbooks do fluxo de trabalho do PowerShell tem a seguinte forma geral, em que os parâmetros múltiplos são separados por vírgulas.

```powershell
Param
(
  [Parameter (Mandatory= $true/$false)]
  [Type] $Name1 = <Default value>,

  [Parameter (Mandatory= $true/$false)]
  [Type] $Name2 = <Default value>
)
```

> [!NOTE]
> Quando a definição de parâmetros, se não especificar a **obrigatório** de atributos, em seguida, por predefinição, o parâmetro é considerado opcional. Além disso, se definir um valor predefinido para um parâmetro em runbooks do fluxo de trabalho do PowerShell, ela é tratada pelo PowerShell como um parâmetro opcional, independentemente do **obrigatório** valor do atributo.
> 
> 

Por exemplo, vamos configurar os parâmetros de entrada para um runbook de fluxo de trabalho do PowerShell que produz detalhes sobre as máquinas virtuais, uma VM única ou todas as VMs dentro de um grupo de recursos. Este runbook tem dois parâmetros, como mostrado na captura de ecrã seguinte: o nome da máquina virtual e o nome do grupo de recursos.

![Fluxo de trabalho de PowerShell de automatização](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

Esta definição de parâmetro, os parâmetros **$VMName** e **$resourceGroupName** são parâmetros simples do tipo cadeia. No entanto, os runbooks do PowerShell e o fluxo de trabalho do PowerShell suportam todos os tipos simples e tipos complexos, como **objeto** ou **PSCredential** para parâmetros de entrada.

Se o runbook tem um parâmetro de entrada de tipo de objeto, em seguida, utilizar uma tabela de hash do PowerShell com (nome, valor) pares de passar um valor. Por exemplo, se tiver o seguinte parâmetro num runbook:

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

Em seguida, pode passar o seguinte valor para o parâmetro:

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```
> [!NOTE]
> Quando não passar nenhum valor para opcional `[String]` escreva o parâmetro que tem um _valor predefinido_ de `$null`, o valor do parâmetro será um _vazio a cadeia de caracteres_, **não** `$null`.
> 
> 

## <a name="configure-input-parameters-in-graphical-runbooks"></a>Configurar parâmetros de entrada em runbooks gráficos

Para [configurar um runbook gráfico](automation-first-runbook-graphical.md) com parâmetros de entrada, vamos criar um runbook gráfico que apresenta detalhes sobre as máquinas virtuais, optar por uma única VM ou todas as VMs dentro de um grupo de recursos. Configurar um runbook é composta por duas atividades principais, conforme descrito abaixo.

[**Autenticar Runbooks com a conta Run As do Azure** ](automation-sec-configure-azure-runas-account.md) para autenticar com o Azure.

[**Get-AzureRmVm** ](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) para obter as propriedades de uma máquina virtual.

Pode utilizar o [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) atividade para os nomes das máquinas virtuais de saída. A atividade **Get-AzureRmVm** aceita dois parâmetros, o **nome da máquina virtual** e o **nome do grupo de recursos**. Uma vez que estes parâmetros poderiam exigir valores diferentes sempre que iniciar o runbook, pode adicionar parâmetros de entrada ao runbook. Eis os passos para adicionar parâmetros de entrada:

1. Selecione o runbook gráfico do **Runbooks** painel e clique em [ **editar** ](automation-graphical-authoring-intro.md) -lo.
2. No editor de runbook, clique em **de entrada e saída** para abrir o **entrada e saída** painel.
   
    ![Runbook gráfico da automatização](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. O **de entrada e saída** painel mostra uma lista de parâmetros de entrada definidos para o runbook. Neste painel, pode adicionar um novo parâmetro de entrada ou editar a configuração de um parâmetro de entrada existente. Para adicionar um novo parâmetro para o runbook, clique em **Adicionar entrada** para abrir o **parâmetro de entrada do Runbook** painel. Aqui, pode configurar os seguintes parâmetros:
   
   | **Propriedade** | **Descrição** |
   |:--- |:--- |
   | Nome |Necessário. O nome do parâmetro. Isso tem de ser exclusivo dentro do runbook e pode conter apenas letras, números ou carateres de sublinhado. Tem de começar com uma letra. |
   | Descrição |Opcional. Descrição sobre a finalidade do parâmetro de entrada. |
   | Tipo |Opcional. O tipo de dados que é esperado para o valor do parâmetro. Tipos de parâmetro suportados são **cadeia de caracteres**, **Int32**, **Int64**, **Decimal**, **booleano**,  **DateTime**, e **objeto**. Se um tipo de dados não estiver selecionado, é assumida como predefinição para **cadeia de caracteres**. |
   | Obrigatório |Opcional. Especifica se deve ser fornecido um valor para o parâmetro. Se escolher **Sim**, em seguida, deve ser fornecido um valor quando o runbook é iniciado. Se escolher **nenhuma**, em seguida, um valor não é necessário quando o runbook é iniciado e pode ser definido um valor predefinido. |
   | Valor Predefinido |Opcional. Especifica um valor que é utilizado para o parâmetro se um valor não é passado quando o runbook é iniciado. Pode ser definido um valor predefinido para um parâmetro que não é obrigatório. Para definir um valor predefinido, escolher **personalizado**. Este valor é utilizado, a menos que outro valor é fornecido quando o runbook é iniciado. Escolher **None** se não quiser fornecer qualquer valor predefinido. |
   
    ![Adicionar nova entrada](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Criar dois parâmetros com as seguintes propriedades, que é utilizada pelos **Get-AzureRmVm** atividade:
   
   * **Parameter1:**
     
     * Nome - VMName
     * Tipo - cadeia de caracteres
     * Obrigatório - não
   * **Parameter2:**
     
     * Nome - resourceGroupName
     * Tipo - cadeia de caracteres
     * Obrigatório - não
     * Valor predefinido - personalizado
     * Valor predefinido personalizado - \<nome do grupo de recursos que contém as máquinas virtuais >
5. Depois de adicionar os parâmetros, clique em **OK**. Agora pode visualizá-las no **entrada e o painel de saída**. Clique em **OK** novamente e, em seguida, clique em **guardar** e **publicar** runbook.

## <a name="configure-input-parameters-in-python-runbooks"></a>Configurar parâmetros de entrada nos runbooks de Python

Ao contrário do PowerShell, o fluxo de trabalho do PowerShell e runbooks gráficos, os runbooks de Python não têm parâmetros nomeados.
Todos os parâmetros de entrada são analisados como uma matriz de valores de argumento.
Aceder à matriz através da importação de `sys` módulo no seu script de Python e, em seguida, usando o `sys.argv` matriz.
É importante ter em atenção que o primeiro elemento da matriz, `sys.argv[0]`, é o nome do script, pelo que é o primeiro parâmetro de entrada real `sys.argv[1]`.

Para obter um exemplo de como usar parâmetros de entrada de um runbook de Python, veja [meu primeiro runbook de Python na automatização do Azure](automation-first-runbook-textual-python2.md).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Atribuir valores de parâmetros em runbooks de entrada

Pode passar valores para parâmetros em runbooks nos seguintes cenários de entrada:

### <a name="start-a-runbook-and-assign-parameters"></a>Iniciar um runbook e atribuir os parâmetros

Um runbook pode ser iniciado várias formas: através do portal do Azure, com um webhook, com os cmdlets do PowerShell, com a API REST ou com o SDK. A seguir, vamos abordar diferentes métodos para iniciar um runbook e a atribuição de parâmetros.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Iniciar um runbook publicado com o portal do Azure e atribuir os parâmetros

Quando [iniciar o runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), o **iniciar Runbook** é aberto o painel e pode introduzir valores para os parâmetros que criou.

![Começar a utilizar o portal](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Na etiqueta por baixo da caixa de entrada, pode ver os atributos que foram definidos para o parâmetro. Atributos incluem obrigatório ou opcional, tipo e valor predefinido. No balão de ajuda junto ao nome do parâmetro, pode ver todas as informações da chave tem de tomar decisões sobre valores de entrada do parâmetro. Estas informações incluem se um parâmetro é obrigatório ou opcional. Ele também inclui o tipo e valor padrão (se houver) e outras notas úteis.

> [!NOTE]
> Suporte de parâmetros de tipo de cadeia de caracteres **vazio** valores de cadeias de caracteres.  Introduzir **[Cadeiavazia]** o parâmetro de entrada caixa passa uma cadeia vazia para o parâmetro. Além disso, não oferecem suporte a parâmetros de tipo de cadeia de caracteres **nulo** valores a ser transmitidos. Se não passar qualquer valor para o parâmetro de cadeia de caracteres, em seguida, PowerShell interpreta-o como nulo.
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Iniciar um runbook publicado utilizando cmdlets do PowerShell e atribuir os parâmetros

* **Cmdlets do Gestor de recursos do Azure:** Pode iniciar um runbook da automatização que foi criado num grupo de recursos por meio [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook).
  
  **Exemplo:**
  
  ```powershell
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* **Cmdlets do modelo de implementação clássica do Azure:** Pode iniciar um runbook da automatização que foi criado num grupo de recursos padrão por meio [Start-AzureAutomationRunbook](https://docs.microsoft.com/powershell/module/servicemanagement/azure/start-azureautomationrunbook).
  
  **Exemplo:**
  
  ```powershell
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> Quando inicia um runbook com cmdlets do PowerShell, um parâmetro predefinido, **MicrosoftApplicationManagementStartedBy** é criado com o valor **PowerShell**. Pode ver este parâmetro na **detalhes da tarefa** painel.  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Iniciar um runbook com um SDK e atribuir os parâmetros

* **Método do Gestor de recursos do Azure:** Pode iniciar um runbook com o SDK de uma linguagem de programação. Segue-se um trecho de código c# para iniciar um runbook na conta de automatização. Pode ver todo o código na nossa [repositório do GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  
  
  ```csharp
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* **Método de modelo de implementação clássica do Azure:** Pode iniciar um runbook com o SDK de uma linguagem de programação. Segue-se um trecho de código c# para iniciar um runbook na conta de automatização. Pode ver todo o código na nossa [repositório do GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).
  
  ```csharp
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  Para começar a este método, criar um dicionário para armazenar os parâmetros do runbook **VMName** e **resourceGroupName**e os respetivos valores. Em seguida, inicie o runbook. Segue-se o fragmento de código c# para chamar o método que é definido acima.
  
  ```csharp
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>Iniciar um runbook com a API do REST e atribuir os parâmetros
Uma tarefa de runbook pode ser criada e iniciada com a API REST da automatização do Azure através da **colocar** método com o URI do pedido seguinte: https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`


No URI do pedido, substitua os seguintes parâmetros:

* **subscription-id:** ID da subscrição do Azure.  
* **cloud-service-name:** O nome do serviço cloud para o qual o pedido deve ser enviado.  
* **automation-account-name:** O nome da sua conta de automatização que esteja alojado no serviço em nuvem especificado.  
* **job-id:** O GUID para a tarefa. GUIDs no PowerShell podem ser criados utilizando o **[GUID]::NewGuid(). ToString ()** comando.

Para poder passar os parâmetros para a tarefa de runbook, utilize o corpo do pedido. Ele usa as seguintes duas propriedades fornecidas no formato JSON:

* **Nome do Runbook:** Necessário. O nome do runbook para a tarefa iniciar.  
* **Parâmetros do Runbook:** Opcional. Um dicionário de lista de parâmetros (nome, valor) formato em que o nome deve ser do tipo cadeia e o valor pode ser qualquer valor JSON válido.

Se deseja iniciar o **Get-AzureVMTextual** runbook criado anteriormente com **VMName** e **resourceGroupName** como parâmetros, utilize o seguinte formato JSON para o corpo do pedido.

   ```json
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

Código de estado HTTP 201 é devolvido se a tarefa é criada com êxito. Para obter mais informações sobre cabeçalhos de resposta e o corpo de resposta, consulte o artigo sobre como [criar uma tarefa de runbook com a API REST.](https://msdn.microsoft.com/library/azure/mt163849.aspx)

### <a name="test-a-runbook-and-assign-parameters"></a>Testar um runbook e atribuir os parâmetros
Quando [testar a versão de rascunho de runbook](automation-testing-runbook.md) ao utilizar a opção de teste, o **testar** é aberta a página e pode configurar os valores para os parâmetros que criou.

![Parâmetros de teste e atribuir](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Ligar uma agenda a um runbook e atribuir os parâmetros
Pode [ligar uma agenda](automation-schedules.md) ao runbook para que o runbook é iniciado num momento específico. Atribuir os parâmetros de entrada ao criar o agendamento e o runbook utiliza estes valores quando é iniciada, a agenda. Não é possível guardar o agendamento até que todos os valores dos parâmetros obrigatórios são fornecidos.

![Agendar e atribuir os parâmetros](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Criar um webhook para um runbook e atribuir os parâmetros
Pode criar uma [webhook](automation-webhooks.md) para o runbook e configurar parâmetros de entrada do runbook. Não é possível guardar o webhook até que todos os valores dos parâmetros obrigatórios são fornecidos.

![Criar webhook e atribuir os parâmetros](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Quando executar um runbook ao utilizar um webhook, o parâmetro de entrada predefinido **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** é enviado, juntamente com os parâmetros de entrada que definiu. Pode clicar para expandir a **WebhookData** parâmetro para obter mais detalhes.

![Parâmetro de WebhookData](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre a entrada do runbook e de saída, consulte [automatização do Azure: runbook de entrada, saída e runbooks aninhados](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).
* Para obter detalhes sobre diferentes formas de iniciar um runbook, consulte [a partir de um runbook](automation-starting-a-runbook.md).
* Para editar um runbook textual, consulte [editar runbooks textuais](automation-edit-textual-runbook.md).
* Para editar um runbook gráfico, consulte [criação de gráficos na automatização do Azure](automation-graphical-authoring-intro.md).


