---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 10/23/2018
ms.author: diberry
ms.openlocfilehash: dee31fca841ea309128681637cc41fa0fb1e7aea
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480603"
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
