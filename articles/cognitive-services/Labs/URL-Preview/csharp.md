---
title: Início rápido c# para o URL do projeto de pré-visualização - serviços cognitivos Microsoft | Microsoft Docs
description: Começar a utilizar o URL de projeto pré-visualização nos serviços cognitivos Microsoft no Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 17d44bd0c23d0a1e67da5a0e91248700d3166c1a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354026"
---
# <a name="url-preview-query-in-c"></a>Pré-visualização do URL consulta c#

O exemplo do c# seguinte cria uma pré-visualização de Url para o site SwiftKey Web: https://swiftkey.com/en.

## <a name="prerequisites"></a>Pré-requisitos

Terá de [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A edição da Comunidade gratuito irá funcionar.)

Obter uma chave de acesso para a versão de avaliação gratuita [laboratórios de serviços cognitivos](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Cenário de código

O seguinte código c# cria uma versão de pré-visualização do URL do site SwiftKey Web: https://swiftkey.com/en. 

Está implementado nos passos seguintes:
1. Declare as variáveis para especificar o ponto final e um URL de consulta para pré-visualização.  
2. Crie o pedido.
3. Adicionar o *Ocp-Apim-Subscription-Key* cabeçalho. 
4. Execute o pedido Web no modo assíncrono. 
5. Ler a resposta.
6. Imprima os cabeçalhos e os resultados JSON para a consola.

**Código de origem**

```
using System;
using System.IO;
using System.Net;
using System.Text;

namespace UrlPrevCshp
{
    class Program
    {
        static void Main(string[] args)
        {
            String uriBase = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search";
            String searchQuery = "https://swiftkey.com/en"; 
            var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(searchQuery);

            // Do the Web request and get response
            WebRequest request = HttpWebRequest.Create(uriQuery);
            request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR-SUBSCRIPTION-KEY";
            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            Console.WriteLine("\nHTTP Headers:\n");
            foreach (String header in response.Headers)
            {
                if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
                    Console.WriteLine(header + ": " + response.Headers[header]);
            }

            Console.WriteLine(JsonPrettyPrint(json));
            

            Console.ReadKey();
        }


        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }

    }
}

```
## <a name="running-the-application"></a>Executar a aplicação

Para executar a aplicação:

1. Crie uma nova solução de consola no Visual Studio.
2. Substitua `Program.cs` com o código fornecido.
3. Substitua o `YOUR-ACCESS-KEY` valor com uma chave de acesso válido para a sua subscrição.
4. Execute o programa.

## <a name="next-steps"></a>Passos Seguintes
- [Guia de introdução do Java](java-quickstart.md)
- [Início rápido de JavaScript](javascript.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)