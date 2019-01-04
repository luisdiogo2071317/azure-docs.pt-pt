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
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 0579b1e21e714e25e197cb5abf46793a38978d06
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2018
ms.locfileid: "53755668"
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