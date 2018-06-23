---
title: Moderada de texto, utilizando a API de moderação interrupção de texto no Moderator conteúdo do Azure | Microsoft Docs
description: Test-Drive moderação de interrupção de texto, utilizando a API de moderação interrupção de texto na consola do online.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: ed696c31a886626819414c45eb7995edaf161fff
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352723"
---
# <a name="moderate-text-from-the-api-console"></a>Moderada texto a partir da consola de API

Utilize o [texto moderação interrupção API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) no Azure conteúdo Moderator para analisar o conteúdo de texto. A operação analisa o conteúdo para profanity e compara o conteúdo contra blacklists personalizados e partilhados.


## <a name="get-your-api-key"></a>Obter a chave de API
Antes de pode test-drive API na consola do online, terá da chave de subscrição. Isto está localizado no **definições** separador o **Ocp-Apim-Subscription-Key** caixa. Para obter mais informações, consulte [descrição geral](overview.md).

## <a name="navigate-to-the-api-reference"></a>Navegue para a referência de API
Vá para o [referência da API de moderação interrupção de texto](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  O **texto - ecrã** é aberta a página.

## <a name="open-the-api-console"></a>Abra a consola de API
Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Texto - seleção de região de página do ecrã](images/test-drive-region.png)

  O **texto - ecrã** é aberta a consola de API.

## <a name="select-the-inputs"></a>Selecione as entradas

### <a name="parameters"></a>Parâmetros
Selecione os parâmetros de consulta que pretende utilizar no seu ecrã de texto. Neste exemplo, utilizar o valor predefinido para **idioma**. Pode também deixar em branco porque a operação irá detetar automaticamente o idioma provável como parte da sua execução.

> [!NOTE]
> Para o **idioma** parâmetro, atribuir `eng` ou deixe vazio para ver a máquina assistida por **classificação** resposta (funcionalidade de pré-visualização). **Esta funcionalidade só suporta o inglês**.
>
> Para **termos profanity** deteção, utilize o [código ISO 639 3](http://www-01.sil.org/iso639-3/codes.asp) dos idiomas suportados listados neste artigo, ou deixe em branco.

Para **autocorrect**, **PII**, e **classificar (pré-visualização)**, selecione **verdadeiro**. Deixe o **ListId** campo vazio.

  ![Texto - parâmetros de consulta da consola de ecrã](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>Tipo de conteúdo
Para **Content-Type**, selecione o tipo de conteúdo que pretende ecrã. Neste exemplo, utilizar a predefinição **texto/plain** tipo de conteúdo. No **Ocp-Apim-Subscription-Key** box, introduza a chave de subscrição.

### <a name="sample-text-to-scan"></a>Texto de exemplo para analisar
No **corpo do pedido** box, introduza algum texto. O exemplo seguinte mostra um erro de digitação intencional no texto.

> [!NOTE]
> O número de segurança social inválidos no texto de exemplo seguinte é intencional. O objetivo é transmitir o exemplo de entrada e saída de formato.

```
    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
    Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="text-classification-feature"></a>Funcionalidade de classificação de texto

No exemplo seguinte, verá de resposta de classificação do conteúdo Moderator texto assistida por máquina. Ajuda a detetar conteúdo potencialmente indesejado. O conteúdo sinalizado pode ser considerado como inapropriado, dependendo de contexto. Para além de conveying a probabilidade de cada categoria, este poderá recomendar uma revisão humana do conteúdo. A funcionalidade utiliza um modelo preparado para identificar possíveis idioma abusivo, derogatory ou discriminatory. Isto inclui slang, abreviadas palavras, palavras ofensivas e intencionalmente com erros ortográficos para revisão. 

#### <a name="explanation"></a>Explicação

- `Category1` representa a presença potencial de idioma que pode ser considerada sexually explícita ou para adultos em determinadas situações.
- `Category2` representa a presença potencial de idioma que pode ser considerada sexually suggestive ou madura em determinadas situações.
- `Category3` representa a presença potencial de idioma que pode ser considerada ofensiva em determinadas situações.
- `Score` está entre 0 e 1. Quanto maior for a classificação, quanto maior for o modelo é a previsão que a categoria pode ser aplicável. Esta pré-visualização depende de um modelo de análises, em vez de resultados manualmente codificados. Recomendamos que teste com o seus próprios conteúdo para determinar a forma como cada categoria alinha os seus requisitos.
- `ReviewRecommended` é true ou false, dependendo do modelo de pontuação interno limiares. Os clientes devem avaliar se pretende utilizar este valor ou opte por utilizar limiares personalizados com base nas políticas os respetivos conteúdas.

### <a name="analyze-the-response"></a>Analisar a resposta
A resposta seguinte mostra as informações de vários partir da API de. Contém profanity potencial, PII, classificação (pré-visualização) e a versão de auto-corrigido.

> [!NOTE]
> A funcionalidade de 'Classificação' assistida por máquina está em pré-visualização e suporta apenas inglês.

```
{
    "OriginalText": "Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
    "NormalizedText": "Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation": null,
    "PII": {
            "Email": [{
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 32
                }],
            "IPA": [{
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 72
                }],
            "Phone": [{
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 56
                }, {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                }, {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                }, {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                }, {
                "CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
            }],
         "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                "Index": 89
            }],
            "SSN": [{
                "Text": "999999999",
                "Index": 56
            }, {
                "Text": "999-99-9999",
                "Index": 266
            }]
        },
    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
        },
        "Category2": {
            "Score": 0.12747249007225037
        },
        "Category3": {
            "Score": 0.98799997568130493
        }
        },
    "Language": "eng",
    "Terms": [{
            "Index": 21,
            "OriginalIndex": 21,
            "ListId": 0,
         "Term": "crap"
        }],
    "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        },
     "TrackingId": "2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

Para obter uma explicação detalhada de todas as secções na resposta JSON, consulte o [descrição geral de moderação de interrupção API texto](text-moderation-api.md).

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [guia de introdução do texto moderação interrupção .NET](text-moderation-quickstart-dotnet.md) para integrar com a sua aplicação.
