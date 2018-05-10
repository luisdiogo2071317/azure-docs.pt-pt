---
title: Intelligence de Media Services do Azure | Microsoft Docs
description: Quando utilizar os Media Services do Azure, pode analisar o contnet de áudio e vídeo através de AudioAnalyzerPreset e VideoAnalyzerPreset.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2018
ms.author: juliako
ms.openlocfilehash: 804a418f6ee88974d6e74a2c18bc5d01b6adf838
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="media-intelligence"></a>Informações de multimédia

API de v3 de REST de serviços de suporte de dados do Azure permite-lhe analisar o conteúdo de áudio e vídeo. Para analisar o conteúdo, crie um **transformar** e submeter um **tarefa** que utiliza um destas predefinições: **AudioAnalyzerPreset** ou **VideoAnalyzerPreset** . 

## <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

**AudioAnalyzerPreset** permite-lhe extrair vários insights áudio a partir de um ficheiro de vídeo ou áudio. O resultado inclui um ficheiro JSON (com todas as informações do) e VTT um ficheiro para o transcript áudio. Esta predefinição aceita uma propriedade que especifica o idioma do ficheiro de entrada sob a forma de um [BCP47](https://tools.ietf.org/html/bcp47) cadeia. As informações de áudio incluem:

* Áudio transcription – um transcript das palavras ditas com carimbos. São suportados vários idiomas
* Orador indexação – um mapeamento dos speakers e as palavras ditas correspondentes
* Análise de dados de sentimento de reconhecimento de voz – o resultado da análise de dados de sentimento efetuada em transcription de áudio
* Palavras-chave – palavras-chave que são extraídas do transcription áudio.

## <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

**VideoAnalyzerPreset** permite-lhe extrair vários insights de áudio e vídeos a partir de um ficheiro de vídeo. O resultado inclui um ficheiro JSON (com todas as informações do), um ficheiro VTT para a vídeo transcript e uma coleção de miniaturas. Esta predefinição também aceita um [BCP47](https://tools.ietf.org/html/bcp47) cadeia (que representa o idioma do vídeo) como uma propriedade. As vídeos informações incluem todas as informações de áudio mencionadas acima e os seguintes itens adicionais:

* Controlo de rostos em – o tempo durante os quais faces estão presentes no vídeo. Cada enfrentam tem um id de letra e de uma coleção correspondente de miniaturas
* Texto visual – o texto que é detetado através de reconhecimento de caráter optical. O texto é o tempo de carimbo de data / e também utilizado para extrair as palavras-chave (além de áudio transcript)
* Keyframes – uma coleção de keyframes que são extraídos de vídeo
* Visual conteúda moderação interrupção – a parte de vídeos que tenham sido sinalizados para adultos ou racy natureza
* Anotação – um resultado de anotar os vídeos baseados no modelo de objeto previamente definido

##  <a name="insightsjson-elements"></a>elementos de insights.JSON

O resultado inclui um ficheiro JSON (insights.json) com as informações que foram encontradas no vídeo ou áudio. O json pode conter os seguintes elementos:

### <a name="transcript"></a>transcript

|Nome|Descrição|
|---|---|
|ID|O ID de linha.|
|Texto|O transcript próprio.|
|idioma|O idioma de transcript. Destina-se suportar transcript onde cada linha pode ter um idioma diferente.|
|instâncias|Uma lista de intervalos de tempo em que esta linha apareceu. Se a instância for um transcript, terá apenas 1 instância.|

Exemplo:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

### <a name="ocr"></a>OCR

|Nome|Descrição|
|---|---|
|ID|O id de linha OCR.|
|Texto|O texto de OCR.|
|confiança|Confiança de reconhecimento.|
|idioma|O idioma de OCR.|
|instâncias|Uma lista de intervalos de tempo em que esta OCR apareceu (o mesmo OCR pode aparecer várias vezes).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

### <a name="keywords"></a>Palavras-chave

|Nome|Descrição|
|---|---|
|ID|O id de palavra-chave.|
|Texto|O texto da palavra-chave.|
|confiança|Confiança de reconhecimento da palavra-chave.|
|idioma|O idioma de palavra-chave (quando convertidos).|
|instâncias|Uma lista de intervalos de tempo em que esta palavra-chave apareceu (uma palavra-chave pode aparecer várias vezes).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

### <a name="faces"></a>faces

|Nome|Descrição|
|---|---|
|ID|O id de letra.|
|nome|O nome de letra. Pode ser 'Desconhecido #0', uma celebrity identificado ou uma pessoa treinado do cliente.|
|confiança|Confiança de identificação de letra.|
|descrição|Em caso de um celebrity, a respetiva descrição ("Satya Nadella nasceu em..."). |
|thumbnalId|O id de miniatura desse tipo de letra.|
|knownPersonId|Em caso de uma pessoa conhecida, o id interno.|
|ReferenceId|Em caso de celebrity Bing, o id do Bing.|
|referenceType|Atualmente, apenas o Bing.|
|título|Em caso de um celebrity, o título dele (por exemplo "CEO da Microsoft").|
|imageUrl|Em caso de um celebrity, o url da imagem.|
|instâncias|Estes são instâncias de onde o tipo de letra apareceu no intervalo de tempo especificado. Cada instância tem também uma thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

### <a name="labels"></a>Etiquetas

|Nome|Descrição|
|---|---|
|ID|o id da etiqueta.|
|nome|O nome de etiqueta (por exemplo, 'Computador', 'TV').|
|idioma|Idioma de nome etiqueta (quando convertidos). BCP 47|
|instâncias|Uma lista de intervalos de tempo em que esta etiqueta apareceu (uma etiqueta pode aparecer várias vezes). Cada instância tem um campo de confiança. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

### <a name="shots"></a>capturas de

|Nome|Descrição|
|---|---|
|ID|O id de captura.|
|keyFrames|Uma lista de chaves fotogramas dentro de captura (cada um tem um Id e uma lista de intervalos de tempo de instâncias).|
|instâncias|Uma lista de intervalos de tempo desta captura (capturas tem apenas 1 instância).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

### <a name="audioeffects"></a>audioEffects

|Nome|Descrição|
|---|---|
|ID|O ID de áudio em vigor.|
|tipo|O tipo de áudio efeito (por exemplo, silêncio Clapping, voz,).|
|instâncias|Uma lista de intervalos de tempo em que este efeito áudio apareceu.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```


### <a name="sentiments"></a>sentiments

Sentiments são agregados pelo respetivo campo sentimentType (neutras/positivos/negativo). Por exemplo, 0-0.1 0.1 0,2.

|Nome|Descrição|
|---|---|
|ID|O ID de dados de sentimento.|
|averageScore |A média de todas as pontuações de todas as instâncias desse tipo de dados de sentimento - positivos/neutras/negativo|
|instâncias|Uma lista de intervalos de tempo em que esta sentimento apareceu.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial: Analisar vídeos com Media Services do Azure](analyze-videos-tutorial-with-api.md)
