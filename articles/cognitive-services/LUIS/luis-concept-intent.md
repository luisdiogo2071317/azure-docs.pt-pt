---
title: Pendentes de compreender LUIS aplicações do Azure | Microsoft Docs
description: Descreve quais pendentes são aplicações de serviço de inteligente compreensão do idioma (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 5c2feb0240b676d4e106cbda65aaaed7604a35c5
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265157"
---
# <a name="intents-in-luis"></a>Pendentes no LUIS

Objetivo representa uma tarefa ou ação do utilizador que pretende efetuar. É um objetivo ou objetivo expressado num utilizador [utterance](luis-concept-utterance.md).

Defina um conjunto de pendentes que corresponde a ações que os utilizadores que querem tirar na sua aplicação. Por exemplo, uma aplicação levar define várias pendentes:

Viajam pendentes de aplicação   |   Utterances de exemplo   | 
------|------|
 BookFlight     |   "Livro-me um voo para Rio semana seguinte" <br/> "Viaje até-me Rio no 24th" <br/> "Preciso de um pedido de plane Domingo seguinte para Rio Alemanha Janeiro"    |
 Saudação     |   "Olá" <br/>"Olá" <br/>"Boa manhã"  |
 CheckWeather | "O que é a meteorologia, como no Boston?" <br/> "Mostrar previsão para este fim de semana" |
 Nenhuma         | "Get-me uma receitas de cookie"<br>"Os Lakers win?" |

Todas as aplicações vêm com o objetivo predefinido, "[nenhum](#none-intent-is-fallback-for-app)" que é o objetivo de contingência. 

## <a name="prebuilt-domains-provide-intents"></a>Domínios prebuilt fornecem pendentes
Além de pendentes por si, pode utilizar prebuilt pendentes a partir de um dos domínios prebuilt. Para obter mais informações, consulte [utilizar domínios prebuilt nas aplicações LUIS](luis-how-to-use-prebuilt-domains.md) para saber mais sobre como personalizar pendentes a partir de um domínio prebuilt para utilização na sua aplicação.

## <a name="return-all-intents-scores"></a>Devolver pontuações de todos os pendentes
Atribuir um utterance para um objetivo único. Quando LUIS recebe um utterance no ponto final, devolve a intenção de principal de um para esse utterance. Se quiser pontuações para todos os pendentes para o utterance, pode fornecer `verbose=true` sinalizador a cadeia de consulta da API de [chamada de ponto final](https://aka.ms/v1-endpoint-api-docs). 

## <a name="intent-compared-to-entity"></a>Objetivo em comparação comparado a entidade
A intenção representa uma ação a chatbot deve tomar para o utilizador e baseado no utterance todo. A entidade representa palavras ou frases reconhecíveis contidas no interior do utterance. Um utterance pode ter apenas uma parte superior da classificação de intenção, mas pode ter várias entidades. 

<a name="how-do-intents-relate-to-entities"></a> Criar um objetivo quando o utilizador _intenção_ acionaria a uma ação na sua aplicação de cliente, como uma chamada para a função de checkweather(). Em seguida, crie uma entidade para representar parâmetros necessários para executar a ação. 

|Intenção de exemplo   | Entidade | Entidade no utterances de exemplo   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | {"type": "localização", "entidade": "seattle"}<br>{"type": "builtin.datetimeV2.date","entity": "amanhã", "Resolução": "2018-05-23"} | O que a meteorologia tal como no `Seattle` `tomorrow`? |
| CheckWeather | {"type": "date_range", "entidade": "este fim de semana"} | Mostrar a previsão de `this weekend` | 

## <a name="custom-intents"></a>Pendentes personalizados

Da mesma forma intentioned [utterances](luis-concept-utterance.md) corresponde a um objetivo único. Utterances na sua intenção podem utilizar qualquer [entidade](luis-concept-entity-types.md) na aplicação, uma vez que as entidades não sejam específicas de intenção. 

## <a name="prebuilt-domain-intents"></a>Pendentes prebuilt domínio

[Domínios prebuilt](luis-how-to-use-prebuilt-domains.md) ter pendentes com utterances.  

## <a name="none-intent-is-fallback-for-app"></a>Nenhum intenção é de contingência para a aplicação
O **nenhum** intenção é um objetivo de catch-all ou de contingência. É utilizado para lhe ensinar LUIS utterances que não são importantes no domínio de aplicação (área requerente). O **nenhum** intenção deve ter entre 10 e 20 por cento dos utterances totais da aplicação. Não deixe-o vazio. 

### <a name="none-intent-helps-conversation-direction"></a>Nenhum intenção ajuda a direção de conversação
Quando um utterance é prever como a intenção e devolvido para o chatbot com esse predição a bot pode fazer perguntas mais ou forneça um menu para direcionar o utilizador escolhas válidos no chatbot. 

### <a name="no-utterances-in-none-intent-skews-predictions"></a>Nenhum utterances em nenhum intenção skews predições
Se não adicionar qualquer utterances para o **nenhum** LUIS intenção, força uma utterance que está fora do domínio para um dos pendentes do domínio. Isto irá desfasamento as pontuações de predição por teaching LUIS a intenção errada para a utterance. 

### <a name="add-utterances-to-the-none-intent"></a>Adicionar utterances como a intenção None
O **nenhum** intenção é criada mas deixada em branco de propósito. Preencha-o com utterances que estão fora do seu domínio. Uma boa utterance para **nenhum** é algo de completamente fora da aplicação, bem como o setor a aplicação funciona. Por exemplo, uma aplicação levar não deve utilizar qualquer utterances para **nenhum** pode relacionadas com a viajar, tais como as reservas, de faturação, hospitality, prato, entretenimento em utilização e carga. 

O tipo de utterances restantes para a nenhum intenção? Começar a utilizar algo mais específico que o seu bot não deve responder a esses "que tipo de dinosaur tem teeth azul?" Esta é uma pergunta até que ponto fora de uma aplicação levar muito específica. 

### <a name="none-is-a-required-intent"></a>Nenhuma é um objetivo necessário
O **nenhum** intenção é um objetivo necessário e não pode ser eliminada ou mudada.

## <a name="negative-intentions"></a>Intenções negativos 
Se pretende determinar positivas e negativos intenções, tal como "Posso **pretende** um carro" e "Posso **não** pretende um carro", pode criar duas pendentes (um positivos e um negativos) e adicione utterances adequados para cada. Ou pode criar um objetivo único e marcar os dois diferentes positivos e negativos termos como uma entidade.  

## <a name="intent-balance"></a>Saldo intenção
Os pendentes de domínio de aplicação devem ter um equilíbrio de utterances entre cada tentativa. Não dispõe de um objetivo com 10 utterances e outra tentativa com 500 utterances. Não é balanceado. Se tiver nesta situação, reveja o objetivo com 500 utterances para ver se muitas dos pendentes podem reorganizar para um [padrão](luis-concept-patterns.md). 

O **nenhum** intenção não está incluída no equilíbrio. Que objetivo deve conter a 10% dos utterances totais na aplicação.

## <a name="intent-limits"></a>Limites de intenção
Reveja [limites](luis-boundaries.md#model-boundaries) para compreender quantos pendentes pode adicionar a um modelo. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Se precisar de mais do que o número máximo de pendentes 
Em primeiro lugar, considere se o sistema está a utilizar demasiados pendentes. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Podem pendentes vários ser combinados objetivo único com entidades 
Pendentes são demasiado semelhantes podem tornar mais difícil LUIS distinguir entre eles. Pendentes devem ser diversificados suficiente para capturar as tarefas principais que o utilizador é solicitando, mas não precisam de capturar todos os caminho demora do seu código. Por exemplo, poderá ser separados pendentes numa aplicação levar BookFlight e FlightCustomerService mas BookInternationalFlight e BookDomesticFlight são demasiado semelhantes. Se o sistema tem de distingui-los, utilize entidades ou outros lógica vez pendentes. 

### <a name="dispatcher-model"></a>Modelo de emissor
Saiba mais sobre combinar LUIS e de QnA maker de aplicações com o [modelo de emissão](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Ajuda do pedido para aplicações com um número significativo de pendentes
Se reduzir o número de pendentes ou dividir a sua pendentes em várias aplicações não funcionar, contacte o suporte. Se a sua subscrição do Azure inclui serviços de suporte, contacte [o suporte técnico do Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [entidades](luis-concept-entity-types.md), que são importantes palavras relevantes para pendentes
* Saiba como [adicionar e gerir pendentes](luis-how-to-add-intents.md) na sua aplicação LUIS.
* Reveja o objetivo [melhores práticas](luis-concept-best-practices.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website