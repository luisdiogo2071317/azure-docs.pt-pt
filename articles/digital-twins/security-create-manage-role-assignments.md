---
title: Noções básicas sobre a autenticação e conectividade dos dispositivos duplos Digital do Azure | Documentos da Microsoft
description: Utilização duplos Digital do Azure para ligar e autenticar os dispositivos
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: adfb4c369ea1b324da8562a5b0b245ebdecff602
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324189"
---
# <a name="create-and-manage-role-assignments"></a>Criar e gerir atribuições de funções

Os gémeos Digital do Azure utiliza o controlo de acesso baseado em funções ([RBAC](./security-role-based-access-control.md)) para gerir o acesso aos recursos.

Cada atribuição de função inclui:

* Uma **identificador de objeto** (um ID de diretório do Azure Active Directory, ID de objeto do principal de serviço ou nome de domínio).
* Uma **tipo de identificador de objeto**.
* R **ID de definição de função**.
* R **caminho de espaço**.
* (Na maioria dos casos) um Azure Active Directory **ID de inquilino**.

## <a name="role-definition-identifiers"></a>Identificadores da definição de função

A tabela abaixo mostra o que pode ser obtido consultando a API de funções do sistema:

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
| GatewayDevice | d4c69766-e9bd-4e61-BFC1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>ObjectIdTypes suportados

O suporte `ObjectIdTypes` são:

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
| RoleId| Sim |cadeia | O identificador de definição de função. Definições de funções e os seus identificadores podem ser encontrados através da consulta da API do sistema. |
| objectId | Sim |cadeia | O ID de objeto para a atribuição de função têm de ser formatado vzhledem KE svému typu associado. Para o `DomainName` ObjectIdType, ObjectId tem de começar com o `“@”` caráter. |
| objectIdType | Sim |cadeia | O tipo de atribuição de função. Tem de ser uma das seguintes linhas nesta tabela. |
| TenantId | Varia | cadeia |O identificador do inquilino. Não são permitidas para `DeviceId` e `TenantId` ObjectIdTypes. Necessário para `UserId` e `ServicePrincipalId` ObjectIdTypes. Opcional para o DomainName ObjectIdType. |
| caminho * | Sim | cadeia |O caminho de acesso total para o `Space` objeto. Por exemplo: `/{Guid}/{Guid}` se precisar de um identificador a atribuição de função para todo o gráfico, especifique `"/"` (que designa a raiz). No entanto, não recomendado a utilizar, ou seja e **sempre que deve seguir o princípio do menor privilégio**. |

## <a name="sample-configuration"></a>Configuração de exemplo

Um utilizador tem acesso administrativo a um andar de um espaço de inquilino:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

Um aplicativo que é executado testar cenários de dispositivos e sensores de simulação:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Todos os utilizadores faz parte de um domínio receberá acesso de leitura para os espaços, sensores e utilizadores, incluindo os respetivos objetos relacionados correspondentes:

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

Para obter uma atribuição de função:

```plaintext
HTTP GET /api/v1/roleassignments?path={path}
```

| **Nome** | **No** | **Necessário** |    **Tipo** |  **Descrição** |
| --- | --- | --- | --- | --- |
| Caminho | Caminho | Verdadeiro | Cadeia | O caminho completo para o espaço |

Para eliminar uma atribuição de função:

```plaintext
HTTP DELETE /api/v1/roleassignments/{id}
```

| **Nome** | **No** | **Necessário** | **Tipo** | **Descrição** |
| --- | --- | --- | --- | --- |
| ID | Caminho | Verdadeiro | Cadeia |   ID de atribuição de função |

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [autenticação de API](./security-authenticating-apis.md).
