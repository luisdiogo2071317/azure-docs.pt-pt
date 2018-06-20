---
title: Conceitos de empresa para uma aplicação de LUIS - Azure | Microsoft Docs
description: Compreenda os conceitos de design para aplicações de LUIS grandes.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/05/2018
ms.author: v-geberr
ms.openlocfilehash: 1f501981dd4b45f4d36188ef4c2aaa6cb11881a2
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263758"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>Estratégias de empresa para uma aplicação LUIS
Reveja estes estratégias de design para a sua aplicação da empresa.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Quando espera que os pedidos de LUIS acima da quota
Se a taxa de pedidos de aplicação LUIS exceder permitidos [taxa de quota](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/), propagar-se a carga mais LUIS aplicações com o [mesma definição de aplicação](#use-multiple-apps-with-same-app-definition) ou crie e [atribuir várias chaves](#assign-multiple-luis-keys-to-same-app) para o aplicação. 

### <a name="use-multiple-apps-with-same-app-definition"></a>Utilizar várias aplicações com a mesma definição de aplicação
Exportar a aplicação LUIS original, em seguida, importar a aplicação no aplicações separadas. Cada aplicação tem um ID próprio aplicação. Quando publica, em vez de utilizar a mesma chave em todas as aplicações, crie uma chave separada para cada aplicação. Balancear a carga em todas as aplicações para que não existe nenhuma aplicação único é overwhelmed. Adicionar [Application Insights](luis-tutorial-bot-csharp-appinsights.md) para monitorizar a utilização. 

Para obter a mesma intenção superior entre todas as aplicações, certifique-se que a intenção predição entre a primeira e segunda intenção é suficientemente grande para se LUIS não se encontra confundido, concedendo resultados diferentes entre as aplicações para pequenas variações nos utterances. 

Designe uma única aplicação especificada como mestre. Qualquer utterances que são sugeridos para revisão devem ser adicionados à aplicação principal, em seguida, novamente movidas para todas as outras aplicações. Este é o uma exportação completa da aplicação, ou carregar os utterances identificados do mestre de para os elementos subordinados. Carregar pode ser feito a partir de [LUIS] [ LUIS] Web site ou a API de criação de um [único utterance](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) ou para um [batch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09). 

Agendar um periódica [revisão de utterances de ponto final](label-suggested-utterances.md) de aprendizagem Active Directory, tais como a cada duas semanas, reparametrização dos e voltar a publicar. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Atribuir LUIS várias chaves a mesma aplicação
Se a sua aplicação LUIS recebe mais ponto final que permite que a quota da sua chave único, criar e atribuir mais chaves para a aplicação de LUIS. Criar um tráfego manager ou gerir as consultas de ponto final entre as chaves de subscrição balanceadores de carga. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Quando a sua aplicação monolithic devolve intenção errada
Se a aplicação se destinar a prever uma ampla variedade de utterances de utilizador, considere implementar o [modelo de emissão](#dispatch-tool-and-model). Dividi uma aplicação monolithic permite LUIS para deteção de foco entre pendentes com êxito em vez de introdução confundido entre pendentes em toda a aplicação principal e subordinado de aplicações. 

Agendar um periódica [revisão de utterances de ponto final](label-suggested-utterances.md) de aprendizagem Active Directory, tais como a cada duas semanas, reparametrização dos e voltar a publicar. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>Quando tem de ter mais do que 500 pendentes
Por exemplo, vamos supor que estiver a desenvolver um assistente do office que tenha mais 500 pendentes. Se 200 pendentes referem-se para o agendamento de reuniões, 200 são sobre lembretes, 200 são sobre como obter informações sobre os seus colegas e são 200 para enviar correio eletrónico, pendentes de grupo para que cada grupo seja numa única aplicação, em seguida, criar uma aplicação de nível superior que contém cada intenção. Utilize o [emitir ferramenta e arquitetura](#dispatch-tool-and-model) para criar a aplicação de nível superior. Em seguida, altere o bot para utilizar a chamada em cascata como mostrar no [tutorial de emissão][dispatcher-application-tutorial]. 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Quando tiver de combinar LUIS vários e de QnA aplicações maker
Se tiver aplicações de maker LUIS vários e de QnA que precisam para responder a um bot, utilize o [ferramenta de emissão](#dispatch-tool-and-model) para criar a aplicação de nível superior. Em seguida, altere o bot para utilizar a chamada em cascata como mostrar no [tutorial de emissão][dispatcher-application-tutorial]. 

## <a name="dispatch-tool-and-model"></a>Ferramenta de emissão e o modelo
Utilize o [emissão] [ dispatch-tool] ferramenta da linha de comandos, foi encontrada no [BotBuilder ferramentas](https://github.com/Microsoft/botbuilder-tools) combinar várias LUIS e/ou Maker de QnA aplicações numa aplicação LUIS principal. Esta abordagem permite-lhe ter um domínio principal, incluindo todos os assuntos e domínios de sujeito do subordinado diferentes aplicações separado. 

![Imagem conceptual da arquitetura de emissão](./media/luis-concept-enterprise/dispatch-architecture.png)

O domínio principal é apresentado na LUIS como um **V emissão** aplicação. 

![Lista de aplicações de captura de ecrã de LUIS com aplicação LUIS criada pela ferramenta de emissão](./media/luis-concept-enterprise/dispatch.png)

O chatbot recebe o utterance, em seguida, envia para o elemento principal aplicação LUIS para predição. A intenção prevista superior da aplicação principal determina qual subordinado LUIS aplicação denomina-se em seguida. O chatbot envia o utterance para a aplicação de subordinados para uma previsão mais específico.

Compreender a forma como esta hierarquia de chamadas é efetuada a partir v4 do construtor Bot [tutorial de aplicação de emissor][dispatcher-application-tutorial].  

### <a name="intent-limits-in-dispatch-model"></a>Limites de intenção no modelo de emissão
Uma aplicação de distribuição tem 500 origens de emissão, equivalentes para 500 pendentes, como o máximo. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [um lote de teste](luis-how-to-batch-test.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[dispatcher-application-tutorial]: https://aka.ms/bot-dispatch
[dispatch-tool]: https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch