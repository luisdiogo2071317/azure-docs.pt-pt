---
title: Migrar a partir do Azure v2 v3 de serviços de multimédia | Microsoft Docs
description: Este artigo descreve as alterações que foram introduzidas no v3 de Media Services do Azure e mostra as diferenças entre duas versões.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: serviços de multimédia do azure, transmissão, difusão, em direto, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 06/12/2018
ms.author: juliako
ms.openlocfilehash: a382af644d30f9f0ebb586273c982ef1766f50b0
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295688"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Migrar a partir de serviços de multimédia v2 v3

> [!NOTE]
> A versão mais recente dos Serviços de Multimédia do Azure está em Pré-visualização e poderá ser referida como v3.

Este artigo descreve as alterações que foram introduzidas no Serviços de suporte de dados do Azure (AMS) v3 e mostra as diferenças entre duas versões.

## <a name="why-should-you-move-to-v3"></a>Por que motivo deve mover v3?

### <a name="api-is-more-approachable"></a>API é mais acessível

*  v3 baseiam-se uma superfície de API unificada que expõe funcionalidades de gestão e operações incorporadas no Azure Resource Manager. Modelos Azure Resource Manager podem ser utilizados para criar e implementar as transformações, pontos finais de transmissão em fluxo, LiveEvents e muito mais.
* Abrir API (aka Swagger) documento de especificação.
* SDKs disponíveis para .net, .net Core, Node.js, Python, Java, Ruby.
* Integração do CLI do Azure.

### <a name="new-features"></a>Novas funcionalidades

* Codificação agora suporta HTTPS inserção (baseado no Url de entrada).
* Transformações são uma novidade no v3. Uma transformação é utilizada para partilhar as configurações, criar modelos do Azure Resource Manager e isolar as definições de codificação de um inquilino ou cliente específico. 
* Um recurso pode ter vários StreamingLocators com diferentes definições de empacotamento dinâmico e a encriptação dinâmica.
* Proteção de conteúdos suporta funcionalidades várias chaves. 
* Pré-visualização do LiveEvent suporta empacotamento dinâmico e a encriptação dinâmica. Isto permite que a proteção de conteúdos no empacotamento de pré-visualização, bem como DASH e HLS.
* LiveOuput é mais simples utilizar que a entidade mais antiga do programa. 
* Foi adicionado suporte RBAC entidades.

## <a name="changes-from-v2"></a>Alterações de v2

* Nos Media Services v3, encriptação de armazenamento (encriptação AES 256) só é suportada para em efeitos de compatibilidade quando os elementos criados com os Media Services v2. Significado v3 funciona com o armazenamento existente encriptados ativos, mas não permitirá a criação de novos.

    Para elementos criados com v3, os Media Services suportam a [Storage do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) encriptação de armazenamento do lado do servidor.
    
* SDKs de serviços de suporte de dados desassociada do SDK de armazenamento que lhe dá mais controlo sobre o SDK de armazenamento utilizado e evita a problemas de controlo de versões. 
* V3, todas as taxas de bits codificação estão no bits por segundo. Isto é diferente de v2 as restantes predefinições codificador de multimédia Standard. Por exemplo, velocidade de transmissão na v2 teria de ser especificada como 128, mas v3 seria possível 128000. 
* IngestManifests AssetFiles, AccessPolicies, não existem na v3.
* ContentKeys deixam de uma entidade, propriedade o StreamingLocator.
* Suporte de grelha de evento substitui NotificationEndpoints.
* Algumas entidades foram mudar o nome

  * JobOutput substitui tarefas, agora apenas parte da tarefa.
  * LiveEvent substitui o canal.
  * LiveOutput substitui o programa.
  * StreamingLocator substitui localizador.

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

### <a name="publish-an-asset-with-aes-encryption"></a>Publicar um elemento com a encriptação AES 

#### <a name="v2"></a>v2

1. Criar ContentKeyAuthorizationPolicyOption
2. Criar ContentKeyAuthorizationPolicy
3. Criar AssetDeliveryPolicy
4. Criar elemento e carregar conteúdo ou submeter tarefa e utilizar recursos de saída
5. Associar AssetDeliveryPolicy ativo
6. Criar ContentKey
7. Anexar ContentKey ativo
8. Criar AccessPolicy
9. Criar localizador

#### <a name="v3"></a>v3

1. Criar política de chave de conteúdo
2. Criar recurso
3. Carregar conteúdo ou utilizar recursos como JobOutput
4. Criar StreamingLocator

## <a name="next-steps"></a>Passos Seguintes

Para ver como é fácil começar a codificar e a transmitir ficheiros de vídeo, veja [Stream files](stream-files-dotnet-quickstart.md) (Transmitir ficheiros). 

