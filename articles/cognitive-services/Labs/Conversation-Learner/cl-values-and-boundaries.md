---
title: Configuração predefinida do conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba mais sobre a configuração de conversação Learner predefinida.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 56e2140b83bf1c5722a459c14f31b2b4b0ba6b15
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353948"
---
# <a name="default-values-and-boundaries"></a>Os valores predefinidos e limites

Este documento descreve a configuração predefinida de conversação Learner e limites de serviço de chave.

## <a name="default-configuration"></a>Configuração predefinida

Parâmetro | Valor predefinido
--- | --- 
Tempo limite de sessão predefinido | 30 minutos

## <a name="boundaries"></a>Limites

Parâmetro | Limite
--- | --- 
API de criação, chama o HTTP máximo por mês | 5M
API de criação, chama o HTTP máxima por segundo | 25
Sessão API, máximo de HTTP de chamadas por mês | 500 K
Sessão API, máximo de HTTP de chamadas por segundo | 10
Número máx. de entidades (não programático) personalizados por aplicação | Consulte [documento de limites LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries); na prática, número real pode ser ligeiramente inferior
Número máx. de entidades pré-criadas por aplicação | Consulte [documento de limites LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries)
Número máx. de entidades (total) por aplicação | 100
Número máx. de ações por aplicação | 32
Número máx. de caixas de diálogo de formação por aplicação | 1000
Número máx. de utilizador ativa por caixa de diálogo de formação | 100
Número máx. de caixas de diálogo de registo por aplicação | Nenhum limite predefinido, mas as caixas de diálogo de registo só são retidos durante um período antes de ser rejeitada fixo.  Além disso, a IU de Learner conversação irá mostrar 100 caixas de diálogo de registo cada vez. 
Número máx. de aplicações por utilizador | Sem limite predefinido
Número máx. de ações de não espera sequenciais | 5 (*)

(*) Após 5 ações não espera sequenciais, todas as ações de espera não são mascaradas e conversação Learner irá escolher entre as ações de espera disponíveis.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Introdução à conversação Learner](./quickstart.md)
