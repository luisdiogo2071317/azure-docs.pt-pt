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
ms.openlocfilehash: 1d7723f356274bbd18b1ea08e34046da82a1057c
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646358"
---
**Pergunta**: deve usar uma entidade hierárquica ou um padrão com uma entidade com funções? 

**Resposta**: que depende. Padrões e as expressões de exemplo são comparáveis em que eles representam a expressão de um usuário e são específicas para um objetivo.  

Se as expressões não tem um padrão não encriptado, utilize entidades hierárquicas. 

|entidades hierárquicas|Entidade Simple com as funções|
|--|--|
|tem de ter expressões de exemplo com entidades subordinadas rotulado em objetivos|tem de ter expressões de exemplo, **funções não podem ser rotulada como em intenções**|
|Pode utilizar em padrões|**tem** utilizar em padrões|
|poderá ter **mais** expressões de exemplo na intenção|poderá ter **menos** expressões de exemplo na intenção|
