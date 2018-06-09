---
title: Migrar para a API de AI de lote do Azure atualizada | Microsoft Docs
description: Como atualizar o código de AI do Azure Batch e scripts para utilizar a área de trabalho e experimente recursos
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: 94db54f35b7871407368b174536dc6454775779c
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249407"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Migrar para a API de AI do lote atualizado

Na futura API de REST do Batch AI versão 2018-05-01 e relacionados Batch AI SDKs e ferramentas, estão a ser introduzidas alterações significativas e novas funcionalidades.

Se utilizou uma versão anterior da API de AI do Batch, este artigo explica como modificar o código e a funcionar com a nova API de scripts. Efetue estas alterações apenas depois da nova API está disponível.

## <a name="whats-changing"></a>O que está a mudar?

Em resposta a comentários de clientes, iremos estiver a remover os limites no número de tarefas e a tornar mais fácil de gerir recursos de AI do Batch. A nova API introduz dois novos recursos *área de trabalho* e *experimentação*. Tarefas de preparação relacionados sob uma experimentação de recolher e organizar todos os recursos relacionados com Batch AI (clusters de servidores de ficheiros, experimentações, as tarefas) na área de trabalho.  

* **Área de trabalho** -uma coleção de nível superior de todos os tipos de recursos de AI do Batch. Clusters e servidores de ficheiros estão contidos numa área de trabalho. Áreas de trabalho que pertencem a diferentes grupos ou projetos de trabalho, normalmente, separado. Por exemplo, pode ter um dev e uma área de trabalho de teste. Provavelmente terá apenas um número limitado de áreas de trabalho por subscrição. 

* **Experimentação** -uma coleção de tarefas relacionadas que pode ser consultado e gerido em conjunto. Por exemplo, utilize uma experimentação para todas as tarefas que são efetuadas como parte de um paramétrico de otimização de hyper-parâmetro de grupo. 

A imagem seguinte mostra um exemplo de hierarquia de recursos. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>Monitorizar e gerir recursos existentes
Cada grupo de recursos em que já criou clusters AI do Batch, tarefas ou servidores de ficheiros, o serviço Batch AI irá criar uma área de trabalho com o nome `migrated-<region>` (por exemplo, `migrated-eastus`) e uma experimentação denominado `migrated`. Para aceder a tarefas criadas anteriormente, clusters ou servidores de ficheiros, terá de utilizar a área de trabalho migrada e experimente. 

### <a name="portal"></a>Portal 
Para aceder a tarefas criadas anteriormente, clusters ou servidores de ficheiros através do portal, selecione o `migrated-<region>` área de trabalho. Depois de selecionar a área de trabalho, efetue operações como redimensionamento ou eliminar um cluster e ver o estado da tarefa e saídas. 

### <a name="sdks"></a>SDKs 
Para aceder aos trabalhos, clusters ou servidores de ficheiros que criou anteriormente através de SDKs de AI do Batch, forneça o nome da área de trabalho e experimentar parâmetros de nome. 

Se utilizar o SDK Python, alterações relevantes são mostradas nos exemplos seguintes. As alterações são semelhantes em outros SDKs do Batch AI. 


#### <a name="get-old-cluster"></a>Obter o cluster antigo 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Eliminar o cluster antigo 

```python
client.clusters.delete(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="get-old-file-server"></a>Obter o antigo servidor de ficheiros 

```python
cluster = client.fileservers.get(resource_group_name, 'migrated-<region>', fileserver_name)
```

#### <a name="delete-old-file-server"></a>Eliminar o antigo servidor de ficheiros  

```python
client.fileservers.delete(resource_group_name, 'migrated-<region>', fileserver_name)
``` 


#### <a name="get-old-job"></a>Obter tarefa antiga 

```python
cluster = client.jobs.get(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```

#### <a name="delete-old-job"></a>Eliminar a tarefa antiga

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>CLI do Azure 
 
Quando utilizar a CLI do Azure para as tarefas de acesso que criou anteriormente, clusters ou servidores de ficheiros, utilize o `-w` e `-e` parâmetros para fornecer a área de trabalho e os nomes de experimentação. 


#### <a name="get-old-cluster"></a>Obter o cluster antigo

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Eliminar o cluster antigo 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Obter o antigo servidor de ficheiros

```azurecli
az batchai fileserver show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Eliminar o antigo servidor de ficheiros 

```azurecli
az batchai fileserver delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Obter tarefa antiga

```azurecli
az batchai fileserver show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Eliminar a tarefa antiga 

```azurecli
az batchai fileserver delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Criar recursos do Batch AI 
 
Se estiver a utilizar um dos SDKs de AI existentes do Batch, pode continuar a utilizá-la para criar recursos do Batch AI (tarefas, clusters ou servidores de ficheiros) sem fazer alterações de código. No entanto, depois de atualizar para o novo SDK, terá de efetuar as seguintes alterações.
 
### <a name="create-workspace"></a>Criar área de trabalho 
Dependendo do seu cenário, pode criar uma área de trabalho manualmente Monouso através do portal ou a CLI. Se estiver a utilizar a CLI, crie uma área de trabalho utilizando o seguinte comando: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Criar experimentação 


Dependendo do seu cenário, pode criar uma experimentação manualmente Monouso através do portal ou a CLI. Se estiver a utilizar a CLI, crie uma experimentação utilizando o seguinte comando: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Criar clusters, servidores de ficheiros e tarefas
 
Se utilizar o portal para criar tarefas, clusters ou servidores de ficheiros, o portal irá ajudá-lo durante a experiência de criação para fornecer o nome da área de trabalho e experimente parâmetros de nome.

Para criar tarefas, clusters ou servidores de ficheiros através do SDK atualizado, forneça o parâmetro de nome de área de trabalho. Se utilizar o SDK Python, alterações relevantes são mostradas nos exemplos seguintes. Substitua *workspace_name* e *experiment_name* com a área de trabalho e a experimentação que criou anteriormente. As alterações são semelhantes em outros SDKs do Batch AI. 


#### <a name="create-cluster"></a>Criar cluster 

```python
_ = client.clusters.create(resource_group_name, workspace_name, cluster_name, cluster_create_parameters).result()
```

#### <a name="create-file-server"></a>Criar o servidor de ficheiros 

```python
_ = client.fileservers.create(resource_group_name, workspace_name, fileserver_name, fileserver_create_parameters).result()
```

#### <a name="create-job"></a>Criar tarefa 

```python
_ = client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name job_create_parameters).result()
```


## <a name="next-steps"></a>Passos Seguintes

* Consulte a referência da API do Batch AI: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [Node.js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai)e [REST](/rest/api/batchai)