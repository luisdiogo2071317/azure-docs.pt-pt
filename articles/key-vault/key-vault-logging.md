---
title: Registo do Cofre de chaves do Azure - Cofre de chaves do Azure | Documentos da Microsoft
description: Utilize este tutorial para ajudá-lo a começar com o registo do Cofre de Chaves do Azure.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 61f277eda721c1490d9f4b354442718558830fe7
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300204"
---
# <a name="azure-key-vault-logging"></a>Registo do Cofre de Chaves do Azure

O Cofre de Chaves do Azure chave está disponível na maior parte das regiões. Para obter mais informações, consulte a [página de preços do Cofre de Chaves](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Introdução

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Depois de ter criado um ou mais cofres de chaves, é provável que queira controlar como e quando são acedidos os seus cofres de chaves, e por quem. Esta ação é possível ao ativar o registo do Cofre de Chaves, que guarda as informações numa conta de armazenamento do Azure que indicar. É automaticamente criado um novo contentor designado **insights-logs-auditevent** na sua conta de armazenamento especificada, sendo que também pode utilizar essa mesma conta de armazenamento para recolher registos para vários cofres de chaves.

Pode aceder às suas informações de registo, no máximo, 10 minutos depois da operação do cofre de chaves. Na maioria dos casos, o processo será ainda mais rápido.  Cabe-lhe gerir os seus registos na sua conta de armazenamento:

* Utilize métodos padrão de controlo de acesso do Azure para proteger os seus registos, restringindo o seu acesso.
* Elimine os registos que já não pretende manter na sua conta de armazenamento.

Utilize este tutorial para o ajudar a começar com o registo do Cofre de Chaves do Azure, para criar a sua conta de armazenamento, ativar o registo e interpretar as informações de registo recolhidas.  

> [!NOTE]
> Este tutorial não inclui instruções sobre a criação dos cofres de chaves, chaves ou segredos. Para obter estas informações, consulte [o que é o Azure Key Vault?](key-vault-overview.md). Ou, para obter instruções sobre a Interface de Linha de Comandos de várias plataformas, veja o [tutorial equivalente](key-vault-manage-with-cli2.md).
>
> Este artigo fornece instruções de Azure PowerShell para atualizar o registo de diagnósticos. No entanto, o mesmo pode ser ativado ao utilizar o Azure Monitor no portal do Azure no **registos de diagnóstico** secção. 
>
>

Para obter informações gerais sobre o Cofre de Chaves do Azure, consulte o artigo [O que é o Cofre de Chaves do Azure?](key-vault-whatis.md)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, deve ter o seguinte:

* Um cofre de chaves que tiver utilizado.  
* O Azure PowerShell, **a versão mínima do 1.0.0**. Para instalar o Azure PowerShell e associá-lo à sua subscrição do Azure, consulte o artigo [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview). Se já tiver instalado o Azure PowerShell e não souber a versão, a partir da consola do Azure PowerShell, introduza `$PSVersionTable.PSVersion`.  
* Armazenamento suficiente no Azure para os seus registos do Cofre de Chaves.

## <a id="connect"></a>Ligar às suas subscrições

Abra uma sessão no Azure PowerShell e inicie sessão na sua conta do Azure com o seguinte comando:  

```PowerShell
Connect-AzAccount
```

Na janela pop-up do browser, introduza o seu nome de utilizador da conta do Azure e a palavra-passe. O Azure PowerShell irá obter todas as subscrições associadas a esta conta e, por defeito, irá utilizar a primeira.

Se tiver várias subscrições, poderá ter de especificar uma subscrição utilizada para criar o seu Cofre de Chaves do Azure. Escreva o seguinte para ver as subscrições da sua conta:

```PowerShell
    Get-AzSubscription
```

Em seguida, para especificar a subscrição associada ao seu cofre de chaves que irá registar, introduza:

```PowerShell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Isto é um passo importante e especialmente útil se tiver várias subscrições associadas à sua conta. Poderá ser apresentado um erro ao registar o Microsoft.Insights se este passo for ignorado.
>   
>

Para mais informações sobre como configurar o PowerShell, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

## <a id="storage"></a>Criar uma nova conta de armazenamento para os seus registos

Apesar de poder utilizar uma conta de armazenamento existente para os seus registos, iremos criar uma nova conta de armazenamento dedicada aos registos do seu Cofre de Chaves. Para sua comodidade, e já que poderemos ter de especificar estas informações mais tarde, iremos guardar os detalhes numa variável designada **sa**.

Para facilitar ainda mais a gestão, também iremos utilizar o mesmo grupo de recursos que contém o nosso cofre de chaves. A partir do [tutorial de introdução](key-vault-overview.md), este grupo de recursos será designado **ContosoResourceGroup** e continuaremos a utilizar a Ásia Oriental como localização. Substitua estes valores pelos seus próprios valores, conforme aplicável:

```PowerShell
 $sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'
```

> [!NOTE]
> Se optar por utilizar uma conta de armazenamento existente, terá de utilizar a mesma subscrição do que o seu Cofre de Chaves e terá de utilizar o modelo de implementação do Resource Manager em vez do modelo de implementação Clássico.
>
>

## <a id="identify"></a>Identifique o cofre de chaves para os seus registos

No nosso tutorial de introdução, o nome do nosso cofre de chaves era **ContosoKeyVault**. Por isso, continuaremos a utilizar esse nome e a guardar os detalhes numa variável designada **kv**:

```PowerShell
$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'
```

## <a id="enable"></a>Ativar registo

Para ativar o registo para o Key Vault, vamos utilizar o cmdlet Set-AzDiagnosticSetting, juntamente com as variáveis criadas para nossa conta de armazenamento e o nosso Cofre de chaves. Também vamos definir o **-ativado** sinalizador para **$true** e definir a categoria para AuditEvent (a única categoria para o registo do Cofre de chaves):

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

A saída terá o seguinte aspeto:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0

Isto confirma que o registo está agora ativado para o seu cofre de chaves e que as informações serão guardadas na sua conta de armazenamento.

Opcionalmente, pode também definir a política de retenção dos seus registos de forma a que os registos mais antigos sejam eliminados automaticamente. Por exemplo, defina a política de retenção utilizando o sinalizador **- RetentionEnabled** para **$true** e defina o parâmetro **- RetentionInDays** **90** para que os registos com mais de 90 dias sejam eliminados automaticamente.

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

O que é registado:

* Todos os pedidos de REST API autenticados são registados, o que inclui os pedidos falhados resultantes de erros de permissões de acesso, erros do sistema ou pedidos incorretos.
* Operações no próprio cofre de chaves, o que inclui a criação, eliminação, definição das políticas de acesso ao cofre de chaves e a atualização dos atributos do cofre de chaves, como etiquetas.
* Operações sobre chaves e segredos no cofre de chaves, que incluem a criação, eliminação, alteração ou eliminação dessas chaves ou segredos; operações como assinar, verificar, encriptar, desencriptar, moldar e desenrolar chaves, obter segredos, listar segredos e as suas versões.
* Pedidos não autenticados que resultam numa resposta 401. Por exemplo, pedidos que não têm um token de portador ou pedidos incorretamente formulados ou expirados ou com um token inválido.  

## <a id="access"></a>Aceder aos seus registos

Os registos do cofre de chaves são guardados no contentor **insights-logs-auditevent** na conta de armazenamento que indicou. Para listar todos os blobs neste contentor, escreva:

Primeiro, crie uma variável para o nome do contentor. Isso será usado em todo o resto da passagem.

```PowerShell
$container = 'insights-logs-auditevent'
```

Para listar todos os blobs neste contentor, escreva:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

O resultado será semelhante ao seguinte:

```
Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent

Name

- - -
resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json
```

Como pode neste resultado, os blobs seguem uma convenção de nomenclatura: **resourceId =<ARM resource ID>/y =<year>. Note /m =<month>/d =<day of month>/h =<hour>. Note /m =<minute>/filename.json**

Os valores data e hora utilizam o UTC.

Uma vez que a mesma conta de armazenamento pode ser utilizada para recolher registos para vários recursos, o ID do recurso completo no nome do blob é útil para aceder ou transferir os blobs de que precisa. Mas antes de podermos fazer isso, teremos primeiro de saber como transferir todos os blobs.

Primeiro, crie uma pasta para transferir os blobs. Por exemplo:

```PowerShell 
New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force
```

Em seguida, obtenha uma lista de todos os blobs:  

```PowerShell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Encaminhe esta lista por meio de 'Get-AzStorageBlobContent"para transferir os blobs para a nossa pasta de destino:

```PowerShell
$blobs | Get-AzStorageBlobContent -Destination C:\Users\username\ContosoKeyVaultLogs'
```

Quando executar este segundo comando, o delimitador **/** dos nomes blob cria uma estrutura de pasta completa sob a pasta de destino, sendo essa estrutura utilizada para transferir e armazenar os blobs como ficheiros.

Para transferir seletivamente blobs, utilize carateres universais. Por exemplo:

* Se tiver vários cofres de chaves e pretender transferir registos apenas para um cofre de chaves designado CONTOSOKEYVAULT3:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
```

* Se tiver vários grupos de recursos e pretender transferir os registos para apenas um grupo de recursos, utilize `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
```

* Se pretende transferir todos os registos do mês de janeiro de 2016, utilize `-Blob '*/year=2016/m=01/*'`:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
```

Agora, está pronto para começar a procurar o conteúdo dos registos. Mas antes dessa, dois parâmetros adicionais para Get-AzDiagnosticSetting que poderá precisar de saber:

* Para consultar o estado das definições de diagnóstico para o recurso do seu cofre de chaves: `Get-AzDiagnosticSetting -ResourceId $kv.ResourceId`
* Para desativar o registo do seu cofre de chaves: `Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Category AuditEvent`

## <a id="interpret"></a>Interpretar os registos do seu Cofre de Chaves

Os blobs individuais são armazenadas como texto, formatados como um blob JSON. A executar

```PowerShell
Get-AzKeyVault -VaultName 'contosokeyvault'`
```

irá devolver uma entrada de registo semelhante ao mostrado abaixo:

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

A tabela seguinte lista os nomes dos campos e descrições.

| Nome do campo | Descrição |
| --- | --- |
| hora |Data e hora (UTC). |
| resourceId |ID do Recurso do Azure Resource Manager Para registos do Cofre de Chaves, este é sempre o ID de recurso do Cofre de Chaves. |
| operationName |Nome da operação, conforme documentada na tabela seguinte. |
| operationVersion |Trata-se da versão da API REST solicitada pelo cliente. |
| categoria |No caso dos registos do Cofre de Chaves, AuditEvent é o único valor disponível. |
| resultType |Resultado do pedido de API REST. |
| resultSignature |Estado de HTTP. |
| resultDescription |Descrição adicional sobre o resultado, quando disponível. |
| durationMs |Tempo necessário para o processamento do pedido de API REST, em milissegundos. Não inclui a latência de rede, assim, o tempo que medir do lado do cliente poderá não corresponder a este período de tempo. |
| callerIpAddress |Endereço IP do cliente que efetuou o pedido. |
| correlationId |Um GUID opcional que o cliente pode passar para correlacionar os registos do lado do cliente com os registos do lado do serviço (Cofre de Chaves). |
| identidade |Identidade do token apresentado aquando do pedido de API REST. Trata-se geralmente de um "utilizador", um "principal de serviço" ou uma combinação "utilizador + appId", como no caso de um pedido resultante de um cmdlet do Azure PowerShell. |
| propriedades |Este campo irá conter diversas informações com base na operação (oeprationName). Na maioria dos casos, contém informações de cliente (cadeia agente transmitida pelo cliente), o URI de pedido de REST API exata e o código de estado HTTP. Além disso, quando um objeto é devolvido como resultado de um pedido (por exemplo, KeyCreate ou VaultGet), também irá conter a Chave do URI (como "id"), o Cofre do URI ou o Segredo do URI. |

Os valores do campo **operationName** encontram-se no formato ObjectVerb. Por exemplo:

* Todas as operações do cofre de chaves têm o formato 'Vault`<action>`', tal como `VaultGet` e `VaultCreate`.
* Todas as operações de chaves têm o formato 'Key`<action>`', tal como `KeySign` e `KeyList`.
* Todas as operações de segredo têm o formato 'Secret`<action>`', tal como `SecretGet` e `SecretListVersions`.

A tabela seguinte lista o operationName e o comando API REST correspondente.

| operationName | Comando API REST |
| --- | --- |
| Authentication |Através do ponto final do Azure Active Directory |
| VaultGet |[Obter informações sobre um cofre de chaves](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| VaultPut |[Criar ou atualizar um cofre de chaves](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultDelete |[Eliminar um cofre de chaves](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| VaultPatch |[Atualizar um cofre de chaves](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultList |[Lista todos os cofres de chaves num grupo de recursos](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| KeyCreate |[Criar uma chave](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| KeyGet |[Obter informações sobre uma chave](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| KeyImport |[Importar uma chave para um cofre](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| KeyBackup |[Fazer uma cópia de segurança de uma chave](https://msdn.microsoft.com/library/azure/dn878058.aspx). |
| KeyDelete |[Eliminar uma chave](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| KeyRestore |[Restaurar uma chave](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| KeySign |[Assinar com uma chave](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| KeyVerify |[Verificar com uma chave](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| KeyWrap |[Moldar uma chave](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| KeyUnwrap |[Desenrolar uma chave](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| KeyEncrypt |[Encriptar com uma chave](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| KeyDecrypt |[Desencriptar com uma chave](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| KeyUpdate |[Atualizar uma chave](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| KeyList |[Lista as chaves num cofre](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| KeyListVersions |[Lista as versões de uma chave](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| SecretSet |[Criar um segredo](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| SecretGet |[Obter um segredo](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| SecretUpdate |[Atualizar um segredo](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| SecretDelete |[Eliminar um segredo](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| SecretList |[Lista os segredos num cofre](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| SecretListVersions |[Lista as versões de um segredo](https://msdn.microsoft.com/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Utilizar o Log Analytics

Pode utilizar a solução do Azure Key Vault no Log Analytics para rever os registos AuditEvent do Azure Key Vault. Para obter mais informações, incluindo informação sobre configuração, veja [Azure Key Vault solution in Log Analytics (Solução Cofre de Chaves do Azure no Log Analytics)](../azure-monitor/insights/azure-key-vault.md). Este artigo também contém instruções caso precise de migrar a partir da solução Key Vault antiga que foi oferecida durante a pré-visualização do Log Analytics, onde primeiro encaminhou os registos para uma conta de Armazenamento do Azure e configurou o Log Analytics para ler a partir daí.

## <a id="next"></a>Passos seguintes

Para obter um tutorial que utiliza o Azure Key Vault num aplicativo web do .NET, consulte [utilização do Azure Key Vault a partir de uma aplicação Web](tutorial-net-create-vault-azure-web-app.md).

Para as referências de programação, consulte o [Guia para programadores do Cofre de Chaves do Azure](key-vault-developers-guide.md).

Para obter uma lista dos cmdlets do Azure PowerShell 1.0 para o Cofre de Chaves do Azure, consulte o artigo [Cmdlets do Cofre de Chaves do Azure](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).

Para um tutorial de rotação da chave e auditoria do registo com o Cofre de Chaves do Azure, veja [How to setup Key Vault with end to end key rotation and auditing (Como configurar o Cofre de Chaves com auditoria e rotação de chave ponto a ponto)](key-vault-key-rotation-log-monitoring.md).
