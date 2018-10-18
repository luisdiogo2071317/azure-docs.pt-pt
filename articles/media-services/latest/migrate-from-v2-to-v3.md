---
title: Migrar a partir do Azure v2 para a v3 dos serviços de multimédia | Documentos da Microsoft
description: Este artigo descreve as alterações que foram introduzidas no Serviços de multimédia do Azure v3 e mostra as diferenças entre duas versões.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: serviços de multimédia do azure, transmissão, difusão, em direto, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: a17bad5beb651aaa085c626296c200a00c30647e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376367"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Migrar a partir de serviços de multimédia v2 para a v3

Este artigo descreve as alterações que foram introduzidas no Azure Media Services (AMS) v3 e mostra as diferenças entre duas versões.

## <a name="why-should-you-move-to-v3"></a>Por que motivo deve mover a v3?

### <a name="api-is-more-approachable"></a>A API é mais acessível

*  v3 baseia-se numa superfície de API unificada que expõe a funcionalidade de gerenciamento e operações incorporada no Gestor de recursos do Azure. Modelos Azure Resource Manager podem ser utilizados para criar e implementar as transformações, pontos finais de transmissão em fluxo, LiveEvents e muito mais.
* Abra a API (também conhecido como Swagger) um documento de especificação.
* SDKs disponíveis para .net, .net Core, node. js, Python, Java, Ruby.
* Integração de CLI do Azure.

### <a name="new-features"></a>Novos recursos

* Codificação agora suporta HTTPS ingerir (baseado em Url de entrada).
* As transformações são novas na v3. Uma transformação é usada para compartilhar configurações, criar modelos do Azure Resource Manager e isolar as definições de codificação de um cliente específico ou inquilino. 
* Um elemento pode ter vários StreamingLocators com diferentes configurações de empacotamento dinâmico e a encriptação dinâmica.
* Proteção de conteúdo oferece suporte a recursos com múltiplos principais. 
* Pré-visualização do LiveEvent suporta o empacotamento dinâmico e a encriptação dinâmica. Isto permite que a proteção de conteúdo no empacotamento de pré-visualização, bem como DASH e HLS.
* LiveOuput é mais simples de usar do que a entidade de programa mais antiga. 
* Foi adicionado suporte RBAC em entidades.

## <a name="changes-from-v2"></a>Alterações do v2

* Em serviços de multimédia v3, encriptação de armazenamento (encriptação AES-256) só é suportada para em versões anteriores compatibilidade quando os recursos foram criados com os serviços de multimédia v2. O que significa v3 funciona com o armazenamento existente encriptado ativos, mas não permitirá que a criação de novos itens.

    Para os recursos criados com v3, serviços de multimédia suportam o [armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) encriptação de armazenamento do lado do servidor.
    
* SDKs de serviços de multimédia dissociado do SDK de armazenamento que fornece mais controle sobre o SDK de armazenamento utilizado e evita problemas de controle de versão. 
* Na v3, todas as taxas de bits de codificação são em bits por segundo. Isto é diferente da v2 REST que predefine Media Encoder Standard. Por exemplo, a velocidade de transmissão no v2 teria de ser especificada como 128, mas em v3 seria 128000. 
* IngestManifests AssetFiles, AccessPolicies, não existem na v3.
* ContentKeys já não são uma entidade, propriedade do StreamingLocator.
* Suporte a eventos Grid substitui NotificationEndpoints.
* Algumas entidades foram renomeadas

  * JobOutput substitui a tarefa, agora é apenas parte da tarefa.
  * LiveEvent substitui o canal.
  * LiveOutput substitui o programa.
  * StreamingLocator substitui o localizador.

## <a name="code-changes"></a>Alterações do código

### <a name="create-an-asset-and-upload-a-file"></a>Criar um elemento e carregar um ficheiro 

#### <a name="v2"></a>v2

```csharp
IAsset asset = context.Assets.Create(assetName, storageAccountName, options);

IAssetFile assetFile = asset.AssetFiles.Create(assetFileName);

assetFile.Upload(filePath);
```

#### <a name="v3"></a>v3

```csharp
Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, permissions: AssetContainerPermission.ReadWrite, expiryTime: DateTime.Now.AddHours(1));

var sasUri = new Uri(response.AssetContainerSasUrls.First());
CloudBlobContainer container = new CloudBlobContainer(sasUri);

var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
blob.UploadFromFile(fileToUpload);
```

### <a name="submit-a-job"></a>Submeter uma tarefa

#### <a name="v2"></a>v2

```csharp
IMediaProcessor processor = context.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

IJob job = jobs.Create($"Job for {inputAsset.Name}");

ITask task = job.Tasks.AddNew($"Task for {inputAsset.Name}", processor, taskConfiguration);

task.InputAssets.Add(inputAsset);

task.OutputAssets.AddNew(outputAssetName, outputAssetStorageAccountName, outputAssetOptions);

job.Submit();
```

#### <a name="v3"></a>v3

```csharp
client.Assets.CreateOrUpdate(resourceGroupName, accountName, outputAssetName, new Asset());

JobOutput[] jobOutputs = { new JobOutputAsset(outputAssetName)};

JobInput jobInput = JobInputAsset(assetName: assetName);

Job job = client.Jobs.Create(resourceGroupName,
accountName, transformName, jobName,
new Job {Input = jobInput, Outputs = jobOutputs});
```

### <a name="publish-an-asset-with-aes-encryption"></a>Publicar um elemento com encriptação AES 

#### <a name="v2"></a>v2

1. Criar ContentKeyAuthorizationPolicyOption
2. Criar ContentKeyAuthorizationPolicy
3. Criar AssetDeliveryPolicy
4. Criar elemento e carregar conteúdo ou submeter tarefa e utilizar recursos de saída
5. Associe AssetDeliveryPolicy com elemento
6. Criar ContentKey
7. Anexar ContentKey ativo
8. Criar AccessPolicy
9. Criar o localizador

#### <a name="v3"></a>v3

1. Criar política de chave de conteúdo
2. Criar recurso
3. Carregar conteúdo ou utilizar recursos como JobOutput
4. Criar StreamingLocator

## <a name="next-steps"></a>Passos Seguintes

Para ver como é fácil começar a codificar e a transmitir ficheiros de vídeo, veja [Stream files](stream-files-dotnet-quickstart.md) (Transmitir ficheiros). 

