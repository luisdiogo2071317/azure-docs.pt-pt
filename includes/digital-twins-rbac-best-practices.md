---
title: incluir ficheiro
description: incluir ficheiro
services: azure-digital-twins
author: adamgerard
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/27/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 075587df445b46c8b03c5448cebf8cd8b12f1179
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324174"
---
Controlo de acesso baseado em funções é uma estratégia de segurança condicionada por herança para a gestão de acesso, permissões e funções. Funções descendentes herdarem permissões do funções do principal. Também podem ser atribuídas permissões sem a ser herdadas a partir de uma função principal. Eles também podem ser atribuídos para personalizar uma função, conforme necessário.

Por exemplo, um **administrador de espaço** poderá ter acesso global para executar todas as operações para um espaço especificado (incluindo todos os nós abaixo ou dentro da mesma), ao passo que um **instalador dispositivo** talvez só precisem *ler* e *atualizar* permissões para os dispositivos e sensores.

Em todos os casos, as funções devem ser concedidas **exatamente e não mais do que o acesso necessário** para satisfazer as suas tarefas pela **princípio de privilégios mínimos**, que concede uma identidade *única*:

* A quantidade de acesso necessário para concluir seu trabalho.
* Uma função apropriada e limitada a realização de seu trabalho.

>[!IMPORTANT]
> **Seguem sempre o princípio do menor privilégio**.

Dois outras importantes controlo de acesso baseado em funções práticas a seguir:

> [!div class="checklist"]
> * Realize auditorias periódicas atribuições de funções para verificar se cada função tem as permissões corretas.
> * E atribuições de funções devem ser limpos-up como indivíduos alterar funções ou atribuições.