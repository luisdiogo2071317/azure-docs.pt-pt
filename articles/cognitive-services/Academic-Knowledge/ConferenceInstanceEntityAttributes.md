---
title: Atributos de entidade de instância de conferência - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de instância da conferência na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: db025f377a3fab2f788252db0c8e3555837a6de8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196111"
---
# <a name="conference-instance-entity"></a>Entidade de instância de conferência

<sub> * Seguintes atributos são específicos para a entidade de instância de conferência. (Ty = '4') </sub>

Name    |Descrição                            |Type       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
CIN     |Nome da conferência instância normalizado ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Cadeia     |Igual a
DCN     |Nome de exibição de instância de conferência ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Cadeia     |nenhum
CIL     |Localização da instância de conferência    |Cadeia     |É igual a,<br/>StartsWith
CISD    |Data de início da instância de conferência  |Date       |É igual a,<br/>IsBetween
CIED    |Data de fim da instância de conferência    |Date       |É igual a,<br/>IsBetween
CIARD   |Registo de abstrair data de vencimento da instância de conferência  |Date       |É igual a,<br/>IsBetween
CISDD   |Submissão de data de vencimento da instância de conferência     |Date       |É igual a,<br/>IsBetween
CIFVD   |Versão final data de vencimento da instância de conferência  |Date       |É igual a,<br/>IsBetween
CINDD   |Data de notificação da instância de conferência   |Date       |É igual a,<br/>IsBetween
CD.T    |Título de um evento de instância de conferência   |Date       |É igual a,<br/>IsBetween
CD.D    |Data de um evento de instância de conferência    |Date       |É igual a,<br/>IsBetween
PCS.CN  |Nome da série de conferências da instância |Cadeia     |Igual a
PCS.CId |ID de série de conferências da instância |Int64    |Igual a
Cc      |Contagem de citação total de instâncias de conferência           |Int32      |nenhum  
ECC     |Contagem de citação estimado total de instâncias de conferência |Int32      |nenhum


## <a name="extended-metadata-attributes"></a>Atributos de metadados expandidos ##

Name    | Descrição               
--------|---------------------------    
FN      | Instância da conferência nome completo
