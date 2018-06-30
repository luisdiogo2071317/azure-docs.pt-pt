---
title: Adicionar a API de verificação ortográfica do Bing v7 para consultas de LUIS | Microsoft Docs
titleSuffix: Azure
description: Palavras com erros ortográficos corretas no utterances adicionando API V7 de verificação do ortográfica do Bing para consultas de ponto final de LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/27/2018
ms.author: v-geberr
ms.openlocfilehash: 96b23146e726b7fee86b7e449c81d7efc0073e8d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127674"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Palavras com erros ortográficos corretas com verificação de ortográfica do Bing

Pode integrar a sua aplicação LUIS com [API V7 de verificação do ortográfica do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corrigir com erros ortográficos palavras no utterances antes LUIS prevê a classificação e entidades do utterance. 

## <a name="create-first-key-for-bing-spell-check-v7"></a>Criar primeira chave para a V7 de verificação ortográfica do Bing
O [primeira chave de v7 a API de verificação ortográfica do Bing](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) é gratuito. 

![Criar chave livre](./media/luis-tutorial-bing-spellcheck/free-key.png)

< uma nome "criar subscrição-chave" ></a>
## <a name="create-endpoint-key"></a>Criar a chave de ponto final
Se a sua chave livre expirado, crie uma chave de ponto final.

1. Inicie sessão no [Portal do Azure](https://portal.azure.com). 

2. Selecione **crie um recurso** no canto superior esquerdo.

3. Na caixa de pesquisa, introduza `Bing Spell Check API V7`.

    ![Procure ortográfica do Bing verifique V7 de API](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Selecione o serviço. 

5. É apresentado um painel de informações para a direita contendo informações, incluindo o aviso Legal. Selecione **criar** para iniciar o processo de criação de subscrição. 

6. No painel seguinte, introduza as definições de serviço. Aguarde que o processo de criação de serviço concluir.

    ![Introduza as definições de serviço](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Selecione **todos os recursos** sob o **Favoritos** título no painel de navegação do lado esquerdo.

8. Selecione o novo serviço. O tipo é **serviços cognitivos** e a localização é **global**. 

9. No painel principal, selecione **chaves** para ver as chaves de novo.

    ![Copie as chaves](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. Copie a chave primeiro. Só precisa de uma das duas chaves. 

## <a name="using-the-key-in-luis-test-panel"></a>Utilizando a chave no painel de teste de LUIS
Existem dois locais LUIS para utilizar a chave. Está a ser o primeiro o [painel de teste](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). A chave não é guardada na LUIS mas em vez disso, é uma variável de sessão. Tem de definir a chave sempre que pretender que o painel de teste para aplicar o serviço de v7 a API de verificação ortográfica do Bing para o utterance. Consulte [instruções](interactive-test.md#view-bing-spell-check-corrections-in-test-panel) no painel de teste para definir a chave.

## <a name="adding-the-key-to-the-endpoint-url"></a>Adicione a chave para o URL de ponto final
A consulta de ponto final tem da chave transmitida os parâmetros de cadeia de consulta para cada consulta que pretende aplicar a correção de ortografia. Pode ter um chatbot que chama LUIS ou pode chamar o API do ponto final do LUIS diretamente. Independentemente da forma como se chama o ponto final, todas as chamadas tem de incluir as informações necessárias para as correções ortografia funcione corretamente.

O ponto final do URL tem vários valores que tem de ser transmitidos corretamente. A chave de v7 a API de verificação ortográfica do Bing é apenas outra destes. Tem de definir o **spellCheck** parâmetro para true e tem de definir o valor de **bing ortográfica-verifique-subscrição-chave** para o valor da chave:

https://{Region}.API.cognitive.microsoft.com/Luis/v2.0/Apps/{appID}?Subscription-Key={luisKey}&spellCheck=**verdadeiro**& bing ortográfica-verifique-subscrição-chave =**{bingKey}**& verboso = true & timezoneOffset = 0 & q = {utterance}

## <a name="send-misspelled-utterance-to-luis"></a>Enviar utterance com erros ortográficos para LUIS
1. Num browser, copie a cadeia anterior e substitua o `region`, `appId`, `luisKey`, e `bingKey` com os seus próprios valores. Certifique-se de que utiliza a região de ponto final, se for diferente da sua publicação [região](luis-reference-regions.md).

2. Adicione um utterance com erros ortográficos como "até que ponto é o mountainn?". Em inglês, `mountain`, com um `n`, é a ortografia correta. 

3. Selecione introduzir para enviar a consulta para LUIS.

4. LUIS responde com um resultado JSON para `How far is the mountain?`. Se a API de verificação ortográfica do Bing v7 Deteta um misspelling o `query` campo em resposta JSON da aplicação LUIS contém a consulta original e o `alteredQuery` campo contém a consulta corrigida enviada para LUIS.

```
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Ignorar prende ortografia
Se não pretender utilizar o serviço de v7 a API de verificação ortográfica do Bing, pode Etiquetar utterances tem prende ortografia, de modo a que LUIS pode saber ortografia adequada, bem como os erros de digitação. Esta opção requer maior esforço de etiquetas a utilizar um verificador de ortográfica.

## <a name="publishing-page"></a>Página de publicação
O [publicação](publishapp.md) página tem um **Verificador de ortográfica do Bing ativar** caixa de verificação. Este é uma conveniência para criar a chave e compreender a forma como o URL de ponto final é alterado. Ainda tem de utilizar os parâmetros de ponto final correto para ter ortografia corrigida para cada utterance. 

> [!div class="nextstepaction"]
> [Saiba mais sobre utterances de exemplo](luis-how-to-add-example-utterances.md)
