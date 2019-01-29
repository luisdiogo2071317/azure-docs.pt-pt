---
title: Criar atributos de entidade - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de autor na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 48758ac9ec8c993bbdb490229ae20fcce1fb0a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175184"
---
# <a name="author-entity"></a>Entidade de autor
<sub> * Seguintes atributos são específicos para a entidade de autor. (Ty = '1') </sub>

Name    |Descrição                            |Type       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
AuN     |Nome do autor normalizado                 |Cadeia     |Igual a
DAuN    |Nome a apresentar do autor                    |Cadeia     |nenhum
Cc      |Contagem de total de citação do autor            |Int32      |nenhum  
ECC     |Contagem de total de citação estimado do autor  |Int32      |nenhum
E       |Metadados de Extended (consulte a tabela de "Atributos estendidos Meta")  |Cadeia     |nenhum  


## <a name="extended-metadata-attributes"></a>Atributos de metadados expandidos ##

Name    | Descrição               
--------|---------------------------    
LKA. Afn     | nome a apresentar da afiliação associado com o autor  
LKA.AfId        | ID da entidade da afiliação associado com o autor
