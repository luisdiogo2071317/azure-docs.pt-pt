---
title: Migrar para o API atualizada de IA do Azure Batch | Documentos da Microsoft
description: Como atualizar o código do Azure Batch AI e scripts para utilizar a área de trabalho e recursos de experimentação
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
ROBOTS: NOINDEX
ms.openlocfilehash: 75a9a5e9bafd62b320397c00ef6574b7536d9e09
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407785"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Migrar para a API de IA do Batch atualizada

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Na API de REST do Batch AI versão 2018-05-01 e SDKs de IA do Batch relacionados e ferramentas, alterações significativas e os novos recursos foram introduzidos.

Se já usou uma versão anterior da API de IA do Batch, este artigo explica como modificar seus scripts para trabalhar com a nova API e código. 

## <a name="whats-changing"></a>O que está a mudar?

Em resposta aos comentários dos clientes, iremos estiver a remover limites no número de tarefas e tornando mais fácil gerir os recursos do Batch AI. A nova API introduz dois novos recursos, *área de trabalho* e *experimentar*. Tarefas de formação relacionados numa experimentação de recolher e organizar todas as respetivas o Batch AI recursos (clusters, servidores de ficheiros, experiências, tarefas) numa área de trabalho.  

* **Área de trabalho** -uma coleção de nível superior de todos os tipos de recursos do Batch AI. Clusters e os servidores de ficheiros estão contidos numa área de trabalho. Áreas de trabalho que pertencem a diferentes grupos ou os projetos de trabalho geralmente separado. Por exemplo, pode ter um desenvolvimento e uma área de trabalho de teste. Provavelmente, precisa apenas um número limitado de áreas de trabalho por subscrição. 

* **Experimentação** -uma coleção de tarefas relacionadas que podem ser consultadas e geridos em conjunto. Por exemplo, utilize uma experimentação para agrupar todas as tarefas que são executadas como parte do hyper-otimização varrimentos. 

A imagem seguinte mostra um exemplo de hierarquia de recursos. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>Monitorizar e gerir os recursos existentes
Cada grupo de recursos em que já criou clusters, tarefas ou servidores de ficheiros do Batch AI, o serviço Batch AI irá criar uma área de trabalho com o nome `migrated-<region>` (por exemplo, `migrated-eastus`) e uma experimentação com o nome `migrated`. Para acessar as tarefas criadas anteriormente, clusters ou servidores de ficheiros, terá de utilizar a área de trabalho migrada e experimente. 

### <a name="portal"></a>Portal 
Para acessar tarefas criadas anteriormente, clusters ou servidores de ficheiros com o portal, selecione o `migrated-<region>` área de trabalho. Depois de selecionar a área de trabalho, execute operações como redimensionar ou eliminar um cluster e ver o estado da tarefa e saídas. 

### <a name="sdks"></a>SDKs 
Para aceder às tarefas, clusters ou servidores de ficheiros anteriormente criados através de SDKs de IA do Batch, fornecem o nome de área de trabalho em os parâmetros de nome de experimentação. 

Se utilizar o SDK de Python, alterações relevantes são mostradas nos exemplos a seguir. As alterações são semelhantes em outros SDKs do Batch AI. 


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

#### <a name="delete-old-job"></a>Eliminar tarefa antiga

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>CLI do Azure 
 
Ao utilizar a CLI do Azure para tarefas de acesso que criou anteriormente, clusters ou servidores de ficheiros, utilize o `-w` e `-e` parâmetros para fornecer a área de trabalho e nomes de experimentação. 


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
az batchai file-server show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Eliminar o antigo servidor de ficheiros 

```azurecli
az batchai file-server delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Obter tarefa antiga

```azurecli
az batchai job show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Eliminar tarefa antiga 

```azurecli
az batchai job delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Criar recursos do Batch AI 
 
Se estiver a utilizar um dos SDKs de IA do Batch existentes, pode continuar a utilizá-lo para criar recursos do Batch AI (tarefas, clusters ou servidores de arquivos) sem fazer alterações de código. No entanto, após a atualização para o novo SDK, terá de efetuar as seguintes alterações.
 
### <a name="create-workspace"></a>Criar área de trabalho 
Dependendo do seu cenário, pode criar uma área de trabalho manualmente uma única vez através do portal ou na CLI. Se estiver a utilizar a CLI, crie uma área de trabalho com o seguinte comando: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Criar a experimentação 


Dependendo do seu cenário, pode criar uma experimentação manualmente uma única vez através do portal ou na CLI. Se estiver a utilizar a CLI, crie uma experimentação com o seguinte comando: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Criar clusters, servidores de ficheiros e tarefas
 
Se utilizar o portal para criar tarefas, clusters ou servidores de ficheiros, o portal irá guiá-lo durante a experiência de criação para fornecer o nome de área de trabalho e os parâmetros de nome de experimentação.

Para criar tarefas, clusters ou servidores de ficheiros através do SDK atualizado, forneça o parâmetro de nome de área de trabalho. Se utilizar o SDK de Python, alterações relevantes são mostradas nos exemplos a seguir. Substitua *workspace_name* e *experiment_name* com a área de trabalho e a experimentação que criou anteriormente. As alterações são semelhantes em outros SDKs do Batch AI. 


#### <a name="create-cluster"></a>Criar cluster 

```python
_ = client.clusters.create(resource_group_name, workspace_name, cluster_name, cluster_create_parameters).result()
```

#### <a name="create-file-server"></a>Criar servidor de ficheiros 

```python
_ = client.fileservers.create(resource_group_name, workspace_name, fileserver_name, fileserver_create_parameters).result()
```

#### <a name="create-job"></a>Criar tarefa 

```python
_ = client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name job_create_parameters).result()
```


## <a name="next-steps"></a>Passos Seguintes

* Consulte a referência de API do Batch AI: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [node. js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai), e [REST](/rest/api/batchai)