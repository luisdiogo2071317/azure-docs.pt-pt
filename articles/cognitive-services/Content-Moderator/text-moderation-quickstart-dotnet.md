---
title: Moderator conteúdo do Azure - texto moderada através do .NET | Microsoft Docs
description: Como texto moderado utilizando o SDK de Moderator conteúdo do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 238d086e87b0e52f0887af5c4db58e8f72796b49
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352693"
---
# <a name="moderate-text-using-net"></a>Texto moderado através do .NET

Este artigo fornece informações e exemplos de código para o ajudar a começar a utilizar o SDK Moderator conteúdo para o .NET para:
- Detetar potencial profanity em texto com filtragem baseada no prazo
- Utilizar modelos de machine learning baseada em que [classificar o texto](text-moderation-api.md#classification) em três categorias.
- Deteta informações de identificação pessoal (PII) como E.U.A. e números de telefone RU, endereços de correio eletrónico e E.U.A. endereços de correio postal.
- Normaliza o texto e autocorrect erros de digitação

Este artigo pressupõe que já estiver familiarizado com o Visual Studio e c#.

## <a name="sign-up-for-content-moderator-services"></a>Inscreva-se os serviços de Moderator conteúdo

Antes de poder utilizar os serviços de Moderator conteúdo através da API REST ou o SDK, precisa de uma chave de subscrição.
Consulte o [início rápido](quick-start.md) para saber como pode obter a chave.

## <a name="create-your-visual-studio-project"></a>Criar o projeto do Visual Studio

1. Adicione um novo **aplicação de consola (.NET Framework)** projeto para a sua solução.

   No código de exemplo, nome do projeto **TextModeration**.

1. Selecione este projeto como o projeto de arranque único para a solução.

1. Adicione uma referência para o **ModeratorHelper** projeto de assemblagem que criou no [início rápido do programa auxiliar de cliente Moderator conteúdo](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os pacotes de NuGet seguintes:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft

### <a name="update-the-programs-using-statements"></a>O programa de atualização do instruções de utilização

Modificar o programa de instruções de utilização.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;


### <a name="initialize-application-specific-settings"></a>Inicializar definições específicas da aplicação

Adicione os seguintes campos estáticos para o **programa** a classe Program. cs.

    /// <summary>
    /// The name of the file that contains the text to evaluate.
    /// </summary>
    /// <remarks>You will need to create an input file and update this path
    /// accordingly. Relative paths are ralative the execution directory.</remarks>
    private static string TextFile = "TextFile.txt";

    /// <summary>
    /// The name of the file to contain the output from the evaluation.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private static string OutputFile = "TextModerationOutput.txt";

Utilizámos o seguinte texto para gerar a saída para este guia de introdução:

> [!NOTE]
> O número de segurança social inválidos no texto de exemplo seguinte é intencional. O objetivo é transmitir o exemplo de entrada e saída de formato.

    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
    0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Adicionar código para carregar e avaliar o texto de entrada

Adicione o seguinte código para o **Main** método.

    // Load the input text.
    string text = File.ReadAllText(TextFile);
    text = text.Replace(System.Environment.NewLine, " ");

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        // Create a Content Moderator client and evaluate the text.
        using (var client = Clients.NewClient())
        {
            // Screen the input text: check for profanity, classify the text into three categories
                // do autocorrect text, and check for personally identifying 
                // information (PII)
                outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
                var screenResult =
                    client.TextModeration.ScreenText("eng", "text/plain", text, true, true, null, true);
                outputWriter.WriteLine(
                        JsonConvert.SerializeObject(screenResult, Formatting.Indented));
        }
        outputWriter.Flush();
        outputWriter.Close();
    }

> [!NOTE]
> A chave do serviço Moderator conteúdo tem um pedidos por segundo (RPS) para o limite de velocidade e, se for excedido o limite, o SDK emite uma exceção com um código de 429 erro.
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

## <a name="run-the-program-and-review-the-output"></a>Execute o programa e reveja o resultado

O exemplo de saída do programa, que escritos no ficheiro de registo, é:

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
            "   CountryCode": "UK",
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

## <a name="next-steps"></a>Passos Seguintes

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET e começar a utilizar a integração.
