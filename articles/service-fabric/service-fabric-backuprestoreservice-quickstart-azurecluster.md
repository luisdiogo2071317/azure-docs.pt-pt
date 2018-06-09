---
title: Cópia de segurança periódica e de restauro no Azure Service Fabric (pré-visualização) | Microsoft Docs
description: Utilize a cópia de segurança periódica do Service Fabric e restaurar a funcionalidade para proteger as suas aplicações de perda de dados.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: timlt
editor: hrushib
ms.assetid: FAA58600-897E-4CEE-9D1C-93FACF98AD1C
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: hrushib
ms.openlocfilehash: 73b5356f63199c7530fe5eef0c4b4b7ee617ff5f
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236125"
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
* Cluster do Service Fabric com recursos de infraestrutura versão 6.2 e superior. O cluster deve ser configurado no Windows Server. Consulte [artigo](service-fabric-cluster-creation-via-arm.md) para obter passos para criar recursos de infraestrutura do serviço de cluster utilizando o modelo de recursos do Azure.
* Certificado x. 509 para a encriptação de segredos necessárias para ligar ao armazenamento para armazenar as cópias de segurança. Consulte [artigo](service-fabric-cluster-creation-via-arm.md) saber como obter ou criar um certificado x. 509.
* Aplicação de Service Fabric fiável de monitorização de estado criada com o SDK de Service Fabric versão 3.0 ou superior. Para aplicações visar o .net Core 2.0, que aplicações devem ser criadas utilizando o SDK de Service Fabric versão 3.1 ou posterior.
* Crie a conta de armazenamento do Azure para armazenar cópias de segurança da aplicação.

## <a name="enabling-backup-and-restore-service"></a>Ativar o serviço de cópia de segurança e restauro
Primeiro tem de ativar o _cópia de segurança e restaurar o serviço_ no seu cluster. Obter o modelo para o cluster que pretende implementar. Pode utilizar o [modelos de exemplo](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) ou criar um modelo do Resource Manager. Ativar o _cópia de segurança e restaurar o serviço_ com os seguintes passos:

1. Verifique se o `apiversion` está definido como **`2018-02-01`** para o `Microsoft.ServiceFabric/clusters` recurso e se não estiver, atualizá-lo conforme mostrado no seguinte fragmento:

    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Ativar agora o _cópia de segurança e restaurar o serviço_ adicionando o seguinte `addonFeatures` secção em `properties` secção conforme mostrado no seguinte fragmento: 

    ```json
        "properties": {
            ...
            "addonFeatures":  ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```
3. Configure o certificado x. 509 para a encriptação de credenciais. Isto é importante certificar-se de que as credenciais fornecidas para ligar ao armazenamento sejam encriptadas antes de a persistência. Configurar o certificado de encriptação adicionando o seguinte `BackupRestoreService` secção em `fabricSettings` secção conforme mostrado no seguinte fragmento: 

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

4. Depois de ter atualizado o modelo de cluster com as alterações anteriores, aplicá-las e permitir que a implementação/atualização concluída. Depois de concluído, o _cópia de segurança e restaurar o serviço_ entrar em execução no seu cluster. O Uri deste serviço é `fabric:/System/BackupRestoreService` e o serviço pode estar localizado na secção de serviço do sistema no Explorador do Service Fabric. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Ativar a cópia de segurança periódica para serviço de monitorização de estado fiável e Reliable Actors
Vamos guiá-lo pelos passos para ativar a cópia de segurança periódica para serviço de monitorização de estado fiável e Reliable Actors. Estes passos partem do princípio
- Se o cluster está configurado com a segurança de x. 509 com _cópia de segurança e restaurar o serviço_.
- Um serviço fiável de monitorização de estado é implementado num cluster. Com o objetivo deste guia de início rápido, o Uri da aplicação é `fabric:/SampleApp` e o Uri do serviço de monitorização de estado fiável pertencentes a esta aplicação é `fabric:/SampleApp/MyStatefulService`. Este serviço é implementado com única partição e o ID de partição é `974bd92a-b395-4631-8a7f-53bd4ae9cf22`.
- O certificado de cliente com a função de administrador está instalado no _meu_ (_pessoais_) armazenar o nome do _CurrentUser_ na máquina a partir de onde abaixo da localização do arquivo de certificados scripts serão invocados. Este exemplo utiliza `1b7ebe2174649c45474a4819dafae956712c31d3` como thumbprint deste certificado. Para obter mais informações sobre certificados de cliente, consulte [controlo de acesso baseado em funções para clientes de Service Fabric](service-fabric-cluster-security-roles.md).

