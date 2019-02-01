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
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 08328b3c872ce9e8a25db6075b00e0cee2fdf4fd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480298"
---
A aplicação cliente tem de saber se uma expressão não é significativo ou adequado para a aplicação. O **None** intenção é adicionada a cada aplicação como parte do processo de criação para determinar se uma expressão não pode ser respondida pela aplicação de cliente.

Se o LUIS devolve os **None** intenção para uma expressão, a aplicação cliente pode perguntar se o usuário deseja terminar a conversa ou dar mais instruções para continuar a conversa. 

> [!CAUTION] 
> Não deixe a **None** intenção vazia. 

1. Selecione **Intents** (Intenções) no painel esquerdo.

2. Selecione a intenção **None** (Nenhuma). Adicione três expressões com que o usuário poderá introduzir mas não são relevante para a sua aplicação de recursos humanos:

    | Expressões de exemplo|
    |--|
    |Cães que ladram são irritantes|
    |Encomendar uma piza para mim|
    |Pinguins no oceano|
