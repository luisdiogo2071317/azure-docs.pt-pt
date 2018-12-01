---
title: Restaurar cópia de segurança nos recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Utilize a cópia de segurança periódica do Service Fabric e restaurar a funcionalidade para a restauração de dados de cópia de segurança de dados da sua aplicação.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 802F55B6-6575-4AE1-8A8E-C9B03512FF88
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 69604decab354368f336b85bfa1497671f0c3101
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730295"
---
#  <a name="restoring-backup-in-azure-service-fabric"></a>Restaurar cópia de segurança no Azure Service Fabric


Serviços com estado fiável e Reliable Actors no Service Fabric, podem manter um Estado mutável e autoritativo para além do pedido e resposta ou uma transação completa. Se um serviço com estado fica inativo por um longo tempo ou perde informações devido a um desastre, ele poderá ter de ser restaurados a mais recente aceitável cópia de segurança do seu estado para poder continuar a fornecer serviço depois volta e.

Por exemplo, o serviço pode querer seus dados de cópia de segurança para proteger contra os seguintes cenários:

- Em caso de perda permanente de um cluster do Service Fabric inteiro. **(Caso de recuperação após desastre - DR)**
- Perda permanente de a maioria das réplicas de uma partição de serviço. **(Caso de perda de dados)**
- Erros administrativos no qual o estado acidentalmente obtém excluído ou danificado. Por exemplo, um administrador com privilégios suficientes erroneamente elimina o serviço. **(Caso de perda de dados)**
- Bugs no serviço que danificar os dados. Por exemplo, danos nos dados podem ocorrer quando uma atualização de código do serviço iniciará a escrita de dados com falhas para uma coleção fiável. Nesse caso, o código e os dados podem ter a ser revertida para um estado anterior. **(Caso de danos em dados)**


## <a name="prerequisites"></a>Pré-requisitos
* Para acionar, restaurar o _serviço de análise de falhas (FAS)_ deve estar ativado para o cluster
* A cópia de segurança para restauro deve ter sido feita pelo _serviço de restaurar a cópia de segurança (BRS)_
* Só é possível acionar o restauro numa partição.

## <a name="triggering-restore"></a>O acionamento do restauro

O restauro pode ser de qualquer um dos seguintes cenários 
* Restauro de dados na eventualidade de _recuperação após desastre_ (DR)
* Restauro de dados na eventualidade de _Corrupção de dados / perda de dados_



### <a name="data-restore-in-the-event-of-disaster-recovery-dr"></a>Restauro de dados na eventualidade de _recuperação após desastre_ (DR)
Em caso de um cluster do Service Fabric todo perdido, os dados para as partições do serviço com estado fiável e Reliable Actors podem ser restaurados para um cluster alternativo. A cópia de segurança pretendida pode ser selecionada da enumeração dos [GetBackupAPI com detalhes de armazenamento de cópia de segurança](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). A enumeração de cópia de segurança pode ser para uma aplicação, serviço ou partição.

Permite a assumir o cluster perdido foi mencionado no cluster [cópia de segurança periódica do ativação de serviço com estado fiável e Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), que tinha o `SampleApp` implementado, em que a partição estava tendo dificuldade em política de cópia de segurança ativada e as cópias de segurança foram acontecendo no armazenamento do Azure. 


Execute a seguinte script do PowerShell para invocar a API REST para enumerar as cópias de segurança criadas para todas as partições dentro do `SampleApp` aplicação no cluster do Service Fabric perdido. A enumeração de API requer informações de armazenamento, onde são armazenadas as cópias de segurança de uma aplicação, para enumerar as cópias de segurança disponíveis. 

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Execute a saída de exemplo acima:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            : 
```



Para acionar o restauro, é necessário escolher a cópia de segurança pretendida. Permitir que a cópia de segurança pretendida para a recuperação de desastre (DR) atual ser a cópia de segurança seguinte

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Para a API de restauro, necessária para fornecer a __BackupId__ e __BackupLocation__ detalhes. A partição no cluster alternativo precisa escolhida, de acordo a [esquema de partição](service-fabric-concepts-partitioning.md#get-started-with-partitioning). É da responsabilidade do utilizador para escolher a partição de destino para restaurar a cópia de segurança do cluster alternativo de acordo com o esquema de partição perdido no cluster original.

Partem do princípio de que é o ID de partição no cluster alternativo `1c42c47f-439e-4e09-98b9-88b8f60800c6`, que é mapeado para o ID de partição de cluster original `974bd92a-b395-4631-8a7f-53bd4ae9cf22` comparando a chave de alta e baixa chave para _Ranged (UniformInt64Partition) de criação de partições_.

Para _com o nome de criação de partições_, o valor de nome é comparado para identificar a partição de destino em cluster alternativo.

O restauro é solicitado em relação a partição de cluster de cópia de segurança, pelos seguintes [restaurar API](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)

```powershell 
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 
O progresso da restauração pode ser [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress)

