---
title: Guia de introdução do Python para os serviços do Azure cognitivos, análise de texto API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de análise de texto no Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: ashmaka
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: b4c02767320b71912050ad511811767e6b5decf4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352717"
---
# <a name="quickstart-for-text-analytics-api-with-python"></a>Início rápido para análise de texto API com o Python 
<a name="HOLTop"></a>

Estas instruções mostram como para [detetar idioma](#Detect), [analisar o sentimento](#SentimentAnalysis), e [extrair expressões chaves](#KeyPhraseExtraction) utilizando o [APIs de análise de texto](//go.microsoft.com/fwlink/?LinkID=759711)com o Python.

Pode executar este exemplo como um bloco de notas do Jupyter [MyBinder](https://mybinder.org) ao clicar no início do Gestor de enlaces de destaque: 

[![Gestor de enlaces](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

Consulte o [definições da API](//go.microsoft.com/fwlink/?LinkID=759346) documentação técnica para as APIs.

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de análise de texto**. Pode utilizar o **escalão gratuito para transações/mês 5000** para concluir estas instruções.

Também tem de ter o [chave de acesso e de ponto final](../How-tos/text-analytics-how-to-access-key.md) que foi gerado automaticamente durante a inscrição. 

Para continuar com estas instruções, substitua `subscription_key` com uma chave de subscrição válido que obteve anteriormente.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que a região no `text_analytics_base_url` corresponde à que utilizou quando configurar o serviço. Se estiver a utilizar uma chave de avaliação gratuita, não terá de alterar nada.


```python
text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/"
```

<a name="Detect"></a>

## <a name="detect-languages"></a>Detetar idiomas

A API de deteção de idioma Deteta o idioma de um texto documento, utilizando o [método detetar idioma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7). Ponto final do serviço de deteção de idioma API para a região está disponível através do seguinte URL:


```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/languages


O payload para a API é composta por uma lista de `documents`, cada de que por sua vez, contém um `id` e um `text` atributo. O `text` o texto a ser analisado os arquivos de atributo. 

Substitua o `documents` dicionário com quaisquer outro texto para a deteção de idioma. 


```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

As seguintes algumas linhas de código chamar para a deteção de API de idioma utilizando o `requests` biblioteca do Python para determinar o idioma nos documentos.


```python
import requests
from pprint import pprint
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
                                           'name': 'English',
                                           'score': 1.0}],
                    'id': '1'},
                   {'detectedLanguages': [{'iso6391Name': 'es',
                                           'name': 'Spanish',
                                           'score': 1.0}],
                    'id': '2'},
                   {'detectedLanguages': [{'iso6391Name': 'zh_chs',
                                           'name': 'Chinese_Simplified',
                                           'score': 1.0}],
                    'id': '3'}],
     'errors': []}


As seguintes linhas de código compor os dados JSON como uma tabela HTML.


```python
from IPython.display import HTML
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Analisar sentimento

O detexts de API de análise de dados de sentimento o sentimento de um conjunto de registos de texto, utilizando o [método sentimento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). O exemplo seguinte pontuações dois documentos, inglês num e outra na espanhol.

O ponto final de serviço para análise de dados de sentimento está disponível para a região através do seguinte URL:


```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment


Como com o exemplo de deteção de idioma, o serviço é fornecido com um dicionário com um `documents` chave que é composta por uma lista de documentos. Cada documento é constituída por uma cadeia de identificação de `id`, a `text` fosse analisado e o `language` do texto. Pode utilizar a deteção de idioma API da secção anterior para preencher este campo. 


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

O sentimento API pode agora ser utilizado para analisar os documentos para os respetivos sentiments.


```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

    {'documents': [{'id': '1', 'score': 0.7673527002334595},
                   {'id': '2', 'score': 0.18574094772338867},
                   {'id': '3', 'score': 0.5}],
     'errors': []}


A classificação de dados de sentimento para um documento está entre 0 $$ e $ de $1, com uma pontuação superior que indica um sentimento mais positivo.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Extrair expressões-chave

A API de extração de expressão de chave extrai expressões de chave a partir de um texto documento, utilizando o [método de chave expressões](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). Esta secção de instruções extrai expressões chaves para documentos em inglês e espanhol.

O ponto final de serviço para o serviço de extração de expressão de chave é acedido através do seguinte URL:


```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases


A coleção de documentos é o mesmo que estava a ser utilizada para análise de dados de sentimento.


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```


    {'documents': [
        {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
        {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
        {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
        {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
     'errors': []
    }


O objeto JSON novamente pode ser composto como uma tabela HTML utilizando as seguintes linhas de código:


```python
from IPython.display import HTML
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

## <a name="identify-linked-entities"></a>Identificar entidades ligadas

A API de associação de entidade identifica as entidades conhecidas de um texto documento, utilizando o [entidade de ligação método](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634). O exemplo seguinte identifica as entidades para documentos em inglês.

O ponto final de serviço para o serviço de ligação de entidade é acedido através do seguinte URL:


```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/entities


A coleção de documentos é abaixo:


```python
documents = {'documents' : [
  {'id': '1', 'text': 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.'},
  {'id': '2', 'text': 'The Seattle Seahawks won the Super Bowl in 2014.'}
]}
```

Agora, os documentos podem ser enviados para a API de análise de texto para receber a resposta.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```
    {
        "documents": [
            {
                "id": "1",
                "entities": [
                    {
                        "name": "Xbox One",
                        "matches": [
                            {
                                "text": "XBox One",
                                "offset": 23,
                                "length": 8
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "Xbox One",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                        "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                    },
                    {
                        "name": "Ultra-high-definition television",
                        "matches": [
                            {
                                "text": "4K",
                                "offset": 63,
                                "length": 2
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "Ultra-high-definition television",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                        "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                    }
                ]
            },
            {
                "id": "2",
                "entities": [
                    {
                        "name": "2013 Seattle Seahawks season",
                        "matches": [
                            {
                                "text": "Seattle Seahawks",
                                "offset": 4,
                                "length": 16
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "2013 Seattle Seahawks season",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                        "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                    }
                ]
            }
        ],
        "errors": []
    }

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Análise de texto com o Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)
