---
title: Compreender como as funções são usadas com base no padrão de entidades
titleSuffix: Azure Cognitive Services
description: As funções são subtipos contextuais, com o nome de uma entidade usado apenas em padrões. Por exemplo, a compra de expressão, um pedido de nova York para Londres, nova York e Londres são cidades mas cada um tem um significado diferente da sentença. Nova Iorque é a cidade de origem e Londres é a cidade de destino.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: fea70e2b1912bde94e23dbd98fa933784d2a69ac
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140320"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Funções de entidade em padrões são subtipos contextuais
As funções são subtipos contextuais, com o nome de uma entidade utilizada apenas num [padrões](luis-concept-patterns.md).

Por exemplo, na expressão `buy a ticket from New York to London`, nova York e Londres cidades mas cada um tem um significado diferente da sentença. Nova Iorque é a cidade de origem e Londres é a cidade de destino. 

As funções oferecem um nome para essas diferenças:

|Entidade|Função|Objetivo|
|--|--|--|
|Localização|origem|onde o plano de deixa de|
|Localização|Destino|onde o plano que chegam|
|DatetimeV2 pré-criados|para|data de fim|
|DatetimeV2 pré-criados|de|data de início|

## <a name="how-are-roles-used-in-patterns"></a>Como o funções são usadas em padrões?
Na expressão de modelo de um padrão, as funções são usadas dentro da expressão: 

|Padrão com funções de entidade|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Sintaxe de função em padrões
A entidade e a função estão entre parênteses, `{}`. A entidade e a função são separados por vírgula. 


[!include[H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="roles-with-prebuilt-entities"></a>Funções com entidades previamente concebidas

Utilize as funções com entidades previamente concebidas para dar o significado diferentes instâncias da entidade pré-criados dentro de uma expressão. 

### <a name="roles-with-datetimev2"></a>Funções com datetimeV2

A entidade pré-criados, datetimeV2, realiza um excelente trabalho de compreender uma vasta gama de variedade em datas e horas em expressões. Pode pretender especificar datas e intervalos de datas diferente das noções básicas sobre padrão da entidade pré-criados. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como adicionar [funções](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
