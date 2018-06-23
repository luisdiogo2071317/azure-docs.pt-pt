---
title: Atributos de entidade de instância de conferência na API de conhecimento académico | Microsoft Docs
description: Saiba os atributos que pode utilizar com a entidade de instância de conferência na API de conhecimento académico nos serviços cognitivos.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ef2bca4346a4666905f3dfb7bd448720f3b0ef8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351397"
---
# <a name="conference-instance-entity"></a>Entidade de instância de conferência

<sub> * Seguintes atributos são específicos para a entidade de instância de conferência. (Ty = "4") </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                              |Int64      |Igual a
CIN     |Conferência normalizado o nome da instância ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Cadeia     |Igual a
DCN     |Nome de apresentação da conferência instância ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Cadeia     |nenhum
CIL     |Localização da instância conferência    |Cadeia     |É igual a,<br/>StartsWith
CISD    |Data de início da instância conferência  |Date       |É igual a,<br/>IsBetween
CIED    |Data de fim da instância conferência    |Date       |É igual a,<br/>IsBetween
CIARD   |Separar o registo devida data da instância conferência  |Date       |É igual a,<br/>IsBetween
CISDD   |Submissão devida data da instância conferência     |Date       |É igual a,<br/>IsBetween
CIFVD   |Versão final devida data da instância conferência  |Date       |É igual a,<br/>IsBetween
CINDD   |Data de notificação da instância de conferência   |Date       |É igual a,<br/>IsBetween
CD. T    |Título de um evento de instância de conferência   |Date       |É igual a,<br/>IsBetween
CD. D    |Data de um evento de instância de conferência    |Date       |É igual a,<br/>IsBetween
PCS. CN  |Nome da série Conferência da instância |Cadeia     |Igual a
PCS. CId |Conferência série ID da instância |Int64    |Igual a
Cc      |Conferência citação de total de instâncias           |Int32      |nenhum  
ECC     |Conferência citação estimado total de instâncias |Int32      |nenhum


## <a name="extended-metadata-attributes"></a>Expandido atributos de metadados ##

Nome    | Descrição               
--------|---------------------------    
FN      | Instância de conferência nome completo