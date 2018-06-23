---
title: Criar atributos de entidade na API de conhecimento académico | Microsoft Docs
description: Saiba os atributos que pode utilizar com a entidade de autor na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: afcc3257c49522a4631c4aae0e0c2b88373f9af1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351355"
---
# <a name="author-entity"></a>Entidade de autor
<sub> * Seguintes atributos são específicos para a entidade de autor. (Ty = '1') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
AuN     |Nome do autor normalizado                 |Cadeia     |Igual a
DAuN    |Nome a apresentar do autor                    |Cadeia     |nenhum
Cc      |Contagem de total de citação de autor            |Int32      |nenhum  
ECC     |Contagem de total citação estimado do autor  |Int32      |nenhum
E       |Metadados expandido (consulte a tabela "Expandido Meta atributos")  |Cadeia     |nenhum  


## <a name="extended-metadata-attributes"></a>Expandido atributos de metadados ##

Nome    | Descrição               
--------|---------------------------    
LKA. Afn     | nome a apresentar da afiliação associado do autor  
LKA. AfId        | ID de entidade do pedido de afiliação associado do autor