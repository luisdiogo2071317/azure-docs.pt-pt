---
title: Atributos de entidade de campo de estudo - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de campo de estudo na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 862fd6d506d5f1ca6f7f532f80f53a29200f33db
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900432"
---
# <a name="field-of-study-entity"></a>Campo de entidade de estudo

<sub> * Seguintes atributos são específicos para o campo de estudo de entidade. (Ty = "6") </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
FN      |Campo de nome de normalizado de estudo         |Cadeia     |Igual a
DFN     |Campo de nome de exibição de estudo            |Cadeia     |nenhum
Cc      |Campo de contagem de total de citação de estudo    |Int32      |nenhum  
ECC     |Campo de contagem de total de citação estimado|Int32      |nenhum
FL      |Nível de campos de hierarquia de estudo     |Int32      |É igual a, <br/>IsBetween
FP. FN   |Campo de principal do nome de estudo             |Cadeia     |Igual a
FP. FId  |Campo de principal do ID de estudo               |Int64      |Igual a
FC. FN   |Campo de subordinados do nome de estudo              |Cadeia     |Igual a
FC. FId  |Campo de subordinado de ID de estudo                |Int64      |Igual a