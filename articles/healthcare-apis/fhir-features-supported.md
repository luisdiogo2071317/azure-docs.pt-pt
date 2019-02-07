---
title: Funcionalidades FHIR suportadas no Azure - FHIR na API do Azure
description: Este artigo explica as funcionalidades da especificação FHIR que estão implementadas na API do Azure para FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: a2851272d15ed2ce2c7f940cc3e429ef9f8b4446
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824145"
---
# <a name="features"></a>Funcionalidades

API do Azure para FHIR fornece uma implementação completamente gerida do Microsoft FHIR servidor para o Azure. O servidor é uma implementação do [FHIR](https://hl7.org/fhir) padrão. Este documento apresenta os principais recursos do servidor FHIR.

## <a name="fhir-version"></a>Versão FHIR

versão atual: `3.0.1`

## <a name="rest-api"></a>API REST

| API                            | Suportadas | Comentário |
|--------------------------------|-----------|---------|
| ler                           | Sim       |         |
| vread                          | Sim       |         |
| update                         | Sim       |         |
| Atualizar com bloqueio otimista | Sim       |         |
| Atualizar (condicional)           | Não        |         |
| Patch                          | Não        |         |
| delete                         | Sim       |         |
| Eliminar (condicional)           | Não        |         |
| criar                         | Sim       | Suporte a ambos os PUT/POST |
| criar (condicional)           | Não        |         |
| procurar                         | Parcial   | Veja abaixo |
| Capacidades                   | Sim       |         |
| lote                          | Não        |         |
| Transação                    | Não        |         |
| Histórico                        | Sim       |         |
| Paginação                         | Parcial   | `self` e `next` são suportados |
| intermediários                 | Não        |         |

## <a name="search"></a>Pesquisa

Todos os tipos de parâmetro de pesquisa são suportados. Em cadeia de parâmetros e são de encadeamento inverso *não* suportado.

| Tipo de parâmetro de pesquisa | Suportadas | Comentário |
|-----------------------|-----------|---------|
| Number                | Sim       |         |
| Date/DateTime         | Sim       |         |
| String                | Sim       |         |
| Certificado de                 | Sim       |         |
| Referência             | Sim       |         |
| Composição             | Sim       |         |
| Quantidade              | Sim       | Problema [#103](https://github.com/Microsoft/fhir-server/issues/103) |
| URI                   | Sim       |         |


| Modificadores             | Suportadas | Comentário |
|-----------------------|-----------|---------|
|`:missing`             | Sim       |         |
|`:exact`               | Sim       |         |
|`:contains`            | Sim       |         |
|`:text`                | Sim       |         |
|`:in` (token)          | Não        |         |
|`:below` (token)       | Não        |         |
|`:above` (token)       | Não        |         |
|`:not-in` (token)      | Não        |         |
|`:[type]` (referência)  | Não        |         |
|`:below` (uri)         | Sim       |         |
|`:above` (uri)         | Não        | Problema [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Parâmetro comum de pesquisa | Suportadas | Comentário |
|-------------------------| ----------|---------|
| `_id`                   | Sim       |         |
| `_lastUpdated`          | Sim       |         |
| `_tag`                  | Sim       |         |
| `_profile`              | Sim       |         |
| `_security`             | Sim       |         |
| `_text`                 | Não        |         |
| `_content`              | Não        |         |
| `_list`                 | Não        |         |
| `_has`                  | Não        |         |
| `_type`                 | Sim       |         |
| `_query`                | Não        |         |

| Procurar operações       | Suportadas | Comentário |
|-------------------------|-----------|---------|
| `_filter`               | Não        |         |
| `_sort`                 | Não        |         |
| `_score`                | Não        |         |
| `_count`                | Sim       |         |
| `_summary`              | Parcial   | `_summary=count` é suportada |
| `_include`              | Não        |         |
| `_revinclude`           | Não        |         |
| `_contained`            | Não        |         |
| `_elements`             | Não        |         |

## <a name="persistence"></a>Persistência

A Microsoft FHIR Server tem um módulo de persistência conectável (veja [ `Microsoft.Health.Fhir.Core.Features.Persistence` ](https://github.com/Microsoft/fhir-server/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Atualmente, o código-fonte aberto FHIR Server inclui uma implementação para [do Azure Cosmos DB](../cosmos-db/index-overview.md).

O cosmos DB é uma base de dados distribuído globalmente vários modelos (API de SQL, API de MongoDB, etc.). Ele oferece suporte a diferentes [níveis de consistência](../cosmos-db/consistency-levels.md). O modelo de implementação do padrão configura o servidor de FHIR com `Strong` consistência, mas a política de consistência pode ser modificado (geralmente flexíveis) sobre como utilizar uma base de pedido por pedido o `x-ms-consistency-level` cabeçalho do pedido.

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

O servidor de FHIR utiliza [do Azure Active Directory](https://azure.microsoft.com/services/active-directory/) para controlo de acesso. Especificamente, controlo de acesso baseado em ' (RBAC) da função é aplicada, caso a `FhirServer:Security:Enabled` parâmetro de configuração está definido como `true`e todos os pedidos (exceto `/metadata`) para o servidor de FHIR tem de ter `Authorization` cabeçalho de pedido definido como `Bearer <TOKEN>`. O token tem de conter uma ou mais funções conforme definido no `roles` de afirmação. Uma solicitação será autorizada se o token contém uma função que permite a ação especificada no recurso especificado.

Atualmente, as ações permitidas para uma determinada função são aplicadas *globalmente* na API.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, leu sobre as funcionalidades FHIR suportadas na API do Azure para FHIR. Em seguida, implante uma API de FHIR no Azure.
 
>[!div class="nextstepaction"]
>[Implementar o servidor Open FHIR de origem](fhir-oss-powershell-quickstart.md)