### <a name="create-backup-policy"></a>Criar política de cópia de segurança

Primeiro passo consiste em criar política de cópia de segurança que descreve a agenda de cópia de segurança, armazenamento de destino para dados de cópia de segurança, nome da política e cópias de segurança incrementais máximas permitida antes de acionar a cópia de segurança completa. 

Para armazenamento de cópia de segurança, utilize o armazenamento de Azure conta criada acima. Contentor `backup-container` está configurado para armazenar cópias de segurança. Um contentor com este nome é criado, se já existir, durante o carregamento de cópia de segurança. Preencher `ConnectionString` com uma cadeia de ligação válida para a conta de armazenamento do Azure, substituindo `account-name` com o nome de conta de armazenamento, e `account-key` com a sua chave de conta de armazenamento.

Execute o seguinte script do PowerShell para invocar a API de REST necessários para criar nova política. Substitua `account-name` com o nome de conta de armazenamento, e `account-key` com a sua chave de conta de armazenamento.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

### <a name="enable-periodic-backup"></a>Ativar a cópia de segurança periódica
Depois de definir a política de cópia de segurança para satisfazer os requisitos de proteção de dados da aplicação, a política de cópia de segurança deve ser associada à aplicação. Dependendo do requisito, a política de cópia de segurança pode ser associada a uma aplicação, serviço ou uma partição.

Executar o seguinte script do PowerShell para invocar a API de REST necessárias para associar a política de cópia de segurança com o nome `BackupPolicy1` criado no passo com a aplicação `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/EnableBackup?api-version=6.2-preview"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Certifique-se de que as cópias de segurança periódicas estão a funcionar

Depois de ativar a cópia de segurança ao nível da aplicação, todas as partições que pertencem com monitorização de estado Reliable services e Reliable Actors sob a aplicação irão iniciar a obtenção de cópia de segurança periodicamente de acordo com a política de cópia de segurança associadas. 

![Eventos de estado de funcionamento BackedUp de partição][0]

### <a name="list-backups"></a>Cópias de segurança de lista

As cópias de segurança associadas a todas as partições que pertencem a serviços com monitorização de estado fiável e Reliable Actors da aplicação podem ser enumeradas utilizando _GetBackups_ API. As cópias de segurança podem ser enumeradas para uma partição, uma aplicação ou serviço.

Executar o seguinte script do PowerShell para invocar a API de HTTP ao enumerar as cópias de segurança criadas para todas as partições dentro de `SampleApp` aplicação.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/GetBackups?api-version=6.2-preview"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Execute a saída de exemplo para o procedimento acima:

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

## <a name="preview-limitation-caveats"></a>Limitação de pré-visualização / avisos
- Não existem recursos de infraestrutura de serviço criado nos cmdlets do PowerShell.
- Sem suporte para a CLI de recursos de infraestrutura de serviço.
- Não existe suporte para remoção de cópia de segurança automatizada. Requer manual limpeza de cópias de segurança.
- Não existe suporte para o Service Fabric clusters no Linux.

## <a name="next-steps"></a>Próximos Passos
- [Referência da REST API de restauro de cópia de segurança](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent_Azure.png

