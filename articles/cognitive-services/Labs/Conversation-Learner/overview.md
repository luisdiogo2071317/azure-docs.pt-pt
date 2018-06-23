---
title: O que é a conversação Learner? -Serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba mais sobre Learner de conversação e como funciona.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9cbf0e60382ef17d68aab47cf5f24ea9b8434f13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353990"
---
# <a name="what-is-conversation-learner"></a>O que é a conversação Learner?

Conversação Learner permite-lhe criar e ensinar conversational interfaces que saiba de interações de exemplo. 

Ao contrário das abordagens tradicionais, conversação Learner considera o contexto de ponto a ponto de um diálogo para melhorar as respostas e fornecer mais convincentes experiências de utilizador. A expansão de um conjunto amplo de orientados para tarefas casos de utilização, conversação Learner aplica-se a máquina aprender em segundo plano para tornar bots e agentes inteligentes menos prováveis frustrar utilizadores, implicar custos do serviço de cliente adicionais e intuitiva para interagir com.

Para começar a utilizar, o programador aciona prototypical caixas de diálogo que pretendem para imitar. Como são introduzidas mais caixas de diálogo, o modelo é constantemente atualizado e programadores podem ver quão bem é generalizar o modelo. Assim que o modelo está a funcionar corretamente, o bot pode ser implementado para os utilizadores finais. Conversação Learner regista conversações com utilizadores e o programador pode revê-los. Se prende é spotted, o programador pode efetuar uma correção no-a-lugar para cima e o modelo for retrained e disponíveis para utilização imediatamente.

Esta abordagem reduz o manual de programação de lógica de controlo de diálogo e permite que os proprietários de empresas ou especialistas de domínio contribuir para uma interface conversational sem anterior do machine learning dados de conhecimento. Se está implementada como parte de um bot, o dispositivo inteligente ou o agente inteligente, conversação Learner pode iterar rapidamente competências novo, comportamentos ou competências e melhorar rapidamente os seus qualidade. 

Conversação Learner proporciona aos programadores para aumentar a velocidade mercado e unidade dialogues com êxito através de vários canais conversational através do Microsoft Bot Framework ou de forma autónoma utilizando as seus próprios infraestrutura.

Resumo e destaques:

- Conversação Learner é uma forma de AI primeiro de bots orientados para tarefas de criação.

- Depende de uma ponto-a-ponto periódica a rede neuronal (LSTM) e aprende diretamente a partir de ativar vários exemplos de conversações. 

- Permite designers, os programadores, os utilizadores empresariais e técnicos do Centro de chamada criar e manter bots. 

- Fornece a capacidade para expressar as regras de negócio e sentido comuns no código.

- Durante a teaching sessões, o modelo de rede neuronal é utilizado para pontuar o seguinte conjunto de ações esperados na conversação. O Programador de bot pode, em seguida, selecione a ação correta e preparar a rede para fornecer a resposta adequada.
 
- Depois de concluída a formação, o programador pode utilizar as caixas de diálogo de registo das interações do utilizador para tornar as correções para respostas bot e reparametrização do modelo. 

- Pode chamar APIs específicas do domínio e de terceiros para concluir tarefas.

