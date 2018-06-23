---
title: Perguntas mais frequentes no Chat Personality - serviços cognitivos do Azure | Microsoft Docs
description: Perguntas mais frequentes na Personality Chat
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.topic: faq
ms.date: 05/07/2018
ms.author: nolachar
comment: As a bot developer, I want my bot to be able to handle small talk in a consistent tone so that my bot appears more complete and conversational.
ms.openlocfilehash: 5304b25672f9951e004876dfd19af75ea7d86502
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355627"
---
# <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

## <a name="why-doesnt-this-answer-every-question-i-ask-it-like-other-chat-bots"></a>Por que motivo não este responder a todas as perguntas que posso peça, tal como outros bots chat?

Projeto Personality chat melhorar um bot com comuns peça de pequena showcases personality, que cria uma experiência de utilizador mais completa. Não foi concebido para executar em conversações muito sobre tópicos que não são relevantes para a função principal do bot. Enquanto que pode responder a todas as conversações, ter restringida — na versão beta — para domínios de pequena peça comuns.

## <a name="how-can-i-customize-the-personality-to-suit-my-brand"></a>Como posso personalizar o personality de acordo com as minha marca?

Selecione a pessoa mais próxima as pessoas fictícias predefinidas disponíveis. Atualmente, pode demorar a biblioteca editorial e editar as respostas de acordo com a sua marca melhor. No futuro, pode carregar um conjunto de exemplo de utterances da sua personality escolhido e localizar a versão de ID de pessoa mais próxima. Também existem formas reparametrização dos e personalizar o modelo.

## <a name="is-this-service-powering-existing-intelligent-agents-such-as-zo"></a>Este serviço está a ligar a agentes inteligentes existentes como Zo?

Os serviços a ligar a Zo, Cortana e projeto Personality Chat partilham alguns técnicas semelhantes, mas são pilhas diferentes. -Tem incorporada learnings a partir das experiências com Zo e Cortana.

## <a name="can-this-service-lead-to-bad-customer-experiences"></a>Este serviço pode levar a experiências de cliente incorrecto?

Para fornecer uma experiência mais rica, Personality Chat pode gerar respostas além no conjunto de dados editorial e tentar interpretar todas as intervenção do utilizador. Por isso, é possível que uma resposta não está correta no contexto. Um número de controlos ter foi colocado no local para ajudar a evitar unfavorable respostas, tendo conhecimento dos agentes inteligentes como Zo. Por predefinição, o projeto Personality Chat está definido para responder apenas a pendentes utilizador reconhecido. Poderá pretender testar se projeto Personality Chat é adequado para as circunstâncias. Os seus comentários são boas-vindas se vir tudo o que precisa de mais preparação. Se utilizar este serviço com os seus clientes no futuro, recomendamos que considere anónimos registo para o ajudar a identificar problemas com as interações do utilizador em direto.