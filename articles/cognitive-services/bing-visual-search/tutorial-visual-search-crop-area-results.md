---
title: Área de cortar Bing Visual pesquisa SDK resulta tutorial | Microsoft Docs
description: Como utilizar o SDK de pesquisa Visual do Bing para obter os URLs de imagens semelhante Recortar área da imagem carregada.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: fed9bb396d72f140235a2743c1447076606bb87c
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939463"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Tutorial: Área do SDK do Bing Visual Search imagem cortar e resultados
O SDK de pesquisa Visual inclui uma opção para selecionar uma área de uma imagem e encontre online imagens que são semelhantes a área de cortar da imagem maior.  Este exemplo Especifica área cortar que mostra uma pessoa a partir de uma imagem que contém várias pessoas.  O código envia a área de cortar e o URL da imagem a maior e devolve os resultados que incluem os URLs de pesquisa do Bing e URLs de imagens semelhantes encontrados online.

## <a name="prerequisites"></a>Pré-requisitos

Terá de [Visual Studio 2017](https://www.visualstudio.com/downloads/) para obter este código em execução no Windows. (A edição da Comunidade gratuito irá funcionar.)

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com APIs de pesquisa do Bing. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="application-dependencies"></a>Dependências da aplicação
Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing Web, navegue para a opção de gerir pacotes NuGet do Explorador de soluções no Visual Studio. Adicione o pacote de Microsoft.Azure.CognitiveServices.Search.VisualSearch.

Instalar o pacote NuGet Web pesquisa SDK também instala a dependências, incluindo:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft

## <a name="image-and-crop-area"></a>Área de imagem e cortar
A imagem seguinte mostra a equipa de liderança Diretores de Microsoft.  Utilizando o SDK de pesquisa Visual, podemos carregar uma área de cortar da imagem e localizar outros imagens e páginas Web que incluem a entidade selecionada área da imagem a maior.  Neste caso, a entidade é uma pessoa.

![Equipa de liderança sénior da Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Especifique a área de cortar como ImageInfo no VisualSearchRequest
Este exemplo utiliza uma área de cortar da imagem anterior que especifica superior esquerda e reduzir coordenadas direita por percentagem da imagem de toda.  O código seguinte cria um `ImageInfo` objeto da área de cortar e carrega o `ImageInfo` de objeto para uma `VisualSearchRequest`.  O `ImageInfo` objeto também inclui o URL da imagem online.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.25, left: (float)0.01, right: (float)0.2);
string imageURL = "http://windowsgeek.lk/wp-content/uploads/2016/04/cxc.png";
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Pesquisa para imagens semelhantes Recortar área
O `VisualSearchRequest` contém cortar área informações sobre a imagem e respetivo URL.  O `VisualSearchMethodAsync` método obtém os resultados.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obter os dados de URL de ImageModuleAction
Os resultados da pesquisa Visual estão `ImageTag` objetos.  Cada etiqueta contém uma lista de `ImageAction` objetos.  Cada `ImageAction` contém um `Data` campo uma lista de valores que dependem do tipo de ação:

Pode obter vários tipos com o seguinte código:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Devolve a aplicação concluída:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Entidade -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx, 5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5384.1

Conforme apresentado na lista anterior, o `Entity` `ActionType` contém uma consulta de pesquisa do Bing devolve informações sobre uma pessoa reconhecível, local ou coisa.  O `TopicResults` e `ImageResults` tipos contém consultas para imagens relacionadas. Os URLs na ligação de lista para resultados de pesquisa do Bing.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URLs de imagens que localizou através da pesquisa Visual

Obter os URLs de imagem real requer uma conversão que lê um `ActionType` como `ImageModuleAction`, que contém um `Data` elemento com uma lista de valores.  Cada valor é o URL de uma imagem.  As seguintes casts o `PagesIncluding` tipo de ação para `ImageModuleAction` e lê os valores.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="complete-code"></a>Código de conclusão

O seguinte código é executado exemplos anteriores. Envia uma imagem binário no corpo do pedido post, juntamente com um objeto de cropArea e impressões terminar o Bing Procurar URLs para cada ActionType. Se o ActionType PagesIncluding, o código obtém os itens de ImageObject ImageObject dados.  Os dados contêm uma lista de valores, que são os URLs das imagens em páginas Web.  Copie e cole resultante Visual pesquisa URLs do browser para mostrar os resultados. Copiar e colar itens de ContentUrl browser para mostrar as imagens.

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
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.25, left: (float)0.01, right: (float)0.2);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "http://windowsgeek.lk/wp-content/uploads/2016/04/cxc.png";
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
## <a name="next-steps"></a>Passos Seguintes
[Resposta de pesquisa Visual](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview#the-response)