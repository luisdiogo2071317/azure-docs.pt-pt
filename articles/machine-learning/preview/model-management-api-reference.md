---
title: "Criar uma imagem de Docker para gestão de modelo no Azure Machine Learning | Microsoft Docs"
description: "Este artigo descreve os passos para criar uma imagem de Docker para o seu serviço web."
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
ms.openlocfilehash: f7f9cbd34d84f89d6ce193daf79531617c97ddd3
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
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
Certifique-se de que já leu os passos de instalação no [instalar e criar o guia de introdução](quickstart-installation.md) documento.

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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>A lista de modelos de uma conta de consulta
### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Obter os detalhes de modelo
### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

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
| manifestRequest | corpo | Payload que é utilizada para registar um manifesto. | Sim | [Manifest](#manifest) |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Registo de manifesto foi concluída com êxito. | [Manifest](#manifest) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>A lista de manifestos numa conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

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
| manifestName | consulta | Nome do manifesto. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedManifestList](#paginatedmanifestlist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Obter os detalhes de manifesto

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

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
| 200 | Êxito. | [Manifest](#manifest) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>A lista de imagens numa conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

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
| manifestName | consulta | Nome do manifesto. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedImageList](#paginatedimagelist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Obter os detalhes da imagem

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

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
| 200 | Êxito. | [Image](#image) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |


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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>A lista de serviços de uma conta de consulta

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

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
| manifestName | consulta | Nome do manifesto. | Não | string |
| imageId | consulta | ID de imagem. | Não | string |
| computeResourceId | consulta | ID de recurso de computação Machine Learning. | Não | string |
| contagem | consulta | Número de itens para obter uma página de início. | Não | string |
| $skipToken | consulta | Token de continuação para obter a página seguinte. | Não | string |

### <a name="responses"></a>Respostas
| Código | Descrição | Esquema |
|--------------------|--------------------|--------------------|
| 200 | Êxito. | [PaginatedServiceList](#paginatedservicelist) |
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Obter os detalhes do serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

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
| predefinição | Resposta de erro que descreve o motivo pelo qual a operação falhou. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Obter chaves de serviço

### <a name="request"></a>Pedir
| Método | URI de pedido |
|------------|------------|
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

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
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

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
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

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
| INTRODUÇÃO |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

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
|**id**  <br>*optional*|ID de recurso.|string|
|**mimeType**  <br>*optional*|Tipo MIME de conteúdo de modelos. Para obter mais informações sobre o tipo de MIME, consulte o [lista de tipos de suportes de dados IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|string|
|**unpack**  <br>*optional*|Indica onde é necessário descompactar o conteúdo durante a criação de imagens do Docker.|boolean|
|**url**  <br>*optional*|URL de localização do recurso.|string|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
O estado da operação assíncrona.

*Tipo*: enum (NotStarted, em execução, cancelada, com êxito, falha)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
O estado da operação.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdTime**  <br>*optional*  <br>*read-only*|Hora de criação de operação de assíncrona (UTC).|cadeia (data / hora)|
|**endTime**  <br>*optional*  <br>*read-only*|Hora de fim da operação assíncrona (UTC).|cadeia (data / hora)|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*optional*|ID de operação assíncrona.|string|
|**operationType**  <br>*optional*|Tipo de operação assíncrona.|enumeração (imagem, Service)|
|**resourceLocation**  <br>*optional*|Recursos criados ou atualizados pela operação assíncrona.|string|
|**state**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
As chaves de autenticação para um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**primaryKey**  <br>*optional*|Chave primária.|string|
|**secondaryKey**  <br>*optional*|Chave secundária.|string|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Definições para o autoscaler.


|Nome|Descrição|Esquema|
|---|---|---|
|**autoscaleEnabled**  <br>*optional*|Ativar ou desativar o autoscaler.|boolean|
|**maxReplicas**  <br>*optional*|Número máximo de réplicas pod até a dimensionar.  <br>**Valor mínimo**: `1`|integer|
|**minReplicas**  <br>*optional*|Número mínimo de réplicas pod Dimensionar para baixo para.  <br>**Valor mínimo**: `0`|integer|
|**refreshPeriodInSeconds**  <br>*optional*|Atualize o tempo para o acionador de dimensionamento automático.  <br>**Valor mínimo**: `1`|integer|
|**targetUtilization**  <br>*optional*|Percentagem de utilização que aciona o dimensionamento automático.  <br>**Valor mínimo**: `0`  <br>**O valor máximo**: `100`|integer|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
O recurso de computação de Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**id**  <br>*optional*|ID de recurso.|string|
|**type**  <br>*optional*|Tipo de recurso.|enumeração (Cluster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Configuração para reservar os recursos para um contentor no cluster.


|Nome|Descrição|Esquema|
|---|---|---|
|**cpu**  <br>*optional*|Especifica a reserva da CPU. Formato para Kubernetes: consulte [significado da CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|string|
|**memory**  <br>*optional*|Especifica a reserva de memória. Formato para Kubernetes: consulte [significado da memória](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|string|


<a name="deployment"></a>
### <a name="deployment"></a>Implementação
Uma instância de uma implementação do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*optional*  <br>*read-only*|Hora de criação de implementação (UTC).|cadeia (data / hora)|
|**expiredAt**  <br>*optional*  <br>*read-only*|Implementação expirou hora (UTC).|cadeia (data / hora)|
|**id**  <br>*optional*|ID de implementação.|string|
|**imageId**  <br>*optional*|ID de imagem associado esta implementação.|string|
|**serviceName**  <br>*optional*|Nome do serviço.|string|
|**status**  <br>*optional*|Estado da implementação atual.|string|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Uma matriz de objetos de implementação.

*Tipo*: <[implementação](#deployment)> matriz


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Detalhe do erro de serviço de gestão de modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**code**  <br>*required*|Código de erro.|string|
|**message**  <br>*required*|Mensagem de erro.|string|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Um objeto de erro do serviço de gestão de modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**code**  <br>*required*|Código de erro.|string|
|**details**  <br>*optional*|Matriz de objectos de detalhe do erro.|<[ErrorDetail](#errordetail)> matriz|
|**message**  <br>*required*|Mensagem de erro.|string|
|**statusCode**  <br>*optional*|Código de estado HTTP.|integer|


<a name="image"></a>
### <a name="image"></a>Imagem
A imagem do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**computeResourceId**  <br>*optional*|ID do ambiente criado no recurso de computação de Machine Learning.|string|
|**createdTime**  <br>*optional*|Hora de criação de imagens (UTC).|cadeia (data / hora)|
|**creationState**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|
|**description**  <br>*optional*|Texto de descrição da imagem.|string|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|
|**id**  <br>*optional*|ID de imagem.|string|
|**imageBuildLogUri**  <br>*optional*|URI dos registos carregados na compilação de imagem.|string|
|**imageLocation**  <br>*optional*|Cadeia de localização de registo de contentor do Azure para a imagem de criação.|string|
|**imageType**  <br>*optional*||[ImageType](#imagetype)|
|**manifest**  <br>*optional*||[Manifest](#manifest)|
|**name**  <br>*optional*|Nome da imagem.|string|
|**version**  <br>*optional*|Versão da imagem definido pelo serviço de gestão de modelo.|integer|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>ImageRequest
Um pedido para criar uma imagem do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**computeResourceId**  <br>*required*|ID do ambiente criado no recurso de computação de Machine Learning.|string|
|**description**  <br>*optional*|Texto de descrição da imagem.|string|
|**imageType**  <br>*required*||[ImageType](#imagetype)|
|**manifestId**  <br>*required*|ID do manifesto a partir da qual a imagem será criada.|string|
|**name**  <br>*required*|Nome da imagem.|string|


<a name="imagetype"></a>
### <a name="imagetype"></a>TipoImagem
Especifica o tipo da imagem.

*Tipo*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifesto
O manifesto do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**assets**  <br>*required*|Lista de recursos.|<[Asset](#asset)> matriz|
|**createdTime**  <br>*optional*  <br>*read-only*|Hora de criação de manifesto (UTC).|cadeia (data / hora)|
|**description**  <br>*optional*|Manifesto do texto de descrição.|string|
|**driverProgram**  <br>*required*|Programa de controlador do manifesto.|string|
|**id**  <br>*optional*|ID de manifesto.|string|
|**modelIds**  <br>*optional*|Lista de IDs de modelo de modelos de registado. O pedido irá falhar se qualquer um dos modelos de incluídos não são registados.|<string> Matriz|
|**modelType**  <br>*optional*|Especifica se os modelos já estão registados com o serviço de gestão de modelo.|enumeração (registada)|
|**name**  <br>*required*|Nome do manifesto.|string|
|**targetRuntime**  <br>*required*||[TargetRuntime](#targetruntime)|
|**version**  <br>*optional*  <br>*read-only*|Versão do manifesto atribuído pelo serviço de gestão de modelo.|integer|
|**webserviceType**  <br>*optional*|Especifica o tipo de serviço web que será criado a partir do manifesto pretendido.|enumeração (em tempo real)|


<a name="model"></a>
### <a name="model"></a>Modelo
Uma instância de um modelo do Azure Machine Learning.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*optional*  <br>*read-only*|Hora de criação de modelo (UTC).|cadeia (data / hora)|
|**description**  <br>*optional*|Texto de descrição do modelo.|string|
|**id**  <br>*optional*  <br>*read-only*|ID de modelo.|string|
|**mimeType**  <br>*required*|Tipo MIME do conteúdo do modelo. Para obter mais informações sobre o tipo de MIME, consulte o [lista de tipos de suportes de dados IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|string|
|**name**  <br>*required*|Nome do modelo.|string|
|**etiquetas**  <br>*optional*|Lista de etiquetas de modelo.|<string> Matriz|
|**unpack**  <br>*optional*|Indica se é necessário descompactar o modelo durante a criação de imagens do Docker.|boolean|
|**url**  <br>*required*|URL do modelo. Normalmente, vamos colocar um URL de assinatura de acesso partilhado aqui.|string|
|**version**  <br>*optional*  <br>*read-only*|Versão de modelo atribuído pelo serviço de gestão de modelo.|integer|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
As informações de recolha de dados do modelo.


|Nome|Descrição|Esquema|
|---|---|---|
|**eventHubEnabled**  <br>*optional*|Ative um hub de eventos para um serviço.|boolean|
|**storageEnabled**  <br>*optional*|Ative o armazenamento para um serviço.|boolean|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Uma lista paginated de imagens.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*optional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**value**  <br>*optional*|Matriz de objetos de modelo.|<[Imagem](#image)> matriz|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Uma lista paginated de manifestos.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*optional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**value**  <br>*optional*|Matriz de objetos de manifesto.|<[Manifesto](#manifest)> matriz|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Uma lista paginated de modelos.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*optional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**value**  <br>*optional*|Matriz de objetos de modelo.|<[Modelo](#model)> matriz|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Uma lista paginated de serviços.


|Nome|Descrição|Esquema|
|---|---|---|
|**nextLink**  <br>*optional*|Ligação de continuação (URI absoluto) para a página de resultados na lista seguinte.|string|
|**value**  <br>*optional*|Matriz de objetos do serviço.|<[ServiceResponse](#serviceresponse)> matriz|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Um pedido para criar um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**appInsightsEnabled**  <br>*optional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*required*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*required*|Imagem ao criar o serviço.|string|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|integer|
|**name**  <br>*required*|Nome do serviço.|string|
|**numReplicas**  <br>*optional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se Autoscaler está ativado.  <br>**Valor mínimo**: `0`|integer|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Um pedido para voltar a gerar uma chave para um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**keyType**  <br>*optional*|Especifica qual a chave para voltar a gerar.|enumeração (site primário, secundário)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Estado detalhado do serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**createdAt**  <br>*optional*|Hora de criação de serviço (UTC).|cadeia (data / hora)|
|**ID**  <br>*optional*|ID do serviço.|string|
|**image**  <br>*optional*||[Image](#image)|
|**manifest**  <br>*optional*||[Manifest](#manifest)|
|**modelos**  <br>*optional*|Lista de modelos.|<[Modelo](#model)> matriz|
|**name**  <br>*optional*|Nome do serviço.|string|
|**scoringUri**  <br>*optional*|URI para o serviço de classificação.|string|
|**state**  <br>*optional*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*optional*|Hora (UTC) da última atualização.|cadeia (data / hora)|
|**appInsightsEnabled**  <br>*optional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*required*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|integer|
|**numReplicas**  <br>*optional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se Autoscaler está ativado.  <br>**Valor mínimo**: `0`|integer|
|**error**  <br>*optional*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>ServiceUpdateRequest
Um pedido para atualizar um serviço.


|Nome|Descrição|Esquema|
|---|---|---|
|**appInsightsEnabled**  <br>*optional*|Ative o application insights para um serviço.|boolean|
|**autoScaler**  <br>*optional*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*optional*||[ContainerResourceReservation](#containerresourcereservation)|
|**dataCollection**  <br>*optional*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*optional*|Imagem ao criar o serviço.|string|
|**maxConcurrentRequestsPerContainer**  <br>*optional*|Número máximo de pedidos em simultâneo.  <br>**Valor mínimo**: `1`|integer|
|**numReplicas**  <br>*optional*|Número de réplicas de pod em execução em qualquer altura. Não é possível especificar se Autoscaler está ativado.  <br>**Valor mínimo**: `0`|integer|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
O tipo de tempo de execução de destino.


|Nome|Descrição|Esquema|
|---|---|---|
|**Propriedades**  <br>*required*||< cadeia, cadeia > mapa|
|**runtimeType**  <br>*required*|Especifica o tempo de execução.|enum (SparkPython, Python)|

