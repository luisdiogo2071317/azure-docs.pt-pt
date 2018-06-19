---
title: Dar formação e testar a aplicação de LUIS - Azure | Microsoft Docs
description: Utilize a compreensão de idiomas (LUIS) para trabalhar continuamente na sua aplicação para refiná-lo e melhorar a compreensão de idiomas.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 760434253a3ece14352154a22cc68142ec2b0531
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35760243"
---
# <a name="test-your-luis-app"></a>Testar a aplicação de LUIS
<a name="train-your-app"></a>
[Formação](luis-how-to-train.md) e [testar](luis-concept-test.md) uma aplicação é um processo iterativo. Após a sua aplicação LUIS de preparação, testá-lo com utterances de exemplo para ver se as entidades e pendentes são reconhecidas corretamente. Se não estiver, efetue as atualizações da aplicação de LUIS, formação e teste novamente. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="interactive-testing"></a>
## <a name="test-an-utterance"></a>Testar um utterance

1. Aceder à sua aplicação, selecionando o respetivo nome na **aplicações My** página. 

2. Para o acesso a **teste** painel gradualmente escalamento, selecione **teste** no painel superior da sua aplicação.

    ![Página de aplicação de teste e formação](./media/luis-how-to-interactive-test/test.png)

3. Introduza um utterance na caixa de texto e selecione Enter. Pode escrever como utterances de teste que pretende incluir o **testar**, mas apenas um utterance cada vez.

4. O utterance, a intenção superior e pontuação é adicionadas à lista de utterances na caixa de texto.

    ![Testar interativa identifica a intenção errada](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="clear-test-panel"></a>Painel de teste simples
Para limpar todos os utterances de teste introduzido e os respetivos resultados a partir da consola de teste, selecione **recomeçar** no canto superior esquerdo do **painel de teste**. 

## <a name="close-test-panel"></a>Painel de teste fechar
Para fechar o **teste** painel, selecione o **teste** botão novamente.

## <a name="inspect-score"></a>Inspecione o modelo de pontuação
Inspecione os detalhes do resultado do teste no **inspecionar** painel. 
 
1. Com o **teste** abrir gradualmente Escalamento painel, selecione **inspecionar** para um utterance que pretende comparar. 

    ![Inspecione botão](./media/luis-how-to-interactive-test/inspect.png)

2. O **inspeção** painel aparece. O painel inclui na parte superior da classificação de intenção, bem como quaisquer entidades identificadas. O painel mostra o resultado do utterance selecionado.

    ![Inspecione botão](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Parte superior correto intenção da classificação

1. Se a parte superior da classificação de intenção estiver incorreta, selecione o **editar** botão.

2.  Na lista pendente, selecione o objetivo correto para o utterance.

    ![Selecione a intenção correta](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Ver os resultados da sentiment

Se **análise de dados de sentimento** está configurado no **[publicar](publishapp.md#enable-sentiment-analysis)** página, os resultados do teste incluem o sentimento encontrado no utterance. 

![Imagem do painel de teste com a análise de dados de sentimento](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Corrija a intenção de correspondentes padrão
Se estiver a utilizar [padrões](luis-concept-patterns.md) e o utterance corresponder um padrão, mas foi prever a intenção incorreta, selecione o **editar** ligar o padrão, em seguida, selecione o objetivo correto.

## <a name="compare-with-published-version"></a>Comparar com a versão publicada
Pode testar a versão do Active Directory da sua aplicação com publicado [endpoint](luis-glossary.md#endpoint) versão. No **inspecionar** painel, selecione **comparar com publicados**. Qualquer teste contra o modelo publicado é deducted da sua saldo de quota da subscrição do Azure. 

![Compare com publicado](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Ver o ponto final JSON no painel de teste
Pode ver o ponto final de JSON para a comparação devolvido selecionando o **vista Mostrar JSON**.

![Resposta JSON publicada](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Definições adicionais no painel de teste

### <a name="luis-endpoint"></a>Ponto final de LUIS
Se tiver vários pontos finais de LUIS, utilize o **definições adicionais** ligação de teste do publicados painel para alterar o ponto final utilizado para fins de teste. Se não tiver a certeza de que ponto final a utilizar, selecione a predefinição **Starter_Key**. 

![Painel de teste com ligação definições adicionais realçada](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Ver as correções de verificação de ortográfica do Bing no painel de teste
Pode ver as correções de ortografia fornecidas pelo [Bing de verificação ortográfica v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) API na vista de JSON a **publicada** painel do painel de teste. 

Para utilizar esta funcionalidade, tem de ter publicado aplicações, e tem um ortográfica do Bing verifique [chave do serviço](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). A chave do serviço não está armazenada e tem de ser reposto para cada sessão de browser. 

Utilize o seguinte para utilizar a chave do serviço de v7 a verificação de ortográfica do Bing no painel de teste. 

1. No painel de teste, no **publicada** painel, selecione **definições adicionais**.

2. Na caixa de diálogo de pop-up, introduza o **Bing de verificação ortográfica** chave do serviço. 
    ![Introduza a chave do serviço de verificação de ortográfica do Bing](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)

3. Introduza uma consulta com um spelling incorreto, tais como `book flite to seattle` e selecione introduza. A ortografia incorreta de palavra `flite` é substituído na consulta enviada para LUIS e JSON resultante mostra ambas a consulta original, como `query`e a ortografia corrigida da consulta, como `alteredQuery`.

    ![Corrigido spelling JSON](./media/luis-how-to-interactive-test/interactive-with-spell-check-results.png)

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="batch-testing"></a>Teste do batch
Consulte o lote de teste [conceitos](luis-concept-batch-test.md) e saiba [como](luis-how-to-batch-test.md) testar um lote de utterances.

## <a name="next-steps"></a>Passos Seguintes

Se testar a indicar que a sua aplicação LUIS não reconhece as entidades e pendentes corretos, pode trabalhar para melhorar a exatidão da sua aplicação LUIS por etiquetagem utterances mais ou adicionar funcionalidades. 

* [Etiqueta utterances sugeridos com LUIS](Label-Suggested-Utterances.md) 
* [Utilize as funcionalidades para melhorar o desempenho da sua aplicação LUIS](luis-how-to-add-features.md) 
