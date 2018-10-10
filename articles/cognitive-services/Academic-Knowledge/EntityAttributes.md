---
title: Atributos de entidade Academic Graph - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos de entidade que pode utilizar com o gráfico para instituições académicas na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a203fdf6562dabb1b9d6e8ab5bb8f46ff6d5dc27
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902795"
---
# <a name="entity-attributes"></a>Atributos de entidade

O gráfico para instituições académicas é composta por 7 tipos de entidade. Todas as entidades terão um ID de entidade e um tipo de entidade.

## <a name="common-entity-attributes"></a>Atributos de entidade comuns
Nome    |Descrição                |Tipo       | Operações
------- | ------------------------- | --------- | ----------------------------
Id      |ID de entidade                  |Int64      |Igual a
Ty      |Tipo de entidade                |Enum   |Igual a

## <a name="entity-type-enum"></a>Enumeração de tipo de entidade
Nome                                                            |valor
----------------------------------------------------------------|-----
[Artigo](PaperEntityAttributes.md)                               |0
[Autor](AuthorEntityAttributes.md)                             |1
[Diário](JournalEntityAttributes.md)                           |2
[Série de conferências](JournalEntityAttributes.md)                 |3
[Instância de conferência](ConferenceInstanceEntityAttributes.md)    |4
[Afiliação](AffiliationEntityAttributes.md)                   |5
[Campo de estudo](FieldsOfStudyEntityAttributes.md)                      |6

