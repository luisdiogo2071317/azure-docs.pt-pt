---
title: Atributos de entidade do diário - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de diário na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ffb159dc684b4b6663dcb966706d4745ab88a403
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872583"
---
# <a name="journal-entity"></a>Entidade de diário

<sub> * Seguintes atributos são específicos para a entidade de diário. (Ty = '2') </sub>

Name    |Descrição                            |Type       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
DJN     |Nome do diário normalizado                |String     |nenhum
JN      |Nome a apresentar do diário                   |String     |Igual a
Cc      |Contagem de total de citação de diário           |Int32      |nenhum  
ECC     |Contagem de total de citação estimado de diário |Int32      |nenhum
