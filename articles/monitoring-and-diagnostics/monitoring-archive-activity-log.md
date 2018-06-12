---
title: Arquivar o registo de atividade do Azure
description: O registo de atividade do Azure para a retenção de longa duração numa conta de armazenamento de arquivo.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: johnkem
ms.component: activitylog
ms.openlocfilehash: 508b2f615819f20a717065d8fff25beff64957d5
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263434"
---
# <a name="archive-the-azure-activity-log"></a>Arquivar o registo de atividade do Azure
Neste artigo, mostramos como pode utilizar o portal do Azure, o Cmdlets do PowerShell ou a CLI de várias plataformas para arquivar a [ **registo de atividade do Azure** ](monitoring-overview-activity-logs.md) numa conta do storage. Esta opção é útil se gostaria de manter o registo de atividade mais de 90 dias (com controlo total sobre a política de retenção) para cópia de segurança, auditoria ou análise estático. Se pretender manter os eventos durante 90 dias ou menos não terá de configurar o arquivo para uma conta de armazenamento, uma vez que os eventos de registo de atividade são retidos na plataforma do Azure para 90 dias sem ativar o arquivo.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, terá de [criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) ao qual pode arquivar o registo de atividade. Recomendamos vivamente que utilize uma conta de armazenamento existente que tenha outros não monitorização dados armazenados na mesma forma que melhor pode controlar o acesso a dados de monitorização. No entanto, se também são arquivar os registos de diagnóstico e de métricas para uma conta de armazenamento, poderá fazer sentido utilizar essa conta do storage para o registo de atividade, bem como para manter os dados de monitorização de todos os numa localização central. A conta de armazenamento que utiliza tem de ser uma conta de armazenamento de objetivo geral, não uma conta do blob storage. A conta de armazenamento não tem de estar na mesma subscrição que a subscrição emitir os registos, desde que o utilizador que configura a definição possui acesso RBAC adequado para ambas as subscrições.

> [!NOTE]
>  Atualmente não pode arquivar dados para o armazenamento de um conta que atrás de uma rede virtual protegida.

