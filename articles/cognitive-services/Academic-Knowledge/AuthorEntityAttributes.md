---
title: Criar atributos de entidade - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de autor na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 62e6da6d558a0494fb83115b1e307081099399d4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900694"
---
# <a name="author-entity"></a>Entidade de autor
<sub> * Seguintes atributos são específicos para a entidade de autor. (Ty = '1') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
AuN     |Nome do autor normalizado                 |Cadeia     |Igual a
DAuN    |Nome a apresentar do autor                    |Cadeia     |nenhum
Cc      |Contagem de total de citação do autor            |Int32      |nenhum  
ECC     |Contagem de total de citação estimado do autor  |Int32      |nenhum
E       |Metadados de Extended (consulte a tabela de "Atributos estendidos Meta")  |Cadeia     |nenhum  


## <a name="extended-metadata-attributes"></a>Atributos de metadados expandidos ##

Nome    | Descrição               
--------|---------------------------    
LKA. Afn     | nome a apresentar da afiliação associado com o autor  
LKA. AfId        | ID da entidade da afiliação associado com o autor