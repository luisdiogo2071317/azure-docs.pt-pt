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
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: dd99218765c873120c4117a3ec1712fe0a605e66
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480370"
---
**Pergunta**: deve usar uma entidade hierárquica ou um padrão com uma entidade com funções? 

**Resposta**: que depende. Padrões e as expressões de exemplo são comparáveis em que eles representam a expressão de um usuário e são específicas para um objetivo.  

Se as expressões não tem um padrão não encriptado, utilize entidades hierárquicas. 

|entidades hierárquicas|Entidade Simple com as funções|
|--|--|
|tem de ter expressões de exemplo com entidades subordinadas rotulado em objetivos|tem de ter expressões de exemplo, **funções não podem ser rotulada como em intenções**|
|Pode utilizar em padrões|**tem** utilizar em padrões|
|poderá ter **mais** expressões de exemplo na intenção|poderá ter * * menos expressões de exemplo na intenção|