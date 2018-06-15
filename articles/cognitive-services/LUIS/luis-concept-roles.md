---
title: Compreender como as funções são utilizadas no baseado em padrão entidades - Azure | Microsoft Docs
description: Saiba como uma função é utilizada uma entidade com base em padrão para dar um nome para um subtipo de nível contextual das entidades.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: ab6100e33fb767528b87c6afde4c5ef275fc7c81
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356368"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Funções de entidade em padrões são subtipos contextuais
As funções são nomeados, contextuais subtipos de uma entidade utilizado apenas no [padrões](luis-concept-patterns.md).

Por exemplo, no utterance `buy a ticket from New York to London`, Nova Iorque e Londres são cidades, mas cada um tem um significado diferente no frase. Nova Iorque é a cidade de origem e Londres é a cidade de destino. 

As funções oferecem um nome dessas diferenças:

|Entidade|Função|Objetivo|
|--|--|--|
|Localização|origem|onde o plane deixa de|
|Localização|Destino|onde o plane lands|

## <a name="how-are-roles-used-in-patterns"></a>Como são utilizadas funções na padrões?
Utterance do modelo de um padrão, as funções são utilizadas dentro de utterance: 

```
buy a ticket from {Location:origin} to {Location:destination}
```

## <a name="role-syntax-in-patterns"></a>Sintaxe de função no padrões
A função de entidade e são rodeados parênteses, `{}`. A entidade e a função são separados por ponto e vírgula. 

## <a name="roles-versus-hierarchical-entities"></a>Funções versus hierárquicas entidades
Entidades hierárquicas fornecem as mesmas informações contextuais como funções, mas apenas para utterances no **pendentes**. Da mesma forma, funções de fornecem as mesmas informações contextuais como entidades hierárquicas, mas apenas em **padrões**.

|Contextual learning|Utilizado no|
|--|--|
|entidades hierárquicas|Pendentes|
|funções|Padrões|

## <a name="next-steps"></a>Passos Seguintes

* Saiba como adicionar [funções](luis-how-to-add-entities.md#add-role-to-pattern-based-entity)
