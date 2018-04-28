---
title: Criar uma imagem de Docker para gestão de modelo no Azure Machine Learning | Microsoft Docs
description: Este artigo descreve os passos para criar uma imagem de Docker para o seu serviço web.
services: machine-learning
author: chhavib
ms.author: chhavib
manager: hjerez
editor: jasonwhowell
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: e518799fb4144093e5c3caf0524254a098e4b9cc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Referência da API de conta de gestão do Machine Learning modelo do Azure

Para obter informações sobre como configurar o ambiente de implementação, consulte [configuração da conta de gestão de modelo](deployment-setup-configuration.md).

A API de gestão de conta do Azure Machine Learning modelo implementa as seguintes operações:

- Registo de modelo
- Criação do manifesto
- Criação de imagens do docker
- Criação do serviço Web

Pode utilizar esta imagem para criar um serviço web ou localmente ou um cluster do serviço de contentor Azure remoto ou outro ambiente suportado de Docker à sua escolha.

## <a name="prerequisites"></a>Pré-requisitos
Certifique-se de que já leu os passos de instalação no [instalar e criar o guia de introdução](../service/quickstart-installation.md) documento.

É necessário o seguinte antes de continuar:
1. Modelo de aprovisionamento da conta de gestão
2. Criação do ambiente para implementar e gerir modelos
3. Um modelo de Machine Learning

### <a name="azure-ad-token"></a>Token do Azure AD
Quando estiver a utilizar a CLI do Azure, inicie sessão utilizando `az login`. A CLI utiliza o token do Azure Active Directory (Azure AD) a partir do ficheiro .azure. Se pretender utilizar as APIs, tem as seguintes opções.

##### <a name="acquire-the-azure-ad-token-manually"></a>Adquirir o token do Azure AD manualmente
Pode utilizar `az login` e obter a versão mais recente token a partir do ficheiro .azure no seu diretório raiz.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Adquirir o token do Azure AD através de programação
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Depois de criar o principal de serviço, guarde o resultado. Agora pode utilizá-la para obter um token do Azure AD:

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

O passo de registo do modelo regista o seu modelo de Machine Learning com a conta de gestão de modelo do Azure que criou. Este registo permite que os modelos e as respetivas versões que estão atribuídos ao tempo de registo de controlo. O utilizador fornece o nome do modelo. Registo subsequente dos modelos com o mesmo nome gera uma nova versão e um ID.

### <a name="request"></a>Pedir

| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Descrição
Regista um modelo.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| modelo | corpo | Payload que é utilizada para registar um modelo. | Sim | [Modelo](#model) |


### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | OK. O registo do modelo foi concluída com êxito. | [Modelo](#model) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>A lista de modelos de uma conta de consulta
### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Descrição
Consulta a lista de modelos de uma conta. Pode filtrar a lista de resultados por etiqueta e o nome. Se nenhum filtro é transmitido, a consulta lista todos os modelos na conta. A lista devolvida é paginated e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| nome | consulta | Nome do objeto. | Não | string |
| etiqueta | consulta | Etiqueta de modelo. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedModelList](#paginatedmodellist) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Obter os detalhes de modelo
### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
 |

### <a name="description"></a>Descrição
Obtém um modelo por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de objeto. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Modelo](#model) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Registar um manifesto com o modelo registado e todas as dependências

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Descrição
Regista um manifesto com o modelo registado e todas as respetivas dependências.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| manifestRequest | corpo | Payload que é utilizada para registar um manifesto. | Sim | [Manifesto](#manifest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Registo de manifesto foi concluída com êxito. | [Manifesto](#manifest) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>A lista de manifestos numa conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Descrição
Consulta a lista de manifestos numa conta. Pode filtrar a lista de resultados por ID de modelo e nome de manifesto. Se nenhum filtro é transmitido, a consulta lista todos os manifestos na conta. A lista devolvida é paginated e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| modelId | consulta | ID de modelo. | Não | string |
| ManifestName | consulta | Nome do manifesto. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedManifestList](#paginatedmanifestlist) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Obter os detalhes de manifesto

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

### <a name="description"></a>Descrição
Obtém o manifesto por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de objeto. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Manifesto](#manifest) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Criar uma imagem

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Descrição
Cria uma imagem como uma imagem de Docker no registo de contentor do Azure.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| imageRequest | corpo | Payload que é utilizado para criar uma imagem. | Sim | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Cabeçalhos | Esquema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | URL de localização de operação assíncrona. Uma chamada GET irá mostrar o estado da tarefa de criação de imagem. | Localização de operação |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>A lista de imagens numa conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Descrição
Consulta a lista de imagens de uma conta. Pode filtrar a lista de resultados ao manifesto ID e nome. Se nenhum filtro é transmitido, a consulta lista todas as imagens na conta. A lista devolvida é paginated e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| manifestId | consulta | ID de manifesto. | Não | string |
| ManifestName | consulta | Nome do manifesto. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedImageList](#paginatedimagelist) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Obter os detalhes da imagem

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

### <a name="description"></a>Descrição
Obtém uma imagem por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de imagem. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Imagem](#image) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Criar um serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Descrição
Cria um serviço a partir de uma imagem.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| serviceRequest | corpo | Payload que é utilizado para criar um serviço. | Sim | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Cabeçalhos | Esquema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | URL de localização de operação assíncrona. Uma chamada GET irá mostrar o estado da tarefa de criação de serviço. | Localização de operação |
| 409 | Já existe um serviço com o nome especificado. |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>A lista de serviços de uma conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Descrição
Consulta a lista de serviços de uma conta. Pode filtrar a lista de resultados por nome/ID de modelo, manifesto/ID de nome, ID de imagem, nome do serviço ou ID de recurso de computação para Machine Learning. Se nenhum filtro é transmitido, a consulta lista todos os serviços na conta. A lista devolvida é paginated e a contagem de itens em cada página é um parâmetro opcional.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| serviceName | consulta | Nome do serviço. | Não | string |
| modelId | consulta | Nome do modelo. | Não | string |
| modelName | consulta | ID de modelo. | Não | string |
| manifestId | consulta | ID de manifesto. | Não | string |
| ManifestName | consulta | Nome do manifesto. | Não | string |
| imageId | consulta | ID de imagem. | Não | string |
| computeResourceId | consulta | ID de recurso de computação Machine Learning. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedServiceList](#paginatedservicelist) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Obter os detalhes do serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Descrição
Obtém um serviço por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de objeto. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [ServiceResponse](#serviceresponse) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Atualizar um serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| COLOCAR |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Descrição
As atualizações de serviço existente.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de objeto. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| serviceUpdateRequest | corpo | Payload que é utilizado para atualizar um serviço existente. | Sim |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Cabeçalhos | Esquema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | URL de localização de operação assíncrona. Uma chamada GET irá mostrar o estado da tarefa de atualização do serviço. | Localização de operação |
| 404 | O serviço com o ID especificado não existe. |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Eliminar um serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| ELIMINAR |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Descrição
Elimina um serviço.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de objeto. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. |  |
| 204 | O serviço com o ID especificado não existe. |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Obter chaves de serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Descrição
Obtém as chaves de serviço.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID do serviço. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [AuthKeys](#authkeys)
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Voltar a gerar chaves de serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Descrição
Gera de novo chaves de serviço e devolve-os.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID do serviço. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| regenerateKeyRequest | corpo | Payload que é utilizado para atualizar um serviço existente. | Sim | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [AuthKeys](#authkeys)
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>A lista de implementações de uma conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

### <a name="description"></a>Descrição
Consulta a lista de implementações de uma conta. Pode filtrar a lista de resultados por ID de serviço, que irá devolver apenas as implementações criadas para o serviço específico. Se nenhum filtro é transmitido, a consulta lista todas as implementações na conta.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |
| serviceId | consulta | ID do serviço. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [DeploymentList](#deploymentlist) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Obter os detalhes da implementação

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

### <a name="description"></a>Descrição
Obtém a implementação por ID.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de implementação. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [Implementação](#deployment) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>Obter os detalhes de operação

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

### <a name="description"></a>Descrição
Obtém o estado da operação assíncrona por ID de operação.

### <a name="parameters"></a>Parâmetros
| Nome | Localizado na | Descrição | Necessário | Esquema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | caminho | ID da subscrição do Azure. | Sim | string |
| resourceGroupName | caminho | Nome do grupo de recursos no qual a conta de gestão de modelo está localizada. | Sim | string |
| accountName | caminho | Nome da conta de gestão de modelo. | Sim | string |
| ID | caminho | ID de operação. | Sim | string |
| versão de API | consulta | Versão do fornecedor de recursos Microsoft.Machine.Learning API a utilizar. | Sim | string |
| Autorização | cabeçalho | Token de autorização. Deve ser algo semelhante a "Portador XXXXXX." | Sim | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [OperationStatus](#asyncoperationstatus) |
| predefinido | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definições

<a name="asset"></a>
### <a name="asset"></a>Recurso
O objeto de recurso que será necessária durante a criação de imagens do Docker.


|Nome|Descrição|Esquema|
|---|---|---|
|**id**  <br>*Opcional*|ID de recurso.|string|
|**mimeType**  <br>*Opcional*|Tipo MIME de conteúdo de modelos. Para obter mais informações sobre o tipo de MIME, consulte o [lista de tipos de suportes de dados IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|string|
|**Descompactar**  <br>*Opcional*|Indica onde é necessário descompactar o conteúdo durante a criação de imagens do Docker.|boolean|
|**URL**  <br>*Opcional*|URL de localização do recurso.|string|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
O estado da operação assíncrona.

*Tipo*: enum (NotStarted, em execução, cancelada, com êxito, falha)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
O estado da operação.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdTime**  <br>*Opcional*  <br>*read-only*|Hora de criação de operação de assíncrona (UTC).|cadeia (data / hora)|
|**endTime**  <br>*Opcional*  <br>*read-only*|Hora de fim da operação assíncrona (UTC).|cadeia (data / hora)|
|**error**  <br>*Opcional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*Opcional*|ID de operação assíncrona.|string|
|**operationType**  <br>*Opcional*|Tipo de operação assíncrona.|enumeração (imagem, Service)|
|**resourceLocation**  <br>*Opcional*|Recursos criados ou atualizados pela operação assíncrona.|string|
|**state**  <br>*Opcional*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
As chaves de autenticação para um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**primaryKey**  <br>*Opcional*|Chave primária.|string|
|**secondaryKey**  <br>*Opcional*|Chave secundária.|string|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Definições para o autoscaler.


|Nome|Descrição|Esquema|
|---|---|---|
|**autoscaleEnabled**  <br>*Opcional*|Ativar ou desativar o autoscaler.|boolean|
|**maxReplicas**  <br>*Opcional*|Número máximo de réplicas pod até a dimensionar.  <br>**Valor mínimo**: `1`|número inteiro|
|**minReplicas**  <br>*Opcional*|Número mínimo de réplicas pod Dimensionar para baixo para.  <br>**Valor mínimo**: `0`|número inteiro|
|**refreshPeriodInSeconds**  <br>*Opcional*|Atualize o tempo para o acionador de dimensionamento automático.  <br>**Valor mínimo**: `1`|número inteiro|
|**targetUtilization**  <br>*Opcional*|Percentagem de utilização que aciona o dimensionamento automático.  <br>**Valor mínimo**: `0`  <br>**O valor máximo**: `100`|número inteiro|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
O recurso de computação de Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**id**  <br>*Opcional*|ID de recurso.|string|
|**tipo**  <br>*Opcional*|Tipo de recurso.|enumeração (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Configuração para reservar os recursos para um contentor no cluster.


|Nome|Descrição|Esquema|
|---|---|---|
|**cpu**  <br>*Opcional*|Especifica a reserva da CPU. Formato para Kubernetes: consulte [significado da CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|string|
|**Memória**  <br>*Opcional*|Especifica a reserva de memória. Formato para Kubernetes: consulte [significado da memória](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|string|


<a name="deployment"></a>
### <a name="deployment"></a>Implementação
Uma instância de uma implementação do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*Opcional*  <br>*read-only*|Hora de criação de implementação (UTC).|cadeia (data / hora)|
|**expiredAt**  <br>*Opcional*  <br>*read-only*|Implementação expirou hora (UTC).|cadeia (data / hora)|
|**id**  <br>*Opcional*|ID de implementação.|string|
|**imageId**  <br>*Opcional*|ID de imagem associado esta implementação.|string|
|**serviceName**  <br>*Opcional*|Nome do serviço.|string|
|**status**  <br>*Opcional*|Estado da implementação atual.|string|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Uma matriz de objetos de implementação.

*Tipo*: <[implementação](#deployment)> matriz


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Detalhe do erro de serviço de gestão de modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**código**  <br>*Necessário*|Código de erro.|string|
|**message**  <br>*Necessário*|Mensagem de erro.|string|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Um objeto de erro do serviço de gestão de modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**código**  <br>*Necessário*|Código de erro.|string|
|**Detalhes**  <br>*Opcional*|Matriz de objectos de detalhe do erro.|<[ErrorDetail](#errordetail)> matriz|
|**message**  <br>*Necessário*|Mensagem de erro.|string|
|**statusCode**  <br>*Opcional*|Código de estado HTTP.|número inteiro|


<a name="image"></a>
### <a name="image"></a>Imagem
A imagem do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**computeResourceId**  <br>*Opcional*|ID do ambiente criado no recurso de computação de Machine Learning.|string|
|**createdTime**  <br>*Opcional*|Hora de criação de imagens (UTC).|cadeia (data / hora)|
|**creationState**  <br>*Opcional*||[AsyncOperationState](#asyncoperationstate)|
|**Descrição**  <br>*Opcional*|Texto de descrição da imagem.|string|
|**error**  <br>*Opcional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*Opcional*|ID de imagem.|string|
|**imageBuildLogUri**  <br>*Opcional*|URI dos registos carregados na compilação de imagem.|string|
|**imageLocation**  <br>*Opcional*|Cadeia de localização de registo de contentor do Azure para a imagem de criação.|string|
|**imageType**  <br>*Opcional*||[ImageType](#imagetype)|
|**manifest**  <br>*Opcional*||[Manifesto](#manifest)|
|**name**  <br>*Opcional*|Nome da imagem.|string|
|**Versão**  <br>*Opcional*|Versão da imagem definido pelo serviço de gestão de modelo.|número inteiro|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Um pedido para criar uma imagem do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**computeResourceId**  <br>*Necessário*|ID do ambiente criado no recurso de computação de Machine Learning.|string|
|**Descrição**  <br>*Opcional*|Texto de descrição da imagem.|string|
|**imageType**  <br>*Necessário*||[ImageType](#imagetype)|
|**manifestId**  <br>*Necessário*|ID do manifesto a partir da qual a imagem será criada.|string|
|**name**  <br>*Necessário*|Nome da imagem.|string|


<a name="imagetype"></a>
### <a name="imagetype"></a>TipoImagem
Especifica o tipo da imagem.

*Tipo*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifesto
O manifesto do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**assets**  <br>*Necessário*|Lista de recursos.|<[Asset](#asset)> matriz|
|**createdTime**  <br>*Opcional*  <br>*read-only*|Hora de criação de manifesto (UTC).|cadeia (data / hora)|
|**Descrição**  <br>*Opcional*|Manifesto do texto de descrição.|string|
|**driverProgram**  <br>*Necessário*|Programa de controlador do manifesto.|string|
|**id**  <br>*Opcional*|ID de manifesto.|string|
|**modelIds**  <br>*Opcional*|Lista de IDs de modelo de modelos de registado. O pedido irá falhar se qualquer um dos modelos de incluídos não são registados.|<string> Matriz|
|**modelType**  <br>*Opcional*|Especifica se os modelos já estão registados com o serviço de gestão de modelo.|enumeração (registada)|
|**name**  <br>*Necessário*|Nome do manifesto.|string|
|**targetRuntime**  <br>*Necessário*||[TargetRuntime](#targetruntime)|
|**Versão**  <br>*Opcional*  <br>*read-only*|Versão do manifesto atribuído pelo serviço de gestão de modelo.|número inteiro|
|**webserviceType**  <br>*Opcional*|Especifica o tipo de serviço web que será criado a partir do manifesto pretendido.|enumeração (em tempo real)|


<a name="model"></a>
### <a name="model"></a>Modelo
Uma instância de um modelo do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*Opcional*  <br>*read-only*|Hora de criação de modelo (UTC).|cadeia (data / hora)|
|**Descrição**  <br>*Opcional*|Texto de descrição do modelo.|string|
|**id**  <br>*Opcional*  <br>*read-only*|ID de modelo.|string|
|**mimeType**  <br>*Necessário*|Tipo MIME do conteúdo do modelo. Para obter mais informações sobre o tipo de MIME, consulte o [lista de tipos de suportes de dados IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|string|
|**name**  <br>*Necessário*|Nome do modelo.|string|
|**etiquetas**  <br>*Opcional*|Lista de etiquetas de modelo.|<string> Matriz|
|**Descompactar**  <br>*Opcional*|Indica se é necessário descompactar o modelo durante a criação de imagens do Docker.|boolean|
|**URL**  <br>*Necessário*|URL do modelo. Normalmente, vamos colocar um URL de assinatura de acesso partilhado aqui.|string|
|**Versão**  <br>*Opcional*  <br>*read-only*|Versão de modelo atribuído pelo serviço de gestão de modelo.|número inteiro|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
As informações de recolha de dados do modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**eventHubEnabled**  <br>*Opcional*|Ative um hub de eventos para um serviço.|boolean|
|**storageEnabled**  <br>*Opcional*|Ative o armazenamento para um serviço.|boolean|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Uma lista paginated de imagens.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**valor**  <br>*Opcional*|Matriz de objetos de modelo.|<[Imagem](#image)> matriz|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Uma lista paginated de manifestos.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**valor**  <br>*Opcional*|Matriz de objetos de manifesto.|<[Manifesto](#manifest)> matriz|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Uma lista paginated de modelos.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**valor**  <br>*Opcional*|Matriz de objetos de modelo.|<[Modelo](#model)> matriz|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Uma lista paginated de serviços.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*Opcional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
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
|**imageId**  <br>*Necessário*|Imagem ao criar o serviço.|string|
|**maxConcurrentRequestsPerContainer**  <br>*Opcional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|número inteiro|
|**name**  <br>*Necessário*|Nome do serviço.|string|
|**numReplicas**  <br>*Opcional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se Autoscaler está ativado.  <br>**Valor mínimo**: `0`|número inteiro|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Um pedido para voltar a gerar uma chave para um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**keyType**  <br>*Opcional*|Especifica qual a chave para voltar a gerar.|enumeração (site primário, secundário)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Estado detalhado do serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*Opcional*|Hora de criação de serviço (UTC).|cadeia (data / hora)|
|**ID**  <br>*Opcional*|ID do serviço.|string|
|**image**  <br>*Opcional*||[Imagem](#image)|
|**manifest**  <br>*Opcional*||[Manifesto](#manifest)|
|**modelos**  <br>*Opcional*|Lista de modelos.|<[Modelo](#model)> matriz|
|**name**  <br>*Opcional*|Nome do serviço.|string|
|**scoringUri**  <br>*Opcional*|URI para o serviço de classificação.|string|
|**state**  <br>*Opcional*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*Opcional*|Hora (UTC) da última atualização.|cadeia (data / hora)|
|**appInsightsEnabled**  <br>*Opcional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*Opcional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Necessário*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*Opcional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*Opcional*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*Opcional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|número inteiro|
|**numReplicas**  <br>*Opcional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se Autoscaler está ativado.  <br>**Valor mínimo**: `0`|número inteiro|
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
|**imageId**  <br>*Opcional*|Imagem ao criar o serviço.|string|
|**maxConcurrentRequestsPerContainer**  <br>*Opcional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|número inteiro|
|**numReplicas**  <br>*Opcional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se Autoscaler está ativado.  <br>**Valor mínimo**: `0`|número inteiro|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
O tipo de tempo de execução de destino.


|Nome|Descrição|Esquema|
|---|---|---|
|**Propriedades**  <br>*Necessário*||< cadeia, cadeia > mapa|
|**runtimeType**  <br>*Necessário*|Especifica o tempo de execução.|enumeração (SparkPython, Python)|

