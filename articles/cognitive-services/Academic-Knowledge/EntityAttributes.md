---
title: Atributos de entidade do gráfico académicas para a API de conhecimento académico | Microsoft Docs
description: Saiba mais sobre os atributos de entidade que pode utilizar com o gráfico académico na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f771969c3c6f05e5d2c99b1fbf593ae039ccb12c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351349"
---
# <a name="entity-attributes"></a>Atributos de entidade

O gráfico académico é composto por 7 tipos de entidade. Todas as entidades irão ter um ID de entidade e um tipo de entidade.

## <a name="common-entity-attributes"></a>Atributos comuns de entidade
Nome    |Descrição                |Tipo       | Operações
------- | ------------------------- | --------- | ----------------------------
Id      |ID de entidade                  |Int64      |Igual a
Ty      |Tipo de entidade                |Enum   |Igual a

## <a name="entity-type-enum"></a>Enumeração do tipo de entidade
Nome                                                            |valor
----------------------------------------------------------------|-----
[Artigo](PaperEntityAttributes.md)                               |0
[Autor](AuthorEntityAttributes.md)                             |1
[Diário](JournalEntityAttributes.md)                           |2
[Série de conferência](JournalEntityAttributes.md)                 |3
[Instância de conferência](ConferenceInstanceEntityAttributes.md)    |4
[Afiliação](AffiliationEntityAttributes.md)                   |5
[Campo de prático](FieldsOfStudyEntityAttributes.md)                      |6

