---
title: Atributos de entidade do diário - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de diário na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 397293c17320ef75daa5c31af76d49183b6c435c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193833"
---
# <a name="journal-entity"></a>Entidade de diário

<sub> * Seguintes atributos são específicos para a entidade de diário. (Ty = '2') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
DJN     |Nome do diário normalizado                |Cadeia     |nenhum
JN      |Nome a apresentar do diário                   |Cadeia     |Igual a
Cc      |Contagem de total de citação de diário           |Int32      |nenhum  
ECC     |Contagem de total de citação estimado de diário |Int32      |nenhum
