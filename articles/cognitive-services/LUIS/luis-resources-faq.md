---
title: Compreensão de idiomas (LUIS) no Azure perguntas mais frequentes | Microsoft Docs
description: Obtenha respostas às perguntas mais frequentes sobre a compreensão de idiomas (LUIS)
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: f192c2f71208f3caf8a01a18a7023763f8de63c3
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113141"
---
# <a name="language-understanding-faq"></a>FAQ de compreensão de idiomas

Este artigo contém as respostas a perguntas mais frequentes sobre a compreensão de idiomas (LUIS).

## <a name="luis-authoring"></a>LUIS criação

### <a name="what-are-the-luis-best-practices"></a>Quais são as melhores práticas de LUIS? 
Começar a utilizar o [criação ciclo](luis-concept-app-iteration.md), em seguida, lidos a [melhores práticas](luis-concept-best-practices.md). 

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>O que é a melhor forma de começar a construir a minha aplicação no LUIS?

A melhor forma de criar a sua aplicação é efetuada através de um [processo incremental](luis-concept-app-iteration.md). 

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>O que é uma boa prática para modelar pendentes da minha aplicação? Deve criar mais específicas ou mais genéricos pendentes?

Escolha pendentes que não são, por isso, gerais relativamente a ser sobrepostos, mas não, por isso, específicas que-dificulta LUIS distinguir entre pendentes semelhantes. Criar discriminative pendentes específicos é uma das melhores práticas para LUIS modelação.

### <a name="is-it-important-to-train-the-none-intent"></a>É importante preparar a intenção nenhum?

Sim, é boa preparar o seu **nenhum** intenção com mais utterances à medida que adiciona mais etiquetas para outros pendentes. Um rácio boa é 1 ou 2 etiquetas adicionadas ao **nenhum** para cada 10 etiquetas adicionadas ao objetivo. Este rácio boosts discriminative potência de LUIS.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Como pode corrigir os prende ortografia no utterances?

