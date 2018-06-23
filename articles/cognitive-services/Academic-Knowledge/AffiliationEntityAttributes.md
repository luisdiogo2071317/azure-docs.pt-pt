---
title: Atributos de entidade do pedido de afiliação na API de conhecimento académico | Microsoft Docs
description: Saiba os atributos que pode utilizar com a entidade do pedido de afiliação na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a0ec67cb811ca207b3d038028491da2516028f0b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351350"
---
# <a name="affiliation-entity"></a>Afiliação de entidade

<sub> * Seguintes atributos são específicos para a entidade do pedido de afiliação. (Ty = "5") </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
AfN     |Nome normalizado afiliação        |Cadeia     |Igual a
DAfN    |Nome a apresentar afiliação       |Cadeia     |nenhum
Cc      |Contagem de total de citação de afiliação           |Int32      |nenhum  
ECC     |Contagem de total citação estimado afiliação |Int32      |nenhum

## <a name="extended-metadata-attributes"></a>Expandido atributos de metadados ##

Nome    | Descrição               
--------|---------------------------    
PC      |Contagem de documento do afiliação