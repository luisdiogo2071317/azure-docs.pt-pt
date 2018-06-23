---
title: Os atributos de entidade do campo de prático na API de conhecimento académico | Microsoft Docs
description: Saiba os atributos que pode utilizar com a entidade de campo de prático na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: a8176370f5b2f63b7741f7e892ed5a8c775f19c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351361"
---
# <a name="field-of-study-entity"></a>Campo de prático entidade

<sub> * Seguintes atributos são específicos para o campo de prático de entidade. (Ty = '6') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
FN      |Campo de nome normalizado prático         |Cadeia     |Igual a
DFN     |Campo de nome a apresentar prático            |Cadeia     |nenhum
Cc      |Campo de contagem de total de citação de prático    |Int32      |nenhum  
ECC     |Campo de contagem de total citação estimado|Int32      |nenhum
FL      |Nível de campos de prático de hierarquia     |Int32      |É igual a, <br/>IsBetween
FP. FN   |Campo de principal do nome de prático             |Cadeia     |Igual a
FP. FId  |Campo de principal do ID de prático               |Int64      |Igual a
FC. FN   |Campo de subordinados de prático de nome              |Cadeia     |Igual a
FC. FId  |Campo de subordinados do ID de prático                |Int64      |Igual a