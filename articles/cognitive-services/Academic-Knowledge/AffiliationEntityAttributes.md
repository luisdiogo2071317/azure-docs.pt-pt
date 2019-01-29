---
title: Atributos de entidade de afiliação na API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de afiliação na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 344b26b16f74cd44982e3c93fa69295792daa9a0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190649"
---
# <a name="affiliation-entity"></a>Afiliação de entidade

<sub> * Seguintes atributos são específicos para a entidade de afiliação. (Ty = '5') </sub>

Name    |Descrição                            |Type       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
AfN     |Nome de afiliação normalizado        |Cadeia     |Igual a
DAfN    |Nome a apresentar afiliação       |Cadeia     |nenhum
Cc      |Contagem de total citation de afiliação           |Int32      |nenhum  
ECC     |Contagem de total de citação estimado afiliação |Int32      |nenhum

## <a name="extended-metadata-attributes"></a>Atributos de metadados expandidos ##

Name    | Descrição               
--------|---------------------------    
PC      |Contagem de papel da afiliação
