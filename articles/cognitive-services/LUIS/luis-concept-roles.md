---
title: Funções para entidades
titleSuffix: Azure Cognitive Services
description: As funções são subtipos contextuais, com o nome de uma entidade usado apenas em padrões. Por exemplo, na expressão `buy a ticket from New York to London`, nova York e Londres cidades mas cada um tem um significado diferente da sentença. Nova Iorque é a cidade de origem e Londres é a cidade de destino.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: diberry
ms.openlocfilehash: 958194d49cd403caeaf9dd21dd90a02cab098e45
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55881462"
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


[!INCLUDE [H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="example-role-for-entities"></a>Função de exemplo para entidades

Uma função é apenas um posicionamento contextualmente adquirido de uma entidade dentro de uma expressão. É mais eficaz quando a expressão tem mais do que um desse tipo de entidade. O exemplo mais fácil para qualquer tipo de entidade é para distinguir entre um e para localização. A localização pode ser representada em muitos tipos de entidade diferentes. 

Um caso de utilização de exemplo é a transferência de um funcionário de um departamento para outro em que cada departamento é um item numa lista. Por exemplo: 

`Move [PersonName] from [Department:from] to [Department:to]`. 

Na predição retornada, as duas entidades de departamento vão ser devolvidas na resposta JSON e cada um irá incluir o nome da função. 

## <a name="roles-with-prebuilt-entities"></a>Funções com entidades previamente concebidas

Utilize as funções com entidades previamente concebidas para dar o significado diferentes instâncias da entidade pré-criados dentro de uma expressão. 

### <a name="roles-with-datetimev2"></a>Funções com datetimeV2

A entidade pré-criados, datetimeV2, realiza um excelente trabalho de compreender uma vasta gama de variedade em datas e horas em expressões. Pode pretender especificar datas e intervalos de datas diferente das noções básicas sobre padrão da entidade pré-criados. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como adicionar [funções](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
