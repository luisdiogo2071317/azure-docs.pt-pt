---
title: Atributos de entidade do diário - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de diário na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 45b2860b56e79c197d96998350131a142326157a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902812"
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