---
title: Criar uma imagem do Docker para gestão de modelos no Azure Machine Learning | Documentos da Microsoft
description: Este artigo descreve os passos para criar uma imagem do Docker para o seu serviço web.
services: machine-learning
author: chhavib
ms.author: chhavib
manager: hjerez
editor: jasonwhowell
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 07f8a31244acca48a0bbae8ad26b0c5b509f0c0a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959585"
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Referência da API de conta do gestão de modelo do Machine Learning do Azure

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Para obter informações sobre como configurar o ambiente de implantação, consulte [configuração da conta de gestão de modelos](deployment-setup-configuration.md).

API de conta de gestão de modelo do Azure Machine Learning implementa as seguintes operações:

- Registo do modelo
- Criação do manifesto
- Criação de imagens do docker
- Criação do serviço Web

Pode utilizar esta imagem para criar um web service seja localmente ou num cluster remoto do Azure Container Service ou outro ambiente do Docker com suporte à sua escolha.

## <a name="prerequisites"></a>Pré-requisitos
Certifique-se de que seguiu os passos de instalação no [instalar e criar o guia de introdução](quickstart-installation.md) documento.

É necessário o seguinte antes de continuar:
1. Aprovisionamento da conta de gestão de modelos
2. Criação do ambiente para implementar e gerir modelos
3. Um modelo de Machine Learning

### <a name="azure-ad-token"></a>Tokens do Azure AD
Quando estiver a utilizar o CLI do Azure, inicie sessão com `az login`. A CLI utiliza o token do Azure Active Directory (Azure AD) do ficheiro. Azure. Se pretender utilizar as APIs, tem as seguintes opções.

##### <a name="acquire-the-azure-ad-token-manually"></a>Adquirir o token do Azure AD manualmente
Pode usar `az login` e obter a versão mais recente token a partir do ficheiro no diretório raiz. Azure.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Adquirir o token do Azure AD através de programação
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Depois de criar o principal de serviço, guarde a saída. Agora pode usá-lo para obter um token do Azure AD:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
O token é colocado num cabeçalho de autorização para chamadas de API.


## <a name="register-a-model"></a>Registar um modelo

O passo de registo do modelo registra seu modelo de Machine Learning com a conta de gestão de modelos do Azure que criou. Este registo permite que os modelos e suas versões que estão atribuídos no momento do registo de controlo. O utilizador fornece o nome do modelo. Registo subsequente dos modelos sob o mesmo nome gera uma nova versão e o ID.

### <a name="request"></a>Pedir

| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Descrição
Registra um modelo.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| modelo | corpo | Payload que serve para registar um modelo. | Sim | [Modelo](#model) |


### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | OK. O registo do modelo foi concluída com êxito. | [Modelo](#model) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Consulta a lista de modelos numa conta
### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Descrição
Consulta a lista de modelos numa conta. Pode filtrar a lista de resultados por marca e o nome. Se nenhum filtro for transmitido, a consulta lista todos os modelos na conta. A lista devolvida é paginada e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| nome | consulta | Nome do objeto. | Não | cadeia |
| etiqueta | consulta | Etiqueta de modelo. | Não | cadeia |
| count | consulta | Número de itens a obter numa página. | Não | cadeia |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedModelList](#paginatedmodellist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Obter os detalhes do modelo
### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
 |

### <a name="description"></a>Descrição
Obtém um modelo por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de objeto. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Modelo](#model) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Registe-se de um manifesto com o modelo registado e todas as dependências

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Descrição
Registra um manifesto com o modelo registado e todas as respetivas dependências.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| manifestRequest | corpo | Payload que é utilizado para registar um manifesto. | Sim | [Manifesto](#manifest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Registo de manifesto foi concluída com êxito. | [Manifesto](#manifest) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>A lista de manifestos numa conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Descrição
Consulta a lista de manifestos de uma conta. Pode filtrar a lista de resultados por ID de modelo e nome do manifesto. Se nenhum filtro for transmitido, a consulta lista todos os manifestos na conta. A lista devolvida é paginada e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| modelId | consulta | ID de modelo. | Não | cadeia |
| manifestName | consulta | Nome do manifesto. | Não | cadeia |
| count | consulta | Número de itens a obter numa página. | Não | cadeia |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedManifestList](#paginatedmanifestlist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Obter os detalhes de manifestos

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

### <a name="description"></a>Descrição
Obtém o manifesto por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de objeto. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Manifesto](#manifest) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Criar uma imagem

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Descrição
Cria uma imagem como uma imagem do Docker no Azure Container Registry.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| imageRequest | corpo | Payload que é utilizado para criar uma imagem. | Sim | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Cabeçalhos | Esquema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | URL de localização de operação assíncrona. Uma chamada GET mostrará a o estado da tarefa de criação de imagem. | Operação de localização |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>A lista de imagens numa conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Descrição
Consulta a lista de imagens numa conta. Pode filtrar a lista de resultados por ID de manifesto e nome. Se nenhum filtro for transmitido, a consulta lista todas as imagens na conta. A lista devolvida é paginada e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| manifestId | consulta | ID de manifesto. | Não | cadeia |
| manifestName | consulta | Nome do manifesto. | Não | cadeia |
| count | consulta | Número de itens a obter numa página. | Não | cadeia |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedImageList](#paginatedimagelist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Obter os detalhes da imagem

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

### <a name="description"></a>Descrição
Obtém uma imagem por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de imagem. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Imagem](#image) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Criar um serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Descrição
Cria um serviço a partir de uma imagem.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| serviceRequest | corpo | Payload que é utilizado para criar um serviço. | Sim | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Cabeçalhos | Esquema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | URL de localização de operação assíncrona. Uma chamada GET mostrará a o estado da tarefa de criação de serviço. | Operação de localização |
| 409 | Um serviço com o nome especificado já existe. |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Consulta a lista de serviços numa conta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Descrição
Consulta a lista de serviços numa conta. Pode filtrar a lista de resultados por nome/ID de modelo, nome/ID de manifesto, ID de imagem, nome do serviço ou ID de recurso de computação do Machine Learning. Se nenhum filtro for transmitido, a consulta lista todos os serviços na conta. A lista devolvida é paginada e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| serviceName | consulta | Nome do serviço. | Não | cadeia |
| modelId | consulta | Nome do modelo. | Não | cadeia |
| modelName | consulta | ID de modelo. | Não | cadeia |
| manifestId | consulta | ID de manifesto. | Não | cadeia |
| manifestName | consulta | Nome do manifesto. | Não | cadeia |
| imageId | consulta | ID de imagem. | Não | cadeia |
| computeResourceId | consulta | ID de recurso de computação Machine Learning. | Não | cadeia |
| count | consulta | Número de itens a obter numa página. | Não | cadeia |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedServiceList](#paginatedservicelist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Obter os detalhes do serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Descrição
Obtém um serviço por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de objeto. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [ServiceResponse](#serviceresponse) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Atualizar um serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| PUT |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Descrição
Atualiza um serviço existente.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de objeto. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| serviceUpdateRequest | corpo | Payload que é utilizado para atualizar um serviço existente. | Sim |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Cabeçalhos | Esquema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | URL de localização de operação assíncrona. Uma chamada GET mostrará a o estado da tarefa de serviço de atualização. | Operação de localização |
| 404 | O serviço com o ID especificado não existe. |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Eliminar um serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| DELETE |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Descrição
Elimina um serviço.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de objeto. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. |  |
| 204 | O serviço com o ID especificado não existe. |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Obter chaves de serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Descrição
Obtém as chaves de serviço.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de serviço. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [AuthKeys](#authkeys)
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Voltar a gerar chaves de serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  / API/subscrições / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / regeneratekeys do | 

### <a name="description"></a>Descrição
Regenera as chaves de serviço e devolve-os.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de serviço. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| regenerateKeyRequest | corpo | Payload que é utilizado para atualizar um serviço existente. | Sim | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [AuthKeys](#authkeys)
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Consulta a lista de implementações numa conta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

### <a name="description"></a>Descrição
Consulta a lista de implementações numa conta. Pode filtrar a lista de resultados por ID de serviço, que retornará apenas as implementações criadas para o serviço específico. Se nenhum filtro for transmitido, a consulta lista todas as implementações na conta.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |
| serviceId | consulta | ID de serviço. | Não | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [DeploymentList](#deploymentlist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Obter os detalhes de implementação

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

### <a name="description"></a>Descrição
Obtém a implantação por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de implementação. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Implementação](#deployment) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>Obter os detalhes da operação

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

### <a name="description"></a>Descrição
Obtém o estado da operação assíncrona por ID de operação.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado em | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID de subscrição do Azure. | Sim | cadeia |
| resourceGroupName | caminho | Nome do grupo de recursos no qual está localizada a conta de gestão de modelos. | Sim | cadeia |
| accountName | caminho | Nome da conta de gestão de modelos. | Sim | cadeia |
| ID | caminho | ID de operação. | Sim | cadeia |
| versão de API | consulta | Versão do fornecedor de recursos de Microsoft.Machine.Learning API a utilizar. | Sim | cadeia |
| Autorização | cabeçalho | Token de autorização. Deve ser algo como "Bearer XXXXXX." | Sim | cadeia |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [OperationStatus](#asyncoperationstatus) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definições

<a name="asset"></a>
### <a name="asset"></a>Recurso
O objeto de elemento que serão necessários durante a criação de imagens do Docker.


|Nome|Descrição|Esquema|
|---|---|---|
|**id**  <br>*Opcional*|ID de recurso.|cadeia|
|**mimeType**  <br>*Opcional*|Tipo MIME do conteúdo de modelo. Para obter mais informações sobre o tipo de MIME, consulte a [lista de tipos de mídia IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|cadeia|
|**Descompactar**  <br>*Opcional*|Indica onde é necessário de descompactar o conteúdo durante a criação de imagens do Docker.|boolean|
|**URL**  <br>*Opcional*|URL de localização do recurso.|cadeia|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
O estado da operação de async.

*Tipo de*: enum (NotStarted, em execução, cancelada, com êxito, falha)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
O estado da operação.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdTime**  <br>*Opcional*  <br>*read-only*|Hora de criação de operação de Async (UTC).|a cadeia de caracteres (data-hora)|
|**endTime**  <br>*Opcional*  <br>*read-only*|Hora de fim de operação de Async (UTC).|a cadeia de caracteres (data-hora)|
|**error**  <br>*Opcional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*Opcional*|ID de operação assíncrona.|cadeia|
|**operationType**  <br>*Opcional*|Tipo de operação assíncrona.|enumeração (imagem, serviço)|
|**resourceLocation**  <br>*Opcional*|Recurso criada ou atualizada pela operação assíncrona.|cadeia|
|**state**  <br>*Opcional*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
As chaves de autenticação para um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**primaryKey**  <br>*Opcional*|Chave primária.|cadeia|
|**secondaryKey**  <br>*Opcional*|Chave secundária.|cadeia|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>Dimensionamento automático
Definições para o dimensionamento automático.


|Nome|Descrição|Esquema|
|---|---|---|
|**autoscaleEnabled**  <br>*Opcional*|Ativar ou desativar o dimensionamento automático.|boolean|
|**maxReplicas**  <br>*Opcional*|Número máximo de réplicas de pod Dimensionar até.  <br>**Valor mínimo**: `1`|inteiro|
|**minReplicas**  <br>*Opcional*|Número mínimo de réplicas de pod para diminuir a escala para.  <br>**Valor mínimo**: `0`|inteiro|
|**refreshPeriodInSeconds**  <br>*Opcional*|Hora para o acionador de dimensionamento automático da atualização.  <br>**Valor mínimo**: `1`|inteiro|
|**targetUtilization**  <br>*Opcional*|Percentagem de utilização que aciona o dimensionamento automático.  <br>**Valor mínimo**: `0`  <br>**O valor máximo**: `100`|inteiro|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
O recurso de computação do Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**id**  <br>*Opcional*|ID de recurso.|cadeia|
|**tipo**  <br>*Opcional*|Tipo de recurso.|enumeração (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Configuração de reservar recursos para um contentor no cluster.


|Nome|Descrição|Esquema|
|---|---|---|
|**cpu**  <br>*Opcional*|Especifica a reserva de CPU. Formato para Kubernetes: veja [significado de CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|cadeia|
|**Memória**  <br>*Opcional*|Especifica a reserva de memória. Formato para Kubernetes: veja [significado de memória](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|cadeia|


<a name="deployment"></a>
### <a name="deployment"></a>Implementação
Uma instância de uma implementação do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*Opcional*  <br>*read-only*|Hora de criação da implementação (UTC).|a cadeia de caracteres (data-hora)|
|**expiredAt**  <br>*Opcional*  <br>*read-only*|Implementação expirado hora (UTC).|a cadeia de caracteres (data-hora)|
|**id**  <br>*Opcional*|ID de implementação.|cadeia|
|**imageId**  <br>*Opcional*|ID de imagem associado esta implementação.|cadeia|
|**serviceName**  <br>*Opcional*|Nome do serviço.|cadeia|
|**status**  <br>*Opcional*|Estado da implementação atual.|cadeia|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Uma matriz de objetos de implementação.

*Tipo*: <[implementação](#deployment)> matriz


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Detalhe do erro de serviço de gestão de modelos.


|Nome|Descrição|Esquema|
|---|---|---|
|**Código**  <br>*Necessário*|Código de erro.|cadeia|
|**message**  <br>*Necessário*|Mensagem de erro.|cadeia|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>Byl vrácen Prvek
Um objeto de erro do serviço de gestão de modelos.


|Nome|Descrição|Esquema|
|---|---|---|
|**Código**  <br>*Necessário*|Código de erro.|cadeia|
|**Detalhes**  <br>*Opcional*|Matriz de objetos de detalhes do erro.|<[ErrorDetail](#errordetail)> matriz|
|**message**  <br>*Necessário*|Mensagem de erro.|cadeia|
|**statusCode**  <br>*Opcional*|Código de estado HTTP.|inteiro|


<a name="image"></a>
### <a name="image"></a>Imagem
A imagem do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**computeResourceId**  <br>*Opcional*|ID do ambiente criado no recurso de computação do Machine Learning.|cadeia|
|**createdTime**  <br>*Opcional*|Hora de criação de imagem (UTC).|a cadeia de caracteres (data-hora)|
|**creationState**  <br>*Opcional*||[AsyncOperationState](#asyncoperationstate)|
|**Descrição**  <br>*Opcional*|Texto de descrição de imagem.|cadeia|
|**error**  <br>*Opcional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*Opcional*|ID de imagem.|cadeia|
|**imageBuildLogUri**  <br>*Opcional*|URI dos registos carregados da compilação de imagem.|cadeia|
|**imageLocation**  <br>*Opcional*|Cadeia de localização de registo de contentor do Azure para a imagem criada.|cadeia|
|**imageType**  <br>*Opcional*||[ImageType](#imagetype)|
|**manifest**  <br>*Opcional*||[Manifesto](#manifest)|
|**name**  <br>*Opcional*|Nome da imagem.|cadeia|
|**Versão**  <br>*Opcional*|Versão da imagem definido pelo serviço de gestão de modelos.|inteiro|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Um pedido para criar uma imagem do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**computeResourceId**  <br>*Necessário*|ID do ambiente criado no recurso de computação do Machine Learning.|cadeia|
|**Descrição**  <br>*Opcional*|Texto de descrição de imagem.|cadeia|
|**imageType**  <br>*Necessário*||[ImageType](#imagetype)|
|**manifestId**  <br>*Necessário*|ID do manifesto do qual a imagem será criada.|cadeia|
|**name**  <br>*Necessário*|Nome da imagem.|cadeia|


<a name="imagetype"></a>
### <a name="imagetype"></a>TipoImagem
Especifica o tipo da imagem.

*Tipo de*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifesto
O manifesto do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**assets**  <br>*Necessário*|Lista de recursos.|<[Asset](#asset)> matriz|
|**createdTime**  <br>*Opcional*  <br>*read-only*|Hora de criação de manifesto (UTC).|a cadeia de caracteres (data-hora)|
|**Descrição**  <br>*Opcional*|Texto de descrição de manifesto.|cadeia|
|**driverProgram**  <br>*Necessário*|Programa de Driver do manifesto.|cadeia|
|**id**  <br>*Opcional*|ID de manifesto.|cadeia|
|**modelIds**  <br>*Opcional*|Lista de IDs de modelo dos modelos de registados. O pedido irá falhar se qualquer um dos modelos incluídos não estão registados.|<string> Matriz|
|**modelType**  <br>*Opcional*|Especifica que os modelos já estão registados com o serviço de gestão de modelos.|enumeração (registada)|
|**name**  <br>*Necessário*|Nome do manifesto.|cadeia|
|**targetRuntime**  <br>*Necessário*||[TargetRuntime](#targetruntime)|
|**Versão**  <br>*Opcional*  <br>*read-only*|Versão do manifesto atribuído pelo serviço de gestão de modelos.|inteiro|
|**webserviceType**  <br>*Opcional*|Especifica o tipo desejado de serviço web que será criado a partir do manifesto.|enumeração (em tempo real)|


<a name="model"></a>
### <a name="model"></a>Modelo
Uma instância de um modelo do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*Opcional*  <br>*read-only*|Hora de criação de modelo (UTC).|a cadeia de caracteres (data-hora)|
|**Descrição**  <br>*Opcional*|Texto de descrição do modelo.|cadeia|
|**id**  <br>*Opcional*  <br>*read-only*|ID de modelo.|cadeia|
|**mimeType**  <br>*Necessário*|Tipo MIME do conteúdo do modelo. Para obter mais informações sobre o tipo de MIME, consulte a [lista de tipos de mídia IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|cadeia|
|**name**  <br>*Necessário*|Nome do modelo.|cadeia|
|**etiquetas**  <br>*Opcional*|Lista de etiquetas do modelo.|<string> Matriz|
|**Descompactar**  <br>*Opcional*|Indica se é necessário de descompactar o modelo durante a criação de imagens do Docker.|boolean|
|**URL**  <br>*Necessário*|URL do modelo. Normalmente, colocamos um URL de assinatura de acesso partilhado aqui.|cadeia|
|**Versão**  <br>*Opcional*  <br>*read-only*|Versão de modelo atribuído pelo serviço de gestão de modelos.|inteiro|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
As informações da coleção de dados do modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**eventHubEnabled**  <br>*Opcional*|Ative um hub de eventos para um serviço.|boolean|
|**storageEnabled**  <br>*Opcional*|Ative o armazenamento para um serviço.|boolean|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Uma lista paginada de imagens.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (absolutní identifikátor URI) para a próxima página de resultados na lista.|cadeia|
|**valor**  <br>*Opcional*|Matriz de objetos de modelo.|<[Imagem](#image)> matriz|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Uma lista paginada de manifestos.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (absolutní identifikátor URI) para a próxima página de resultados na lista.|cadeia|
|**valor**  <br>*Opcional*|Matriz de objetos de manifestos.|<[Manifesto](#manifest)> matriz|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Uma lista paginada de modelos.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (absolutní identifikátor URI) para a próxima página de resultados na lista.|cadeia|
|**valor**  <br>*Opcional*|Matriz de objetos de modelo.|<[Modelo](#model)> matriz|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Uma lista paginada de serviços.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (absolutní identifikátor URI) para a próxima página de resultados na lista.|cadeia|
|**valor**  <br>*Opcional*|Matriz de objetos do serviço.|<[ServiceResponse](#serviceresponse)> matriz|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Um pedido para criar um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**appInsightsEnabled**  <br>*Opcional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*Opcional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Necessário*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*Opcional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Opcional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Necessário*|Imagem para criar o serviço.|cadeia|
|**maxConcurrentRequestsPerContainer**  <br>*Opcional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|inteiro|
|**name**  <br>*Necessário*|Nome do serviço.|cadeia|
|**numReplicas**  <br>*Opcional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se o dimensionamento automático está ativado.  <br>**Valor mínimo**: `0`|inteiro|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Um pedido para voltar a gerar uma chave para um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**keyType**  <br>*Opcional*|Especifica qual a chave para voltar a gerar.|enumeração (primário, secundário)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Estado detalhado do serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*Opcional*|Hora de criação de serviço (UTC).|a cadeia de caracteres (data-hora)|
|**ID**  <br>*Opcional*|ID de serviço.|cadeia|
|**image**  <br>*Opcional*||[Imagem](#image)|
|**manifest**  <br>*Opcional*||[Manifesto](#manifest)|
|**modelos**  <br>*Opcional*|Lista de modelos.|<[Modelo](#model)> matriz|
|**name**  <br>*Opcional*|Nome do serviço.|cadeia|
|**scoringUri**  <br>*Opcional*|URI para o serviço de classificação.|cadeia|
|**state**  <br>*Opcional*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*Opcional*|Hora (UTC) da última atualização.|a cadeia de caracteres (data-hora)|
|**appInsightsEnabled**  <br>*Opcional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*Opcional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Necessário*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*Opcional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Opcional*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*Opcional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|inteiro|
|**numReplicas**  <br>*Opcional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se o dimensionamento automático está ativado.  <br>**Valor mínimo**: `0`|inteiro|
|**error**  <br>*Opcional*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>ServiceUpdateRequest
Um pedido para atualizar um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**appInsightsEnabled**  <br>*Opcional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*Opcional*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*Opcional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Opcional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Opcional*|Imagem para criar o serviço.|cadeia|
|**maxConcurrentRequestsPerContainer**  <br>*Opcional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|inteiro|
|**numReplicas**  <br>*Opcional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se o dimensionamento automático está ativado.  <br>**Valor mínimo**: `0`|inteiro|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
O tipo do tempo de execução de destino.


|Nome|Descrição|Esquema|
|---|---|---|
|**Propriedades**  <br>*Necessário*||< string, string > mapa|
|**runtimeType**  <br>*Necessário*|Especifica o tempo de execução.|enumeração (SparkPython, Python)|

