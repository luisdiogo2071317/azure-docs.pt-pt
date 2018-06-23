---
title: Carregamento de imagem do Bing para Insights | Microsoft Docs
description: Aplicação de consola que utiliza a API de pesquisa do Bing imagem para carregar uma imagem e obter informações de imagem.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 12/07/2017
ms.author: v-gedod
ms.openlocfilehash: f0bf32a9951527a072fffe464f6b5f50d0f237a2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351458"
---
# <a name="tutorial-bing-image-upload-for-insights"></a>Tutorial: A imagem do Bing carregar informações

A API de pesquisa do Bing imagem fornece um `POST` opção de carregar uma imagem e procure informações relevantes para a imagem. Esta aplicação de consola c# carrega uma imagem utilizando o ponto final de pesquisa de imagem para obter detalhes sobre a imagem.
Os resultados são resumidamente, objetos JSON, tais como o seguinte:

![[Resultados JSON]](media/cognitive-services-bing-images-api/jsonResult.jpg)

Este tutorial explica como:

> [!div class="checklist"]
> * Utilize a pesquisa de imagem `/details` ponto final num `POST` pedido
> * Especifique os cabeçalhos do pedido
> * Utilize os parâmetros de URL para especificar os resultados
> * Carregar os dados de imagem e enviar o `POST` pedido
> * Imprimir os resultados JSON para a consola do

## <a name="app-components"></a>Componentes de aplicação

O tutorial da aplicação inclui três partes:

> [!div class="checklist"]
> * Configuração de ponto final para especificar o ponto final de pesquisa do Bing imagem e cabeçalhos necessários
> * Carregamento de ficheiros de imagem para `POST` pedido para o ponto final
> * Analisar os resultados JSON que são os detalhes devolveu o `POST` pedido

## <a name="scenario-overview"></a>Descrição geral do cenário
Existem [três pontos finais de pesquisa de imagem](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-endpoint). O `/details` ponto final pode utilizar um `POST` pedido com dados de imagem no corpo do pedido.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
O `modules` seguinte de parâmetro de URL `/details?` Especifica que tipo de detalhes de conter os resultados:
* `modules=All`
* `modules=RecognizedEntities` (as pessoas ou locais visíveis na imagem)

Especifique `modules=All` no `POST` pedido para obter texto JSON que inclui a lista seguinte:
* `bestRepresentativeQuery` -uma consulta de Bing devolve imagens semelhante para a imagem carregada
* `detectedObjects` tal como uma caixa delimitadora ou frequente oportunidades na imagem
* `image` metadados
* `imageInsightsToken` -token para um subsequentes `GET` pedido que obtém `RecognizedEntities` (pessoas ou locais visíveis na imagem) 
* `imageTags`
* `pagesIncluding` -Páginas Web que inclui a imagem
* `relatedSearches`
* `visuallySimilarImages`

Especifique `modules=RecognizedEntities` no `POST` pedido para obter apenas os `imageInsightsToken`, que é utilizada uma subsequentes no `GET` pedido. Esta identifica as pessoas ou coloca visível na imagem.

## <a name="webclient-and-headers-for-the-post-request"></a>Pedem WebClient e cabeçalhos para o POST
Criar um `WebClient` de objeto e definir os cabeçalhos. Todos os pedidos para a API de pesquisa do Bing requerem um `Ocp-Apim-Subscription-Key`. A `POST` pedido para carregar uma imagem também tem de especificar `ContentType: multipart/form-data`.

```
            WebClient client = new WebClient();
            client.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            client.Headers["ContentType"] = "multipart/form-data"; 
```

## <a name="upload-the-image-and-get-results"></a>Carregar a imagem e obter resultados

O `WebClient` classe inclui o `UpLoadFile` método que formata os dados para o `POST` pedido. -Formata o `RequestStream` e chamadas `HttpWebRequest`, evitando uma grande quantidade de complexidade.
Chamar `WebClient.UpLoadFile` com o `/details` ponto final e o ficheiro de imagem para carregar. A resposta é dados binários que facilmente são convertidos em JSON. 

Utilize o texto JSON para inicializar uma instância do `SearchResult` estrutura (consulte o [código fonte da aplicação](tutorial-image-post-source.md) para o contexto).
```        
         const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/details";

        // The image to upload. Replace with your file and path.
        const string imageFile = "ansel-adams-tetons-snake-river.jpg";
            
        byte[] resp = client.UploadFile(uriBase + "?modules=All", imageFile);
        var json = System.Text.Encoding.Default.GetString(resp);

        // Create result object for return
        var searchResult = new SearchResult()
        {
            jsonResult = json,
            relevantHeaders = new Dictionary<String, String>()
        };
```

## <a name="print-the-results"></a>Os resultados de impressão
O resto do código analisa o resultado JSON e imprime-a para a consola.

```
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
```
## <a name="get-request-using-the-imageinsightstoken"></a>OBTER a pedido utilizando o ImageInsightsToken
Para utilizar o `ImageInsightsToken` devolvido com resultados de uma `POST`, crie um `GET` pedido semelhante ao seguinte:
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details?InsightsToken="bcid_A2C4BB81AA2C9EF8E049C5933C546449*ccid_osS7gaos*mid_BF7CC4FC4A882A3C3D56E644685BFF7B8BACEAF2
```
Se existirem identificação pessoas ou locais na imagem, este pedido irá devolver informações sobre os mesmos.
O [inícios Rápidos](https://docs.microsoft.com/azure/cognitive-services/bing-image-search) contêm vários exemplos de código.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API de pesquisa do Bing imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

