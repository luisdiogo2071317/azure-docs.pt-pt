---
title: Atributos de entidade de afiliação na API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de afiliação na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 177fe9da8bbe821a69eae02d89a225e5d4009331
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900483"
---
# <a name="affiliation-entity"></a>Afiliação de entidade

<sub> * Seguintes atributos são específicos para a entidade de afiliação. (Ty = "5") </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
AfN     |Nome de afiliação normalizado        |Cadeia     |Igual a
DAfN    |Nome a apresentar afiliação       |Cadeia     |nenhum
Cc      |Contagem de total citation de afiliação           |Int32      |nenhum  
ECC     |Contagem de total de citação estimado afiliação |Int32      |nenhum

## <a name="extended-metadata-attributes"></a>Atributos de metadados expandidos ##

Nome    | Descrição               
--------|---------------------------    
PC      |Contagem de papel da afiliação