Consulte o [API V7 de verificação do ortográfica do Bing](luis-tutorial-bing-spellcheck.md) tutorial. LUIS impõe limites impostos API V7 de verificação do ortográfica do Bing. 

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>Como posso editar a minha aplicação LUIS programaticamente?
Para editar a sua aplicação de LUIS programaticamente, utilize o [criação API](https://aka.ms/luis-authoring-apis). Consulte [LUIS chamar a API de criação](./luis-quickstart-node-add-utterance.md) e [compilar uma aplicação de LUIS programaticamente com o Node.js](./luis-tutorial-node-import-utterances-csv.md) para obter exemplos de como chamar a API de criação. A API de criação requer que utilize um [criação chave](luis-concept-keys.md#authoring-key) em vez de uma chave de ponto final. Criação programático permite até 1.000.000 chamadas por mês e cinco transações por segundo. Para obter mais informações sobre as chaves a utilizar com LUIS, consulte [gerir chaves](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Qual é a funcionalidade de padrão que fornecia a expressão regular correspondente ao?
Anterior **funcionalidade do padrão** atualmente é preterido, substituídas por  **[padrões](luis-concept-patterns.md)**. 

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Como utilizar uma entidade para solicitar a saída de dados corretos? 
Consulte [entidades](luis-concept-entity-types.md) e [extração de dados](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Devem variações de um utterance de exemplo incluem a pontuação? 
Adicione as variações diferentes como utterances de exemplo para o objetivo ou adicione o padrão da utterance de exemplo com o [sintaxe para ignorar](luis-concept-patterns.md#pattern-syntax) de pontuação. 

## <a name="luis-endpoint"></a>Ponto final de LUIS

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Por que motivo LUIS adicionar os espaços à consulta em torno ou no meio palavras?
LUIS [tokenizes](luis-glossary.md#token) o utterance com base no [cultura](luis-supported-languages.md#tokenization). O valor original e o valor tokenized estão disponíveis para [extração de dados](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Como criar e atribuir um LUIS chave de ponto final?
[Criar a chave de ponto final](luis-how-to-azure-subscription.md#create-luis-endpoint-key) no Azure para a sua [serviço](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) nível. [Atribuir a chave](Manage-keys.md#assign-endpoint-key) no **[publicar](publishapp.md)** página. Não há nenhuma API correspondente para esta ação. Em seguida, tem de alterar os pedidos de HTTP para o ponto final para [utilizar a nova chave de ponto final](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>Como posso interpretar pontuações LUIS? 
O sistema deve utilizar a intenção de classificação mais elevada, independentemente do respetivo valor. Por exemplo, uma pontuação abaixo 0,5 (inferior a 50%) não significa necessariamente que LUIS tem confiança em como baixa. Fornecer os dados de formação mais pode ajudar a aumentar a pontuação da intenção mais provável.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Por que motivo não vejo os meus pedidos de ponto final no Dashboard da minha aplicação?
Os pedidos de ponto final total no Dashboard da aplicação são atualizados periodicamente, mas as métricas associadas à sua chave de ponto final de LUIS no portal do Azure são atualizadas mais frequentemente. 

Se não vir pedidos de ponto final atualizadas no Dashboard, inicie sessão no portal do Azure, localizar o recurso associado à sua chave de ponto final LUIS e abrir **métricas** para selecionar o **Total de chamadas** métrica. Se a chave de ponto final é utilizada para a mais do que uma aplicação de LUIS, a métrica no portal do Azure mostra o número de agregação de chamadas de todas as aplicações de LUIS utilização-lo.

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>A minha aplicação LUIS estava a funcionar ontem mas hoje posso estou obter 403 erros. Não alterar a aplicação. Como corrigi-lo? 
Seguir o [instruções](#how-do-i-create-and-assign-a-luis-endpoint-key) nas FAQ do seguintes para criar uma chave de ponto final LUIS e atribua-a para a aplicação. Em seguida, tem de alterar os pedidos de HTTP para o ponto final para [utilizar a nova chave de ponto final](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>Como proteger o meu endpoint LUIS? 
Consulte [proteger o ponto final](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>A funcionar dentro dos limites de LUIS

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>O que é o número máximo de pendentes e entidades que uma aplicação LUIS pode suportar?
Consulte o [limites](luis-boundaries.md) referência.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Pretende compilar uma aplicação LUIS com mais do que o número máximo de pendentes. O que devo fazer?

Consulte [melhores práticas para pendentes](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>Pretende criar uma aplicação no LUIS com mais do que o número máximo de entidades. O que devo fazer?

Consulte [melhores práticas para entidades](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Quais são os limites no número e tamanho da frase lista?
Para o comprimento máximo de um [lista frase](./luis-concept-feature.md), consulte o [limites](luis-boundaries.md) referência.

### <a name="what-are-the-limits-on-example-utterances"></a>Quais são os limites utterances de exemplo?
Consulte o [limites](luis-boundaries.md) referência.

## <a name="testing-and-training"></a>Formação e teste

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Consulte o artigo alguns erros no lote de painel de teste para alguns modelos da minha aplicação. Como posso resolver este problema?

Os erros indicarem que existe algum discrepância entre as etiquetas e serão as predições dos seus modelos. Para resolver o problema, efetue uma ou ambas as seguintes tarefas:
* Para ajudar a LUIS melhorar discrimination entre pendentes, adicione mais etiquetas.
* Para ajudar a LUIS Saiba mais rápida, adicione as funcionalidades de lista de expressão que introduzir vocabulário específicas do domínio.

Consulte o [Batch testar](luis-tutorial-batch-testing.md) tutorial.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Quando uma aplicação é exportada importado novamente para uma nova aplicação (com um novo ID de aplicação), as pontuações de predição LUIS são diferentes. Por que motivo isto acontecer? 

Consulte [predição as diferenças entre cópias da mesma aplicação](luis-concept-prediction-score.md#differences-with-predictions).

## <a name="app-publishing"></a>Publicação da aplicação

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>O que é o ID do inquilino na janela de "Adicionar uma chave para a aplicação"?
No Azure, um inquilino representa a organização que está associada um serviço ou cliente. Localizar o ID de inquilino no portal do Azure no **ID de diretório** caixa selecionando **do Azure Active Directory** > **gerir**  >  **Propriedades**.

![ID do inquilino no portal do Azure](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
### <a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>Por que razão são existe mais chaves de ponto final na minha aplicação publicar página que posso atribuídos à aplicação? 
Cada aplicação LUIS tem a chave de criação/arranque. As chaves de ponto final de LUIS criadas durante o período de tempo GA estão visíveis na sua página de publicar, independentemente se adicionou a aplicação. Isto foi feito para facilitar a migração de GA. As chaves de ponto final de LUIS novas não aparecem na página de publicar. 

## <a name="app-management"></a>Gestão de aplicações

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Como posso transferir a propriedade de uma aplicação LUIS?
Para transferir uma aplicação LUIS para outra subscrição do Azure, a aplicação de LUIS de exportação e importação-la utilizando uma nova conta. Atualize o ID da aplicação LUIS na aplicação cliente que chama-lo. A nova aplicação pode devolver as LUIS ligeiramente diferentes pontuações da aplicação original. 

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Como posso transferir um registo de utterances do utilizador?
Por predefinição, a aplicação de LUIS regista utterances dos utilizadores. Para transferir um registo de utterances que os utilizadores enviar para a sua aplicação LUIS, visite **aplicações My**e clique nas reticências (***...*** ) na listagem para a sua aplicação. Em seguida, clique em **exportar registos de ponto final**. O registo é formatado como um ficheiro de valores separados por vírgulas (CSV).

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Como desativar o registo de utterances?
Pode desativar o registo de utterances do utilizador através da definição `log=false` no URL do ponto final que a aplicação de cliente utiliza para consulta LUIS. No entanto, desativar o registo desativa a capacidade da sua aplicação LUIS sugerir utterances ou melhorar o desempenho com base no [learning Active Directory](luis-concept-review-endpoint-utterances.md#what-is-active-learning). Se definir `log=false` devido a problemas de privacidade de dados, não é possível transferir um registo desses utterances de utilizador do LUIS ou utilizar essas utterances para melhorar a sua aplicação.

O registo é o armazenamento apenas de utterances. 

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Por que motivo não a pretende todos os meus utterances de ponto final com sessão iniciadas?
Se estiver a utilizar o início de sessão para análise de predição, não capture utterances de teste no seu registo.

## <a name="data-management"></a>Gestão de dados

### <a name="can-i-delete-data-from-luis"></a>Pode eliminar dados de LUIS? 

* Pode sempre eliminar utterances de exemplo utilizados para LUIS de preparação. Se eliminar um utterance de exemplo da sua aplicação LUIS, é removido do serviço web LUIS e não está disponível para exportação.
* Pode eliminar utterances da lista de utterances de utilizador que sugere LUIS no **rever utterances de ponto final** página. Eliminar utterances desta lista evitará que está a ser sugeridos, mas não a eliminá-los de registos.
* Se eliminar uma conta, todas as aplicações são eliminadas, juntamente com os seus registos e utterances de exemplo. Os dados são mantidos nos servidores para 60 dias antes de ser eliminado permanentemente.

## <a name="language-and-translation-support"></a>Suporte de idioma e a tradução 

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Tenho uma aplicação no uma idioma e pretende criar uma aplicação paralela noutro idioma. O que é a forma mais fácil de fazê-lo?
1. Exporte a sua aplicação.
2. Converte os utterances identificados no ficheiro de JSON da aplicação exportado para o idioma de destino.
3. Poderá ter de alterar os nomes das entidades e pendentes ou deixe-os como estão.
4. Por fim, importe a aplicação com uma aplicação LUIS na linguagem de destino.

## <a name="app-notification"></a>Notificação da aplicação

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Por que motivo obtiveram uma mensagem de e-mail a indicar que quase sou fora da quota
A chave de criação/arranque só é permitida 1000 consulta o ponto final de um mês. Criar uma chave de ponto final LUIS (gratuita ou paga) e utilizar essa chave quando efetuar consultas de ponto final. Se estiver a efetuar consultas de ponto final de um bot ou outra aplicação de cliente, terá de alterar a chave de ponto final de LUIS não existe. 

## <a name="integrating-luis"></a>Integrar LUIS

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Em que a minha aplicação LUIS é criada durante o processo de subscrição de bot de aplicação web do Azure?
Se selecionar um modelo de LUIS e selecione o **selecione** botão no painel de modelo, o painel da esquerda é alterado para incluir o tipo de modelo e pede-lhe que região para criar o modelo de LUIS. O processo de bot de aplicação web não criar uma subscrição de LUIS apesar.

![Região do LUIS modelo web app bot](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>As regiões LUIS suportam priming de reconhecimento de voz Bot Framework?
[Priming de reconhecimento de voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) só é suportada para aplicações de LUIS na instância central (E.U.A.). 

## <a name="luis-service"></a>Serviço de LUIS 

### <a name="is-luis-available-on-premises-or-in-private-cloud"></a>É LUIS disponível no local ou na nuvem privada?
Não. 

## <a name="changes-to-the-docs"></a>Alterações para os Docs

### <a name="where-did-the-tutorials-go"></a>Em que aceda os tutoriais? 
Os artigos que foram anteriormente na secção do Tutorial estão agora na secção de procedimentos dos documentos. 

|Tutorial|
|--|
|Integrar LUIS com um bot com [c#](luis-csharp-tutorial-build-bot-framework-sample.md) e [Node.js](luis-nodejs-tutorial-build-bot-framework-sample.md)|
|Adicionar o Application Insights para um Bot com [c#](luis-tutorial-bot-csharp-appinsights.md) e [Node.js](luis-tutorial-function-appinsights.md)|
|Criar uma aplicação de LUIS através de programação utilizando [Node.js](luis-tutorial-node-import-utterances-csv.md)|
|Utilize [entidade composta](luis-tutorial-composite-entity.md) para extrair dados agrupados|
|Adicionar [lista entidade](luis-tutorial-list-entity.md) para deteção de aumento de entidade com o Node.js|
|Melhorar a exatidão da previsão com um [lista frase](luis-tutorial-interchangeable-phrase-list.md), [padrões](luis-tutorial-pattern.md), e [testes do batch](luis-tutorial-batch-testing.md)|
|[Corrija ortografia](luis-tutorial-batch-testing.md) com a API de verificação ortográfica do Bing v7

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Em conferência de 2018 compilação, posso conhecemos uma funcionalidade de compreensão de idiomas ou demonstração mas não me lembro do que foi chamado? 

As seguintes funcionalidades foram lançadas em conferência de 2018 compilação:

|Nome|Conteúdo|
|--|--|
|Melhoramentos|[Expressão regular](luis-concept-data-extraction.md##regular-expression-entity-data) entidade e [expressão de chave](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) entidade
|Padrões|Padrões [conceito](luis-concept-patterns.md), [tutorial](luis-tutorial-pattern.md), [procedimentos](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) entidade conceito incluindo [lista explícita](luis-concept-patterns.md#explicit-lists) exceções<br>[Funções](luis-concept-roles.md) conceito|
|Integrações|[Análise de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) integração de [sentiment analysis](publishapp.md#enable-sentiment-analysis)<br>[Voz](https://docs.microsoft.com/azure/cognitive-services/speech) integração de [priming de reconhecimento de voz](publishapp.md#enable-speech-priming) em conjunto com [SDK de reconhecimento de voz](https://aka.ms/SpeechSDK)|
|Ferramenta de emissão|Parte da [BotBuilder ferramentas](https://github.com/Microsoft/botbuilder-tools), linha de comandos de emissão [ferramenta](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) para combinar várias LUIS e Maker de QnA aplicações num único LUIS aplicação para um melhor reconhecimento intenção num Bot

Criação adicionais [API rotas](https://github.com/Microsoft/LUIS-Samples/blob/master/authoring-routes.md) estavam incluídas. 

Vídeos: 
* [Azure sexta-feira na compilação 2018: Serviços cognitivos-Language (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Compilação 2018 AI Mostrar - Novidades com o serviço de compreensão de idiomas](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Sessão do Build 2018 - Inteligência de bots, Capacidades de Voz e melhores práticas do NLU](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [2018 compilação - LUIS atualizações](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projetos: 
* [Bot contoso Cafe](https://github.com/botbuilderbuild2018/build2018demo) demonstração - o código de origem no Github

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre LUIS, consulte os seguintes recursos:
* [Perguntas de capacidade excedida da pilha etiquetadas com LUIS](https://stackoverflow.com/questions/tagged/luis)
* [Idioma MSDN compreender inteligente (LUIS) fórum de serviços](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