## <a name="log-profile"></a>Perfil de registo
Para arquivar o registo de atividade, utilizando qualquer um dos métodos abaixo, definir o **registo perfil** para uma subscrição. O perfil de registo define o tipo de eventos que são armazenados ou transmissão em fluxo e as saídas — hub de conta e/ou evento de armazenamento. Também define a política de retenção (número de dias a manter) para os eventos armazenados numa conta do storage. Se a política de retenção é definida para zero, eventos são armazenados indefinidamente. Caso contrário, isto pode ser definido como qualquer valor entre 1 e 2147483647. As políticas de retenção são aplicada por-dia, no fim do dia (UTC), registos a partir do dia em que é agora a retenção política será eliminada. Por exemplo, se tiver uma política de retenção de um dia, no início do dia de hoje os registos de ontem de antes do dia seriam eliminados. O processo de eliminação é iniciada à meia-noite UTC, mas tenha em atenção que pode demorar até 24 horas para os registos a eliminar da sua conta de armazenamento. [Pode ler mais sobre o registo perfis aqui](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>O registo de atividade com o portal de arquivo
1. No portal, clique o **registo de atividade** ligação no painel de navegação esquerda. Se não vir uma ligação para o registo de atividade, clique em de **todos os serviços** ligar pela primeira vez.
   
    ![Navegue para o painel de registo de atividade](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Na parte superior do painel, clique em **exportar**.
   
    ![Clique no botão de exportação](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. No painel que aparece, selecione a caixa para **exportar para uma conta de armazenamento** e selecione uma conta de armazenamento.
   
    ![Defina uma conta de armazenamento](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Ao utilizar o controlo de deslize ou a caixa de texto, defina um número de dias para o qual os eventos de registo de atividades devem ser mantidos na sua conta de armazenamento. Se preferir para que os dados persistentes na conta de armazenamento indefinidamente, defina este número para zero.
5. Clique em **Guardar**.

## <a name="archive-the-activity-log-via-powershell"></a>Arquivar o registo de atividade através do PowerShell

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your storage account belongs to>"
   $storageAccountName = "<your storage account name>"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -StorageAccountId $storageAccountId
   ```

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| StorageAccountId |Sim |ID de recurso da conta do Storage para o qual os registos de atividade deve ser guardados. |
| Localizações |Sim |Lista separada por vírgulas das regiões para as quais pretende recolher eventos de registo de atividade. Pode ver uma lista de todas as regiões para a sua subscrição utilizando `(Get-AzureRmLocation).Location`. |
| retentionInDays |Não |Número de dias para que eventos devem ser mantidos, entre 1 e 2147483647. Um valor de zero armazena os registos indefinidamente (indefinidamente). |
| Categorias |Não |Lista separada por vírgulas das categorias de evento que deve ser recolhidas. Os valores possíveis são escrita, a eliminação e a ação.  Se não for indicado, em seguida, todos os valores possíveis são considerados |

## <a name="archive-the-activity-log-via-cli"></a>Arquivar o registo de atividade através da CLI

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --storage-account-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>"
   ```

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| name |Sim |Nome do perfil do registo. |
| storage-account-id |Sim |ID de recurso da conta do Storage para o qual os registos de atividade deve ser guardados. |
| localizações |Sim |Lista de valores separados por espaço das regiões para as quais pretende recolher eventos de registo de atividade. Pode ver uma lista de todas as regiões para a sua subscrição utilizando `az account list-locations --query [].name`. |
| dias |Sim |Número de dias para que eventos devem ser mantidos, entre 1 e 2147483647. Um valor de zero irá armazenar os registos indefinidamente (indefinidamente).  Se zero, em seguida, o parâmetro ativado deve ser definido como true. |
|enabled | Sim |VERDADEIRO ou FALSO.  Utilizado para ativar ou desativar a política de retenção.  Se for VERDADEIRO, o parâmetro de dias tem de ser um valor maior que 0.
| categorias |Sim |Lista de valores separados por espaço das categorias de evento que deve ser recolhidas. Os valores possíveis são escrita, a eliminação e a ação. |

## <a name="storage-schema-of-the-activity-log"></a>Esquema de armazenamento do registo de atividade
Assim que tiver configurado a arquivo, um contentor de armazenamento será criado na conta de armazenamento, assim que ocorre um evento de registo de atividade. Os blobs no contentor siga o mesmo formato entre o registo de atividade e os registos de diagnóstico. A estrutura destas blobs é:

> insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
> 
> 

Por exemplo, poderá ser um nome de blob:

> insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Cada blob PT1H.json contém um blob JSON de eventos que ocorreram dentro da hora especificada no URL do blob (por exemplo, h = 12). Durante a hora presente, os eventos são acrescentados ao ficheiro PT1H.json à medida que ocorrem. O valor de minuto (m = 00) é sempre 00, uma vez que os eventos de registo de atividade são divididos em blobs individuais por hora.

No ficheiro PT1H.json cada evento está armazenado na matriz "registos", segue este formato:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Nome do elemento | Descrição |
| --- | --- |
| hora |Timestamp quando o evento foi gerado pelo processamento do pedido correspondente o evento de serviço do Azure. |
| resourceId |ID de recurso do recurso afetado. |
| operationName |Nome da operação. |
| categoria |Categoria da ação, por exemplo. Escrita, leitura, a ação. |
| resultType |O tipo de resultado, por exemplo. Início de êxito, falha, |
| resultSignature |Depende do tipo de recurso. |
| durationMs |Duração da operação em milissegundos |
| callerIpAddress |Endereço IP do utilizador que efetuou a operação, afirmação UPN ou afirmação SPN com base na disponibilidade. |
| correlationId |Normalmente, um GUID no formato de cadeia. Eventos que partilham um correlationId pertencem à mesma ação de uber. |
| identidade |Blob JSON que descrevem a autorização e afirmações. |
| Autorização |Blob de propriedades do RBAC do evento. Normalmente, inclui as propriedades "action", "função" e "âmbito". |
| nível |Nível do evento. Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" e "Verboso" |
| localização |Região na qual ocorreu a localização (ou global). |
| propriedades |Conjunto de `<Key, Value>` pares (ou seja, dicionário) que descrevem os detalhes do evento. |

> [!NOTE]
> As propriedades e a utilização dessas propriedades podem variar consoante o recurso.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
* [Transferir blobs para análise](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Fluxo de registo de atividade para os Event Hubs](monitoring-stream-activity-logs-event-hubs.md)
* [Leia mais sobre o registo de atividade](monitoring-overview-activity-logs.md)

