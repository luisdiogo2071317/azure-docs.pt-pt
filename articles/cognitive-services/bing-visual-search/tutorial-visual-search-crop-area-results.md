---
title: 'Tutorial: área e resultados do recorte de imagem - Pesquisa Visual do Bing'
description: Como utilizar o SDK de Pesquisa Visual do Bing para obter os URLs de imagens semelhantes para recortar uma área da imagem carregada.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 66e17c00da898e575bb858dbe16a35d1c44a2780
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226915"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Tutorial: área e resultados do recorte de imagem do SDK de Pesquisa Visual do Bing
O SDK de Pesquisa Visual inclui uma opção para selecionar uma área de uma imagem e localizar imagens online que são semelhantes à área de recorte da imagem maior.  Este exemplo especifica uma área de recorte que mostra uma pessoa de uma imagem com várias pessoas.  O código envia a área de recorte e o URL da imagem maior e devolve resultados que incluem os URLs de Pesquisa do Bing e os URLs de imagens semelhantes encontradas online.

## <a name="prerequisites"></a>Pré-requisitos

Precisa do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A Edição de Comunidade gratuita irá funcionar.)

Tem de ter uma [conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com APIs de Pesquisa do Bing. A [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="application-dependencies"></a>Dependências da aplicação
Para configurar a aplicação de consola através do SDK de Pesquisa na Web do Bing, aceda à opção Gerir Pacotes NuGet a partir do Explorador de Soluções no Visual Studio. Adicione o pacote Microsoft.Azure.CognitiveServices.Search.VisualSearch.

A instalação do pacote do SDK de Pesquisa na Web do NuGet também instala as dependências, que incluem:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Imagem e área de recorte
A imagem seguinte mostra a equipa de liderança sénior da Microsoft.  Com o SDK de Pesquisa Visual, podemos carregar uma área de recorte da imagem e encontrar outras imagens e páginas Web que incluem a entidade na área selecionada da imagem maior.  Neste caso, a entidade é uma pessoa.

![Equipa de Liderança Sénior da Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Especificar a área de recorte como ImageInfo no VisualSearchRequest
Este exemplo utiliza uma área de recorte da imagem anterior que especifica as coordenadas no canto superior direito e no canto inferior direito por percentagem de toda a imagem.  O código seguinte cria um objeto `ImageInfo` a partir da área de recorte e carrega o objeto `ImageInfo` para um `VisualSearchRequest`.  O objeto `ImageInfo` também inclui o URL da imagem online.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Pesquisar imagens semelhantes à área de recorte
O `VisualSearchRequest` contém informações da área de recorte sobre a imagem e o respetivo URL.  O método `VisualSearchMethodAsync` obtém os resultados.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obter os dados de URL de ImageModuleAction
Os resultados da Pesquisa Visual são objetos `ImageTag`.  Cada etiqueta contém uma lista de objetos `ImageAction`.  Cada `ImageAction` contém um campo `Data`, que é uma lista de valores que dependem do tipo de ação:

Pode obter os vários tipos com o código seguinte:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
A aplicação concluída devolve:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches  WebSearchURL:
* ActionType: Entity -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1

Conforme mostrado na lista anterior, a `Entity` `ActionType` contém uma consulta de Pesquisa do Bing que devolve informações sobre uma pessoa, local ou objeto reconhecível.  Os tipos `TopicResults` e `ImageResults` contêm consultas de imagens relacionadas. Os URLs na lista ligam aos resultados da pesquisa do Bing.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>URLs de PagesIncluding ActionType de imagens encontradas pela Pesquisa Visual

Obter os URLs da imagem atual requer uma conversão que lê um `ActionType` como `ImageModuleAction`, que contém um elemento `Data` com uma lista de valores.  Cada valor é o URL de uma imagem.  O seguinte converte o tipo de ação `PagesIncluding` em `ImageModuleAction` e lê os valores.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="complete-code"></a>Código completo

O código seguinte executa os exemplos anteriores. Envia uma imagem binária no corpo do pedido post, juntamente com um objeto cropArea, e imprime os URLs de pesquisa do Bing para cada ActionType. Se o ActionType for PagesIncluding, o código obtém os itens ImageObject nos Dados ImageObject.  Os Dados contêm uma lista de valores, que são os URLs de imagens em páginas Web.  Copie e cole os URLs resultantes da Pesquisa Visual no browser, para que os resultados sejam apresentados. Copie e cole os itens de ContentUrl no browser, para que as imagens sejam apresentadas.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            VisualSearchImageBinaryWithCropArea(subscriptionKey);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
                ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);
                
                VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

                Console.WriteLine("\r\nVisual search request with image from URL and crop area combined in knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {

                        foreach(ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            { 
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }

                        }

                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```
## <a name="next-steps"></a>Passos seguintes
[Resposta da Pesquisa Visual](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)