### <a name="data-restore-in-the-event-of-data-corruption--data-loss"></a>Restauro de dados na eventualidade de _Corrupção de dados / perda de dados_

Para o caso de _perda de dados_ ou _danos nos dados_ os dados, para as partições do serviço com estado fiável e Reliable Actors podem ser restaurados a qualquer uma das cópias de segurança escolhidas. Caso seguinte é a continuação do exemplo de como foi mencionado no [cópia de segurança periódica do ativação de serviço com estado fiável e Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), em que a partição tem uma política de cópia de segurança ativada e está a demorar cópia de segurança a uma frequência pretendida num armazenamento do Azure. 

A cópia de segurança pretendida está selecionada a partir da saída de [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). Neste cenário, a cópia de segurança é gerada a partir do mesmo cluster no passado.
Para acionar o restauro, é necessário selecionar a cópia de segurança pretendida na lista. Permitir que a nossa cópia de segurança pretendida para o atual _perda de dados_ / _danos nos dados_ ser a cópia de segurança seguinte

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Para a API de restauro, necessária para fornecer a __BackupId__ e __BackupLocation__ detalhes. Desde o cluster de cópia de segurança ativou o Service Fabric _serviço de restaurar a cópia de segurança (BRS)_ identifica a localização de armazenamento correta da política de cópia de segurança associada.

```powershell
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4', 
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

O progresso da restauração pode ser [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress)


## <a name="tracking-restore-progress"></a>Progresso de restauro de controlo

Uma partição de um serviço com estado fiável ou Reliable Actor aceita apenas um pedido de restauro por vez. Outro pedido pode ser aceites apenas quando o pedido de restauro atual foi concluído. Vários pedidos de restauro podem ser acionados em partições diferentes numa mesma altura.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
 
$restoreResponse = (ConvertFrom-Json $response.Content) 
$restoreResponse | Format-List
```

o andamento do pedido de restauro na seguinte ordem

1. __Aceite__ -o estado de restauro como _aceites_ indica que o pedido foi acionado com parâmetros de pedido correto.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
2. __InProgress__ -o estado de restauro como _InProgress_ indica que a partição vai sofrer um restauro com a cópia de segurança mencionada no pedido. A partição reportará _correção de perda_ estado.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. __Sucesso__/ __falha__/ __tempo limite__ -um pedido de restauro pode ser concluído em qualquer um dos seguintes Estados. Cada Estado possui os seguintes detalhes de significado e a resposta.
       
    * __Sucesso__ -o estado de restauro como _êxito_ indica o estado de partição é voltou a ter. A resposta irá fornecer RestoreEpoch e RestordLSN para a partição juntamente com a hora em UTC. 
    
        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```
        
    *. __Falha de__ -o estado de restauro como _falha_ indica a falha do pedido de restauro. A causa da falha será indicada no pedido.
        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    *. __Tempo limite__ -o estado de restauro como _tempo limite_ indica o pedido tem o tempo limite. Novo pedido de restauro com maior [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) é recomendado; predefinições do tempo limite é de 10 minutos. Recomenda-se para se certificar de que a partição não está em estado de perda de dados, antes de solicitar novamente o restauro.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="auto-restore"></a>Restauro automático

As partições para o serviço com estado fiável e Reliable Actors no cluster do Service Fabric podem ser configuradas para _auto restauro_. Ao criar a política de cópia de segurança, a política pode ter `AutoRestore` definido como _true_.  Habilitando _auto restauro_ para uma partição, restaura os dados da cópia de segurança mais recente se perda de dados é comunicada.
 
 [Ativação de restauro automático na política de cópia de segurança](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)


[Referência da API de RestorePartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
[referência da API de GetPartitionRestoreProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Passos Seguintes
- [Configuração de cópia de segurança periódica compreensão](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referência da REST API de restauro de cópia de segurança](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
