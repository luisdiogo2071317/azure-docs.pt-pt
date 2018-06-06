---
title: Iniciar um runbook de automatização do Azure com um webhook
description: Webhook que permite que um cliente iniciar um runbook na automatização do Azure a partir de uma chamada HTTP.  Este artigo descreve como criar um webhook e como chamar um para iniciar um runbook.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e047dffa86915b0cd6e8829ea27e0335e7f88cb2
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757161"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Iniciar um runbook de automatização do Azure com um webhook

A *webhook* permite-lhe iniciar um determinado runbook na automatização do Azure através de um único pedido HTTP. Isto permite que os serviços externos, como o Visual Studio Team Services, o GitHub, Log Analytics do Azure ou aplicações personalizadas para iniciar runbooks sem a implementar uma solução completa utilizando a API de automatização do Azure.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Pode comparar webhooks para outros métodos de iniciar um runbook [iniciar um runbook na automatização do Azure](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Detalhes de um webhook

A tabela seguinte descreve as propriedades que é necessário configurar para um webhook.

| Propriedade | Descrição |
|:--- |:--- |
| Nome |Pode fornecer um nome que pretende para um webhook, uma vez que este não está exposta ao cliente. Só é utilizada por si para identificar o runbook na automatização do Azure. <br> Como melhor prática, deve dar o webhook um nome relacionadas com o cliente que o utiliza. |
| do IdP |O URL do webhook é o endereço exclusivo que chama um cliente com um POST de HTTP para iniciar o runbook associado para o webhook. É gerado automaticamente quando criar o webhook. Não é possível especificar um URL personalizado. <br> <br> O URL contém um token de segurança que permite que o runbook seja invocado por um sistema de terceiros sem autenticação adicional. Por este motivo, deve ser tratado como uma palavra-passe. Por motivos de segurança, só pode ver o URL no portal do Azure no momento que é criar o webhook. Anote o URL numa localização segura para utilização futura. |
| Data de validade |Como um certificado, cada webhook tem uma data de expiração em que momento que já não pode ser utilizado. Esta data de expiração pode ser modificada depois de criar o webhook. |
| Ativado |Um webhook está ativado por predefinição, quando é criado. Se definir como desativado, nenhum cliente é conseguir utilizá-lo. Pode definir o **ativado** propriedade ao criar o webhook ou em qualquer altura uma vez é criada. |

### <a name="parameters"></a>Parâmetros

Um webhook pode definir valores de parâmetros do runbook que são utilizados quando o runbook for iniciado por essa webhook. O webhook tem de incluir valores para todos os parâmetros obrigatórios do runbook e pode incluir valores de parâmetros opcionais. Um valor de parâmetro configurado para um webhook pode ser modificado depois de criar o webhook. Cada um vários webhooks associados a um único runbook pode utilizar valores de parâmetro diferentes.

Quando um cliente inicia um runbook com um webhook, este não pode substituir os valores de parâmetros definidos no webhook. Para receber dados do cliente, o runbook pode aceitar um único parâmetro chamado **$WebhookData** do tipo [object], que contém dados que inclui o cliente no pedido POST.

![Propriedades de Webhookdata](media/automation-webhooks/webhook-data-properties.png)

O **$WebhookData** objeto tem as seguintes propriedades:

| Propriedade | Descrição |
|:--- |:--- |
| WebhookName |O nome do webhook. |
| RequestHeader |Tabela hash que contém os cabeçalhos do pedido POST a receber. |
| RequestBody |O corpo do pedido POST a receber. Isto mantém qualquer formatação como cadeia JSON, XML, ou dados codificados do formulário. O runbook deve ser escrito para trabalhar com o formato dos dados que é esperado. |

Não há nenhuma configuração de webhook necessário para suportar o **$WebhookData** parâmetro e o runbook não é necessário aceitá-lo. Se o runbook não defina o parâmetro, os detalhes do pedido enviados do cliente é ignorada.

Se especificar um valor para $WebhookData ao criar o webhook que valor é substituído quando o webhook inicia o runbook com os dados do pedido POST cliente, mesmo que o cliente não incluir quaisquer dados no corpo do pedido. Se iniciar um runbook que tenha $WebhookData utilizando um método que não seja um webhook, pode fornecer um valor para $Webhookdata é reconhecida pelo runbook. Este valor deve ser um objeto com o mesmo [propriedades](#details-of-a-webhook) como $Webhookdata para que o runbook possam funcionar corretamente com o mesmo como se estava a funcionar com WebhookData real transmitido por um webhook.

Por exemplo, se estiver a iniciar o runbook seguinte do portal do Azure e pretender passar algumas WebhookData de exemplo para fins de teste, uma vez que WebhookData é um objeto, deve ser transmitida como JSON na IU.

![Parâmetro de WebhookData da IU](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Para o runbook seguinte, se tem as seguintes propriedades para o parâmetro WebhookData:

* WebhookName: *MyWebhook*
* RequestBody: *[{'ResourceGroup': 'myResourceGroup', 'Name': 'vm01'}, {'ResourceGroup': 'myResourceGroup', 'Name': 'vm02'}]*

Em seguida, teria de passar o seguinte valor JSON na IU para o parâmetro WebhookData. O exemplo seguinte com avanço devolve e carateres de nova linha corresponde ao formato que é transmitido a partir de um webhook.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![Parâmetro de WebhookData de início da IU](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Os valores de todos os parâmetros de entrada com o qual iniciou a tarefa de runbook. Isto significa que qualquer entrada fornecida pelo cliente no pedido de webhook será iniciada e disponíveis para qualquer pessoa com acesso para a tarefa de automatização.  Por este motivo, deve ter atenção sobre, incluindo informações confidenciais chamadas para o webhook.

## <a name="security"></a>Segurança

A segurança de um webhook depende de privacidade do respetivo URL, que contém um token de segurança que permite ser invocado. A automatização do Azure não efetuar qualquer autenticação no pedido, desde é efetuado para o URL correto. Por este motivo, webhooks não deve ser utilizada para runbooks que efetuam funções altamente confidenciais sem utilizar um meio alternativo de validar o pedido.

Pode incluir lógica dentro do runbook para determinar se este foi chamado por um webhook, verificando o **WebhookName** propriedade do parâmetro $WebhookData. O runbook foi possível efetuar mais validação, procurando informações específicas a **RequestHeader** ou **RequestBody** propriedades.

É outra estratégia para que o runbook efetuar algumas validação de uma condição externa quando recebeu um pedido de webhook. Por exemplo, considere um runbook que é chamado pelo GitHub sempre que há uma novo consolidação para conseguir um repositório do GitHub. O runbook pode ligar-se ao GitHub para validar que um novo consolidação tinha ocorreu antes de continuar.

## <a name="creating-a-webhook"></a>Criar um webhook

Utilize o procedimento seguinte para criar um novo webhook ligado a um runbook no portal do Azure.

1. Do **Runbooks página** no portal do Azure, clique o runbook inicia o webhook para ver a página de detalhes.
1. Clique em **Webhook** na parte superior da página para abrir o **adicionar Webhook** página.
1. Clique em **criar novo webhook** para abrir o **página de criação de webhook**.
1. Especifique um **nome**, **data de expiração** para o webhook e se deve ser ativada. Consulte [detalhes de um webhook](#details-of-a-webhook) para obter mais informações estas propriedades.
1. Clique no ícone de cópia e prima Ctrl + C para copiar o URL do webhook. Em seguida, registe-o num local seguro. **Depois de criar o webhook, não é possível obter o URL novamente.**

   ![URL de webhook](media/automation-webhooks/copy-webhook-url.png)

1. Clique em **parâmetros** para fornecer valores para os parâmetros do runbook. Se o runbook tiver parâmetros obrigatórios, em seguida, não é possível criar o webhook, a menos que os valores são fornecidos.
1. Clique em **criar** para criar o webhook.

## <a name="using-a-webhook"></a>Utilizar um webhook

Para utilizar um webhook depois de ter sido criado, a aplicação cliente tem de emitir um HTTP POST com o URL para o webhook. A sintaxe do webhook é o seguinte formato:

```http
http://<Webhook Server>/token?=<Token Value>
```

O cliente recebe um dos códigos de retorno seguintes no pedido de POST.

| Código | Texto | Descrição |
|:--- |:--- |:--- |
| 202 |Aceite |O pedido foi aceite e o runbook com êxito foi colocado em fila. |
| 400 |Pedido Incorreto |O pedido não foi aceite para uma das seguintes razões: <ul> <li>O webhook expirou.</li> <li>O webhook está desativado.</li> <li>O token no URL é inválido.</li>  </ul> |
| 404 |Não Encontrado |O pedido não foi aceite para uma das seguintes razões: <ul> <li>Não foi encontrado o webhook.</li> <li>O runbook não foi encontrado.</li> <li>A conta não foi encontrada.</li>  </ul> |
| 500 |Erro Interno do Servidor |O URL era válido, mas ocorreu um erro. Volte a submeter o pedido. |

Pressupondo que o pedido for bem sucedido, a resposta do webhook contém o id da tarefa no formato JSON da seguinte forma. Irá conter um id de tarefa única, mas permite que o formato JSON para potenciais melhoramentos futuros.

```json
{"JobIds":["<JobId>"]}
```

O cliente não consegue determinar quando a tarefa de runbook é concluída ou o estado de conclusão do webhook. Pode determinar estas informações com o id da tarefa com outro método, tais como [do Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) ou [API de automatização do Azure](/rest/api/automation/job).

## <a name="sample-runbook"></a>Runbook de exemplo

O runbook de exemplo seguintes aceita o aceita dados de webhook e inicia as máquinas virtuais especificadas no corpo do pedido. Para testar este runbook na sua conta de automatização em **Runbooks**, clique em **+ adicionar um runbook**. Se não souber como criar um runbook, consulte o artigo [criar um runbook](automation-quickstart-create-runbook.md).

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Retrieve VM's from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName" 

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal." 
            Add-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzureRMVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="test-the-example"></a>O exemplo de teste

O exemplo seguinte utiliza o Windows PowerShell para iniciar um runbook com um webhook. Qualquer idioma que pode efetuar um pedido HTTP pode utilizar um webhook; Windows PowerShell é utilizado aqui como exemplo.

O runbook está à espera de uma lista de máquinas virtuais formatado em JSON no corpo do pedido.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms

$response = Invoke-RestMethod -Method Post -Uri $uri -Body $body
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

O exemplo seguinte mostra o corpo do pedido que está disponível para o runbook no **RequestBody** propriedade **WebhookData**. Isto é formatado como JSON porque foi que o formato que foi incluído no corpo do pedido.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

A imagem seguinte mostra o pedido que está a ser enviado do Windows PowerShell e a resposta resultante. O id da tarefa é extraído da resposta e convertido numa cadeia.

![Botão de Webhooks](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Passos Seguintes

* Para saber como utilizar a automatização do Azure para executar ações em alertas do Azure, consulte [utilizar um alerta para acionar um runbook de automatização do Azure](automation-create-alert-triggered-runbook.md).
