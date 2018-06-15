---
title: Cópia de segurança periódica e de restauro no Azure Service Fabric (pré-visualização) | Microsoft Docs
description: Utilize a cópia de segurança periódica do Service Fabric e restaurar a funcionalidade para proteger as suas aplicações de perda de dados.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAADBCAB-F0CF-4CBC-B663-4A6DCCB4DEE1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: hrushib
ms.openlocfilehash: cfbc0e6ca255bd005bb6e4cc381a9121347fe227
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206050"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric-preview"></a>Cópia de segurança periódica e de restauro no Service Fabric do Azure (pré-visualização)
> [!div class="op_single_selector"]
> * [Clusters no Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Clusters autónomos](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric é uma plataforma de sistemas distribuídos que facilita a desenvolver e gerir aplicações do micro-serviços fiável, distribuído, com base na nuvem. Permite a execução dos serviços de micro sem monitorização de estado e com monitorização de estado. Serviços com monitorização de estado podem manter o estado da mutável, autoritativo para além do pedido e resposta ou uma transação concluída. Se um serviço com estado fica inativo durante muito tempo ou perde informações devido a um desastre, pode ter de ser restaurados para algumas recente cópia de segurança de estado para poder continuar a fornecer serviço depois de este volta a funcionar.

Service Fabric replica o estado em vários nós para se certificar de que o serviço está altamente disponível. Mesmo se um nó no cluster falha, o serviço continua a estar disponíveis. No entanto, em certos casos, é ainda necessário para os serviço de dados fiável contra falhas mais ampla.
 
Por exemplo, o serviço poderá querer os dados de cópia de segurança para proteger contra os seguintes cenários:
- Em caso de perda permanente de um cluster de Service Fabric completo.
- Perda permanente da maioria das réplicas de uma partição de serviço
- Erros administrativos na qual o estado acidentalmente obtém eliminado ou danificado. Por exemplo, um administrador com privilégios suficientes indica elimina o serviço.
- Erros no serviço danificar os dados. Por exemplo, isto pode acontecer quando uma atualização de código do serviço começa a escrever dados defeituoso numa coleção fiável. Nesse caso, o código e os dados poderão ter de ser revertida para um estado anterior.
- Processamento de dados offline. Pode ser conveniente para que o processamento de dados de business intelligence, que ocorre em separado do serviço que gera dados offline.

O Service Fabric fornece uma API para o ponto no tempo integrada [cópia de segurança e restaurar](service-fabric-reliable-services-backup-restore.md). Os programadores de aplicações podem utilizar estas APIs para criar cópias de segurança do Estado do serviço periodicamente. Além disso, se pretenderem que os administradores de serviço acionar uma cópia de segurança a partir de fora de serviço a uma hora específica, como antes de atualizar a aplicação, os programadores necessário expor a cópia de segurança (e restaurar) como uma API do serviço. Manter as cópias de segurança é um custo superior esta adicional. Por exemplo, poderá demorar 5 cópias de segurança incrementais a cada meia hora, seguido de uma cópia de segurança completa. Após a cópia de segurança completa, pode eliminar as cópias de segurança incrementais anteriores. Esta abordagem requer códigos adicionais à esquerda para custos adicionais durante o desenvolvimento de aplicações.

Cópia de segurança dos dados de aplicação numa base periódica for básico necessário para gerir uma aplicação distribuída e guarding contra perda de dados ou prolongada perda de disponibilidade do serviço. O Service Fabric fornece uma cópia de segurança opcional e o serviço de restauro, o que lhe permite configurar a cópia de segurança periódica de monitorização de estado Reliable Services (incluindo serviços de Atores) sem ter de escrever qualquer código adicional. Também facilita a restaurar anteriormente efetuadas cópias de segurança. 

> [!NOTE]
> A funcionalidade de cópia de segurança e restauro periódica se encontra atualmente em **pré-visualização** e não é suportado para cargas de trabalho de produção. 
>

O Service Fabric fornece um conjunto de APIs para alcançar a seguinte funcionalidade relacionados periódica para cópia de segurança e restaurar a funcionalidade:

- Agendar periódica cópia de segurança dos serviços de monitorização de estado fiável e Reliable Actors com suporte para carregar as localizações de armazenamento de cópia de segurança para (externo). Localizações de armazenamento suportadas
    - Storage do Azure
    - Partilha de ficheiros (local)
- Enumerar as cópias de segurança
- Acione uma cópia de segurança ad-hoc de uma partição
- Restaurar uma partição utilizando a cópia de segurança anterior
- Suspender temporariamente as cópias de segurança
- Gestão de retenção de cópias de segurança (futuras)

## <a name="prerequisites"></a>Pré-requisitos
* Cluster do Service Fabric com recursos de infraestrutura versão 6.2 e superior. O cluster deve ser configurado no Windows Server. Consulte [artigo](service-fabric-cluster-creation-for-windows-server.md) para obter os passos transferir o pacote necessário.
* Certificado x. 509 para a encriptação de segredos necessárias para ligar ao armazenamento para armazenar as cópias de segurança. Consulte [artigo](service-fabric-windows-cluster-x509-security.md) saber como adquirir ou para criar um certificado x. 509 autoassinado.
* Aplicação de Service Fabric fiável de monitorização de estado criada com o SDK de Service Fabric versão 3.0 ou superior. Para aplicações visar o .net Core 2.0, que aplicações devem ser criadas utilizando o SDK de Service Fabric versão 3.1 ou posterior.

## <a name="enabling-backup-and-restore-service"></a>Ativar o serviço de cópia de segurança e restauro
Primeiro tem de ativar o _cópia de segurança e restaurar o serviço_ no seu cluster. Obter o modelo para o cluster que pretende implementar. Pode utilizar o [modelos de exemplo](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples). Ativar o _cópia de segurança e restaurar o serviço_ com os seguintes passos:

1. Verifique se o `apiversion` está definido como `10-2017` na configuração do cluster de ficheiros e, se não estiver, atualizá-lo conforme mostrado no seguinte fragmento:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Ativar agora o _cópia de segurança e restaurar o serviço_ adicionando o seguinte `addonFeatures` secção em `properties` secção conforme mostrado no seguinte fragmento: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```

3. Configure o certificado x. 509 para a encriptação de credenciais. Isto é importante para se certificar de que as credenciais fornecidas, se existir, para ligar ao armazenamento são encriptados antes de a persistência. Configurar o certificado de encriptação adicionando o seguinte `BackupRestoreService` secção em `fabricSettings` secção conforme mostrado no seguinte fragmento: 

    ```json
    "properties": {
        ...
        "addonFeatures": ["BackupRestoreService"],
        "fabricSettings": [{
            "name": "BackupRestoreService",
            "parameters":  [{
                "name": "SecretEncryptionCertThumbprint",
                "value": "[Thumbprint]"
            }]
        }
        ...
    }
    ```

4. Depois de ter atualizado o ficheiro de configuração de cluster com as alterações anteriores, aplicá-las e permitir que a implementação/atualização concluída. Depois de concluído, o _cópia de segurança e restaurar o serviço_ entrar em execução no seu cluster. O Uri deste serviço é `fabric:/System/BackupRestoreService` e o serviço pode estar localizado na secção de serviço do sistema no Explorador do Service Fabric. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Ativar a cópia de segurança periódica para serviço de monitorização de estado fiável e Reliable Actors
Vamos guiá-lo pelos passos para ativar a cópia de segurança periódica para serviço de monitorização de estado fiável e Reliable Actors. Estes passos partem do princípio
- Se o cluster está configurado com _cópia de segurança e restaurar o serviço_.
- Um serviço fiável de monitorização de estado é implementado num cluster. Com o objetivo deste guia de início rápido, o Uri da aplicação é `fabric:/SampleApp` e o Uri do serviço de monitorização de estado fiável pertencentes a esta aplicação é `fabric:/SampleApp/MyStatefulService`. Este serviço é implementado com única partição e o ID de partição é `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`.  

### <a name="create-backup-policy"></a>Criar política de cópia de segurança

Primeiro passo consiste em criar política de cópia de segurança que descreve a agenda de cópia de segurança, armazenamento de destino para dados de cópia de segurança, nome da política e cópias de segurança incrementais máximas permitida antes de acionar a cópia de segurança completa. 

Para armazenamento de cópia de segurança, criar partilha de ficheiros e conceda acesso de ReadWrite para esta partilha de ficheiros para todas as máquinas de nó de recursos de infraestrutura de serviço. Este exemplo assume a partilha com o nome `BackupStore` está presente no `StorageServer`.

Execute o seguinte script do PowerShell para invocar a API de REST necessários para criar nova política.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

### <a name="enable-periodic-backup"></a>Ativar a cópia de segurança periódica
Depois de definir a política para satisfazer os requisitos de proteção de dados da aplicação, a política de cópia de segurança deve ser associada à aplicação. Dependendo do requisito, a política de cópia de segurança pode ser associada a uma aplicação, serviço ou uma partição.

Executar o seguinte script do PowerShell para invocar a API de REST necessárias para associar a política de cópia de segurança com o nome `BackupPolicy1` criado no passo com a aplicação `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Certifique-se de que as cópias de segurança periódicas estão a funcionar

Depois de ativar a cópia de segurança para a aplicação, todas as partições que pertencem com monitorização de estado Reliable services e Reliable Actors sob a aplicação irão iniciar a obtenção de cópia de segurança periodicamente de acordo com a política de cópia de segurança associadas. 

![Eventos de estado de funcionamento BackedUp de partição][0]

### <a name="list-backups"></a>Cópias de segurança de lista

As cópias de segurança associadas a todas as partições que pertencem a serviços com monitorização de estado fiável e Reliable Actors da aplicação podem ser enumeradas utilizando _GetBackups_ API. Dependendo do requisito, as cópias de segurança podem ser enumeradas para uma partição, aplicação ou serviço.

Executar o seguinte script do PowerShell para invocar a API de HTTP ao enumerar as cópias de segurança criadas para todas as partições dentro de `SampleApp` aplicação.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.2-preview"

$response = Invoke-WebRequest -Uri $url -Method Get

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Execute a saída de exemplo para o procedimento acima:

```
BackupId                : d7e4038e-2c46-47c6-9549-10698766e714
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.39.40.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2058
CreationTimeUtc         : 2018-04-01T19:39:40Z
FailureError            : 

BackupId                : 8c21398a-2141-4133-b4d7-e1a35f0d7aac
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.54.38.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2237
CreationTimeUtc         : 2018-04-01T19:54:38Z
FailureError            : 

BackupId                : fc75bd4c-798c-4c9a-beee-e725321f73b2
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 20.09.44.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2437
CreationTimeUtc         : 2018-04-01T20:09:44Z
FailureError            : 
```

## <a name="preview-limitation-caveats"></a>Limitação de pré-visualização / avisos
- Não existem recursos de infraestrutura de serviço criado nos cmdlets do PowerShell.
- Sem suporte para a CLI de recursos de infraestrutura de serviço.
- Não existe suporte para remoção de cópia de segurança automatizada. Requer manual limpeza de cópias de segurança.
- Não existe suporte para o Service Fabric clusters no Linux.

## <a name="next-steps"></a>Próximos Passos
- [Referência da REST API de restauro de cópia de segurança](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent.png

