---
title: Atributos de entidade de série de conferência na API de conhecimento académico | Microsoft Docs
description: Saiba mais sobre os atributos que pode utilizar com a entidade de série de conferência nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 332736c927bdaa00334546f626a6eabb8e11d3b5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351362"
---
# <a name="conference-series-entity"></a>Entidade de série de conferência

<sub> * Seguintes atributos são específicos para a entidade de série de conferência. (Ty = "3") </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
CN      |Nome da conferência série normalizado      |Cadeia     |Igual a
DCN     |Nome a apresentar séries conferência         |Cadeia     |nenhum
Cc      |Contagem de citação de total de série de conferência         |Int32      |nenhum  
ECC     |Contagem de total citação estimado de série de conferência   |Int32      |nenhum
F.FId   |Campo de ID de entidade prático associado à série conferência |Int64  | Igual a
F.FN    |Campo de nome de prático associada à série conferência  | É igual a,<br/>StartsWith