---
title: 'Início rápido: Analisar texto conteúdo no C# -moderador de conteúdo'
titlesuffix: Azure Cognitive Services
description: Como analisar os conteúdos de texto quanto a diversos materiais censuráveis com o SDK Content Moderator para .NET
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: sajagtap
ms.openlocfilehash: cc8cbc19f416e6e80d2d0fdfb4862536da7f0ce2
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312247"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-c"></a>Início rápido: Analisar o conteúdo de texto para objetáveis material doC# 

Este artigo apresenta informações e exemplos de código para o ajudara começar a utilizar o [SDK Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/). Vai aprender a executar a filtragem baseada em termos e a classificação de conteúdos de texto com o objetivo de moderar materiais potencialmente censuráveis.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="prerequisites"></a>Pré-requisitos
- A chave de uma subscrição do Content Moderator. Siga as instruções em [Create a Cognitive Services account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) (Criar uma conta dos Serviços Cognitivos) para subscrever o Content Moderator e obter a sua chave.
- Qualquer edição do [Visual Studio 2015 ou 2017](https://www.visualstudio.com/downloads/)

> [!NOTE]
> Este guia utiliza uma subscrição do Content Moderator no escalão gratuito. Para obter informações sobre o que cada escalão de subscrição disponibiliza, veja a página [Preços e limites](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

1. No Visual Studio, crie um projeto **Aplicação de Consola (.NET Framework)** novo e dê ao mesmo o nome **TextModeration**. 
1. Se houver outros projetos na sua solução, selecione esta como o único projeto de arranque.
1. Obtenha os pacotes NuGet necessários. Clique com o botão direito do rato no seu projeto no Explorador de Soluções e selecione **Manage NuGet Packages** (Gerir Pacotes NuGet); em seguida, localize e instale os pacotes seguintes:
    - `Microsoft.Azure.CognitiveServices.ContentModerator`
    - `Microsoft.Rest.ClientRuntime`
    - `Newtonsoft.Json`

## <a name="add-text-moderation-code"></a>Adicionar o código da moderação de textos

Depois, vai copiar e colar o código neste guia no projeto, para implementar um cenário de moderação de conteúdos simples.

### <a name="include-namespaces"></a>Incluir espaços de nomes

Adicione as declarações `using` seguintes à parte superior do ficheiro *Program.cs*.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=1-8)]

### <a name="create-the-content-moderator-client"></a>Criar o cliente do Content Moderator

Adicione o código seguinte ao ficheiro *Program.cs* para criar um fornecedor de cliente do Content Moderator para a sua subscrição. Adicione o código junto à classe **Program**, no mesmo espaço de nomes. Tem de atualizar os campos **AzureRegion** e **CMSubscriptionKey** com os valores do identificador da sua região e da sua chave de subscrição.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=54-77)]

### <a name="set-up-input-and-output-targets"></a>Configurar destinos de entrada e de saída

Adicione os seguintes campos estáticos à classe **Programa** em _Program.cs_. Estes campos especificam os ficheiros dos conteúdos de texto de entrada e dos conteúdos JSON de saída.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=15-19)]

Tem de criar o ficheiro de entrada *TextFile.txt* e atualizar o respetivo caminho em conformidade (os caminhos relativos são relativos ao diretório de execução). Abra _TextFile.txt_ e adicione o texto a moderar. Este início rápido utiliza o texto de exemplo seguinte:

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="load-the-input-text"></a>Carregar o texto de entrada

Adicione o seguinte código ao método **Principal**. O método **ScreenText** é a operação essencial. Os parâmetros do método especificam que operações de moderação de conteúdos vão ser feitas. Neste exemplo, o método está configurado para:
- Detetar potencial linguagem obscena no texto.
- Normalizar o texto e corrigir automaticamente erros ortográficos.
- Detetar informação pessoal, como números de telefone e endereços de e-mail dos EUA e do Reino Unido e moradas postais dos EUA.
- Utilizar modelos baseados em machine learning para classificar o texto em três categorias.

Se quiser saber mais sobre o que estas operações fazem, siga a ligação na secção [Passos seguintes](#next-steps).

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=23-48)]

## <a name="run-the-program"></a>Execute o programa

O programa escreve os dados da cadeia JSON no ficheiro _TextModerationOutput.txt_. Os exemplos de textos utilizados neste início rápido originam a saída abaixo:

```json
Autocorrect typos, check for matching terms, PII, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, desenvolveu uma aplicação .NET simples que utiliza o serviço Content Moderator para devolver informações relevantes sobre um determinado exemplo de texto. Em seguida, saiba o que querem dizer os diferentes sinalizadores e as diferentes classificações, para determinar de que dados precisa e como é que a sua aplicação os deve processar.

> [!div class="nextstepaction"]
> [Text moderation guide](text-moderation-api.md) (Guia de moderação de textos)
