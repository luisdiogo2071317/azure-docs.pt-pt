---
title: Compreender como as funções são usadas com base no padrão de entidades
titleSuffix: Azure Cognitive Services
description: As funções são subtipos contextuais, com o nome de uma entidade usado apenas em padrões. Por exemplo, a compra de expressão, um pedido de nova York para Londres, nova York e Londres são cidades mas cada um tem um significado diferente da sentença. Nova Iorque é a cidade de origem e Londres é a cidade de destino.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 033e5e5e054b0a29961ad60e72b1466b51d1df0c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035203"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Funções de entidade em padrões são subtipos contextuais
As funções são subtipos contextuais, com o nome de uma entidade utilizada apenas num [padrões](luis-concept-patterns.md).

Por exemplo, na expressão `buy a ticket from New York to London`, nova York e Londres cidades mas cada um tem um significado diferente da sentença. Nova Iorque é a cidade de origem e Londres é a cidade de destino. 

As funções oferecem um nome para essas diferenças:

|Entidade|Função|Objetivo|
|--|--|--|
|Localização|origem|onde o plano de deixa de|
|Localização|Destino|onde o plano que chegam|

## <a name="how-are-roles-used-in-patterns"></a>Como o funções são usadas em padrões?
Na expressão de modelo de um padrão, as funções são usadas dentro da expressão: 

|Padrão com funções de entidade|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Sintaxe de função em padrões
A entidade e a função estão entre parênteses, `{}`. A entidade e a função são separados por vírgula. 

## <a name="roles-versus-hierarchical-entities"></a>Funções versus entidades hierárquicas
Entidades hierárquicas fornecem as mesmas informações contextuais, como funções, mas apenas para expressões com na **intenções**. Da mesma forma, as funções fornecem as mesmas informações contextuais entidades hierárquicas mas apenas numa **padrões**.

|Contextual learning|Utilizado no|
|--|--|
|entidades hierárquicas|Objetivos|
|funções|Padrões de|

## <a name="next-steps"></a>Passos Seguintes

* Saiba como adicionar [funções](luis-how-to-add-entities.md#add-role-to-pattern-based-entity)
