---
title: Cópia de segurança a pedido nos recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Utilize a cópia de segurança do Service Fabric e restaurar a funcionalidade de cópia de segurança de dados da sua aplicação numa base de necessidade.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 2e83dcb4ce42a0c08ce482e9ffbbf1ac18634fd5
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730312"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Cópia de segurança a pedido no Azure Service Fabric

Os dados de com monitoração de estado de Reliable services e Reliable Actors podem ser copiados para cenários de perda de dados de desastres ou do endereço.

Service Fabric é equipado com recursos para [cópia de segurança periódica de dados](service-fabric-backuprestoreservice-quickstart-azurecluster.md) e cópia de segurança de dados numa base de necessidade. Cópia de segurança a pedido é útil como ele protege contra _perda de dados_/_danos nos dados_ causado devido a alterações planeadas no serviço subjacente ou seu ambiente.

As funcionalidades de cópia de segurança a pedido é útil para capturar o estado dos serviços, antes de qualquer operação manualmente acionada relacionados com o serviço ou o ambiente de serviço. Como alterar os binários do serviço, ou seja, atualizar ou fazer downgrade do serviço; como ele terá os dados protegidos contra danos nos dados por bugs no código do aplicativo.

## <a name="triggering-on-demand-backup"></a>Acionar cópia de segurança a pedido

A cópia de segurança a pedido requer detalhes de armazenamento para carregar ficheiros de cópia de segurança. A cópia de segurança a pedido irá acontecer no armazenamento especificado na política de cópia de segurança periódica ou no armazenamento especificado no pedido de cópia de segurança a pedido.

### <a name="on-demand-backup-to-storage-specified-by-periodic-backup-policy"></a>Cópia de segurança a pedido para o armazenamento especificado pela política de cópia de segurança periódica

A partição de um serviço com estado fiável ou dos Reliable Actors pode ser pedida para adicionais na necessidade de cópia de segurança de base para o armazenamento especificado na política de cópia de segurança periódica. 

Caso seguinte é a continuação do exemplo de como foi mencionado no [cópia de segurança periódica do ativação de serviço com estado fiável e Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), em que a partição tem uma política de cópia de segurança ativada e está a demorar cópia de segurança a uma frequência pretendida no armazenamento do Azure.

A cópia de segurança a pedido para o ID de partição `974bd92a-b395-4631-8a7f-53bd4ae9cf22` pode ser acionada com [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API. 

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

O [progresso de cópia de segurança a pedido](service-fabric-backuprestoreservice-ondemand-backup.md#tracking-on-demand-backup-progress) pode ser controlado pelo [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API.

### <a name="on-demand-backup-to-specified-storage"></a>Cópia de segurança a pedido para o armazenamento especificado

A cópia de segurança a pedido pode ser pedida para uma partição de um serviço com estado fiável ou Reliable Actor juntamente com as informações de armazenamento. As informações de armazenamento devem ser fornecidas como parte do pedido de cópia de segurança a pedido.

A cópia de segurança a pedido para o ID de partição `974bd92a-b395-4631-8a7f-53bd4ae9cf22` pode ser acionada com [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API com informações de armazenamento do Azure, conforme mostrado abaixo.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

O [progresso de cópia de segurança a pedido](service-fabric-backuprestoreservice-ondemand-backup.md#tracking-on-demand-backup-progress) pode ser controlado pelo [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API.


## <a name="tracking-on-demand-backup-progress"></a>Acompanhando o progresso de cópia de segurança a pedido

Uma partição de um serviço com estado fiável ou Reliable Actor aceita somente uma solicitação de cópia de segurança a pedido, ao mesmo tempo. Outro pedido pode ser aceites apenas quando o pedido de cópia de segurança a pedido atual foi concluído. 

Vários pedidos de cópia de segurança a pedido podem ser acionados em partições diferentes numa mesma altura.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

Curso a pedido cópia de segurança da solicitação pode ser um dos seguintes Estados

* **Aceite** -a cópia de segurança foi iniciada na partição e está em curso.
    ```
    BackupState             : Accepted
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          : 
    EpochOfLastBackupRecord : 
    LsnOfLastBackupRecord   : 0
    FailureError            : 
    ```
    
* **Êxito / falha / tempo limite** -um pedido de cópia de segurança a pedido pode ser concluída em qualquer um dos seguintes Estados. Cada Estado possui os seguintes detalhes de significado e a resposta.

    * **Sucesso** -o estado de cópia de segurança como _êxito_ indica que o estado de partição é uma cópia de segurança com êxito. A resposta irá fornecer __BackupEpoch__ e __BackupLSN__ para a partição juntamente com a hora em UTC.
        ```
        BackupState             : Success
        TimeStampUtc            : 2018-11-21T20:00:01Z
        BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
        BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
        EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
        LsnOfLastBackupRecord   : 36
        FailureError            : 
        ```
        
    * **Falha de** -o estado de cópia de segurança como _falha_ indica a falha ocorreu durante a cópia de segurança do Estado da partição. A causa da falha será indicada na resposta.
        ```
        BackupState             : Failure
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
        ```
       
    * **Tempo limite** -o estado de cópia de segurança como _tempo limite_ indica que a cópia de segurança do Estado de partição não foi possível criar num dado período de tempo; o valor de tempo limite predefinido é de 10 minutos. Iniciar um novo pedido de cópia de segurança com maior [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) no pedido de cópia de segurança a pedido, é recomendado neste cenário.

        ```
        BackupState             : Timeout
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
        ```

## <a name="next-steps"></a>Passos Seguintes
- [Configuração de cópia de segurança periódica compreensão](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referência da REST API de restauro de cópia de segurança](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

