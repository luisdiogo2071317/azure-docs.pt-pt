---
title: Compreender como as funções são utilizadas nas entidades baseada em padrões - Azure | Documentos da Microsoft
description: Saiba como uma função é utilizada numa entidade com base no padrão para dar um nome para um subtipo de entidade contextuais.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry
ms.openlocfilehash: d2692cdce9da7428bd7b30c4feaf7347792618f5
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222708"
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

```
buy a ticket from {Location:origin} to {Location:destination}
```

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
