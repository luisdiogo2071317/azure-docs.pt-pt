---
title: Configuração de padrão de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba mais sobre a configuração predefinida do Aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c0ad9f71665e503fe794c68200b90a8474750823
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173630"
---
# <a name="default-values-and-boundaries"></a>Valores predefinidos e limites

Este documento descreve a configuração predefinida de aprendiz de conversação e limites de serviço de chave.

## <a name="default-configuration"></a>Configuração predefinida

Parâmetro | Valor predefinido
--- | --- 
Tempo limite de sessão predefinido | 30 minutos

## <a name="boundaries"></a>Limites

Parâmetro | Limite
--- | --- 
API de criação, HTTP máx. de chamadas por mês | 5M
API de criação, chamadas de HTTP máximo por segundo | 25
Sessão de API, Máx. de HTTP de chamadas por mês | 500 K
Sessão de API, Máx. de HTTP de chamadas por segundo | 10
Número máx. de entidades (não programática) personalizadas por modelo | Ver [documento de limites de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries); na prática, número real pode ser um pouco menor
Número máx. de entidades criadas previamente por modelo | Consulte [documento de limites de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries)
Número máx. de entidades (no total) por modelo | 100
Número máximo de ações por modelo | 32
Número máx. de caixas de diálogo train por modelo | 1000
Número máx. de utilizador ativa por caixa de diálogo preparar | 100
Número máx. de caixas de diálogo de registo por modelo | Sem limite predefinida, mas caixas de diálogo de registo, apenas são mantidas durante um período fixo antes de a ser eliminadas.  Além disso, a interface do Usuário de aprendiz de conversação mostrará 100 caixas de diálogo de registo ao mesmo tempo. 
Número máx. de modelos de por utilizador | Sem limite predefinida
Número máximo de ações de não-espera sequenciais | 5 (*)

(*) Depois de 5 ações de não-espera sequenciais, todas as ações de não-espera são mascaradas e Aprendiz de conversação irá escolher entre as ações de espera disponíveis.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Introdução ao aprendiz de conversação](./quickstart.md)
