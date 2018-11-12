---
title: Compreender a autenticação e conectividade dos dispositivos duplos Digital do Azure | Documentos da Microsoft
description: Utilizar o gémeos Digital do Azure para ligar e autenticar dispositivos
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: 42c1b0fbb6d87e9ed35d4ecce3971d8512eed4d4
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012467"
---
# <a name="create-and-manage-role-assignments"></a>Criar e gerir atribuições de funções

Os gémeos Digital do Azure utiliza o controlo de acesso baseado em funções ([RBAC](./security-role-based-access-control.md)) para gerir o acesso aos recursos.

Cada atribuição de função inclui:

* **Identificador de objeto**: um ID do Azure Active Directory, o ID de objeto do principal de serviço ou o nome de domínio
* **Tipo de identificador de objeto**
* **ID de definição de função**
* **Caminho de espaço**
* **ID do inquilino**: na maioria dos casos, ID de inquilino de um Azure Active Directory

## <a name="role-definition-identifiers"></a>Identificadores da definição de função

A tabela seguinte mostra o que pode ser obtido consultando as API de sistema/funções.

| **Função** | **Identificador** |
| --- | --- |
| Administrador de espaço | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Administrador de Utilizadores| dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Administrador de Dispositivos | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Chave de administrador | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Administrador de token | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Utilizador | b1ffdb77-c635-4E7E-ad25-948237d85b30 |
| Especialista de suporte | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Instalador do dispositivo | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Dispositivo de gateway | d4c69766-e9bd-4e61-BFC1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>ObjectIdTypes suportados

O suporte `ObjectIdTypes`:

* `UserId`
* `DeviceId`
* `DomainName`
* `TenantId`
* `ServicePrincipalId`
* `UserDefinedFunctionId`

## <a name="create-a-role-assignment"></a>Criar uma atribuição de função

```plaintext
HTTP POST /api/v1.0/roleassignments
```

| **Nome** | **Necessário** | **Tipo** | **Descrição** |
| --- | --- | --- | --- |
| RoleId| Sim |Cadeia | O identificador de definição de função. Encontre as definições de função e seus identificadores consultando a API do sistema. |
| objectId | Sim |Cadeia | O ID de objeto para a atribuição de função têm de ser formatado vzhledem KE svému typu associado. Para o `DomainName` ObjectIdType, ObjectId tem de começar com o `“@”` caráter. |
| objectIdType | Sim |Cadeia | O tipo de atribuição de função. Tem de ser uma das seguintes linhas nesta tabela. |
| TenantId | Varia | Cadeia |O identificador do inquilino. Não são permitidas para `DeviceId` e `TenantId` ObjectIdTypes. Necessário para `UserId` e `ServicePrincipalId` ObjectIdTypes. Opcional para o DomainName ObjectIdType. |
| caminho * | Sim | Cadeia |O caminho de acesso total para o `Space` objeto. Um exemplo é `/{Guid}/{Guid}`. Se precisar de um identificador a atribuição de função para todo o gráfico, especifique `"/"`. Este caráter designa a raiz, mas a sua utilização não é recomendada. Seguem sempre o princípio do menor privilégio. |

## <a name="sample-configuration"></a>Configuração de exemplo

Neste exemplo, um utilizador tem acesso administrativo a um andar de um espaço de inquilino.

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

Neste exemplo, um aplicativo é executado a simulação de dispositivos e sensores de cenários de teste.

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Todos os utilizadores que fazem parte de um domínio recebem acesso de leitura para usuários, sensores e espaços. Este acesso inclui seus objetos relacionados correspondentes.

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

OBTER utilização para obter uma atribuição de função.

```plaintext
HTTP GET /api/v1/roleassignments?path={path}
```

| **Nome** | **No** | **Necessário** |    **Tipo** |  **Descrição** |
| --- | --- | --- | --- | --- |
| Caminho | Caminho | Verdadeiro | Cadeia | O caminho completo para o espaço |

Utilize a eliminação para eliminar uma atribuição de função.

```plaintext
HTTP DELETE /api/v1/roleassignments/{id}
```

| **Nome** | **No** | **Necessário** | **Tipo** | **Descrição** |
| --- | --- | --- | --- | --- |
| ID | Caminho | Verdadeiro | Cadeia |   ID de atribuição de função |

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [autenticação de API](./security-authenticating-apis.md).
