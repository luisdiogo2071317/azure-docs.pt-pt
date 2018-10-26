---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: 0ad3b360611a12b95568e9a20f13a57c93b2e603
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086121"
---
## <a name="roles-versus-hierarchical-entities"></a>Funções versus entidades hierárquicas

Deve usar uma entidade hierárquica ou um padrão com uma entidade com funções? 

Que depende. Padrões e as expressões de exemplo são comparáveis em que eles representam a expressão de um usuário e são específicas para um objetivo.  

Se as expressões não tem um padrão não encriptado, utilize entidades hierárquicas. 

|entidades hierárquicas|Entidade Simple com as funções|
|--|--|
|Disponível em expressões de exemplo e padrões da intenção|Disponível apenas em padrões|
|Tem de ter expressões de exemplo na intenção com entidades subordinadas rotulado|Não podem ser rotulada como funções em expressões de exemplo na intenção|
|Poderá ter **mais** expressões de exemplo em intenção, para extrair a entidade|Caso precise **menos** expressões de exemplo na intenção|
