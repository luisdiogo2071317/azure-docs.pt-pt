---
title: Atributos de entidade de série de conferências - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos que pode utilizar com a entidade de série de conferências.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f9f28afd7005d7a61aa0d2f4dba69ca598034b52
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900753"
---
# <a name="conference-series-entity"></a>Entidade de série de conferências

<sub> * Seguintes atributos são específicos para a entidade de série de conferências. (Ty = "3") </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
CN      |Série de conferências normalizados nome      |Cadeia     |Igual a
DCN     |Nome de exibição de série de conferências         |Cadeia     |nenhum
Cc      |Contagem de citação de total de série de conferências         |Int32      |nenhum  
ECC     |Contagem de total de citação estimado de série de conferência   |Int32      |nenhum
F.FId   |Campo de ID de entidade de estudo associada com a série de conferências |Int64  | Igual a
F.FN    |Campo de nome de estudo associada com a série de conferências  | É igual a,<br/>StartsWith