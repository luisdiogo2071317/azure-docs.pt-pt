---
title: Chamar a API de imagem digitalizada | Documentos da Microsoft
description: Saiba como chamar a API de imagem digitalizada usando REST nos serviços cognitivos.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/20/2017
ms.author: kefre
ms.openlocfilehash: 34705681e665b57a89c43f31ca695e0acb45ae3f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760250"
---
# <a name="how-to-call-the-computer-vision-api"></a>Como chamar a API de imagem digitalizada

Este guia demonstra como chamar a API de imagem digitalizada com REST. Os exemplos são escritos em c# usando a biblioteca de cliente de API de imagem digitalizada e como chamadas de HTTP POST/obter. Nos concentraremos em:

-   Como obter "Etiquetas", "Descrição" e "Categorias".
-   Como obter informações de "Específicas do domínio" (celebridades).

### <a name="Prerequisites">Pré-requisitos</a> 
URL da imagem ou o caminho para a imagem armazenada localmente.
  * Métodos de entrada suportados: imagens Raw binárias na forma de um application/octet-stream ou URL da imagem
  * Formatos de imagem suportados: JPEG, PNG, GIF, BMP
  * Tamanho do ficheiro de imagem: menos de 4MB
  * Dimensão da imagem: superior a 50 x 50 pixéis
  
Nos exemplos abaixo, são demonstrados os seguintes recursos:

1. Analisar uma imagem e a uma matriz de etiquetas e uma descrição devolvido.
2. Analisar uma imagem com um modelo de domínio específico (especificamente, o modelo de "celebridades") e obter o correspondente resultar em retune de JSON.

Funcionalidades são divididas em:

  * **Opção um:** análise com âmbito - analisar apenas um modelo específico
  * **Opção dois:** análise avançada - analisar para fornecer detalhes adicionais com [taxonomia de categorias de 86](../Category-Taxonomy.md)
  
### <a name="Step1">Passo 1: Autorizar a chamada de API</a> 
Todas as chamadas para a API de imagem digitalizada requerem uma chave de subscrição. Esta chave tem de passar por um parâmetro de cadeia de caracteres de consulta ou ser especificada no cabeçalho do pedido. 

Para obter uma chave de assinatura, consulte [como obter chaves de subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md
).

**1.** Transferir a chave de subscrição por meio de uma cadeia de consulta, veja a seguir como um exemplo de API de imagem digitalizada:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** Passando a chave de subscrição também pode ser especificado no cabeçalho do pedido HTTP:

```ocp-apim-subscription-key: <Your subscription key>```

**3.** Ao utilizar a biblioteca de cliente, a chave de subscrição é passada por meio do construtor de VisionServiceClient:

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">Passo 2: Carregar uma imagem para o serviço de API de imagem digitalizada e obter etiquetas, descrições e celebridades</a>
A forma básica para efetuar a chamada de API de imagem digitalizada é carregar uma imagem diretamente. Isso é feito ao enviar um pedido de "POST" com o tipo de conteúdo application/octet-stream em conjunto com os dados lidos a partir da imagem. Para "Etiquetas" e "Descrição", este método de carregamento será igual para todas as chamadas de API de imagem digitalizada. A única diferença será o utilizador Especifica os parâmetros de consulta. 

Eis como obter "Etiquetas" e "Descrição" para uma determinada imagem:

**Opção um:** obter a lista de "Etiquetas" e um "Descrição"
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```
```
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```
**Opção dois** obter lista de "Etiquetas" apenas, ou uma lista de "Descrição" apenas:

###### <a name="tags-only"></a>Só de etiquetas:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Só de descrição:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>Eis como obter análises de específicas de domínio (no nosso caso, para celebridades).

**Opção um:** análise com âmbito - analisar apenas um modelo específico
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
Para esta opção, todos os outros parâmetros de consulta {visualFeatures, detalhes} não são válidos. Se quiser ver modelos todos com suporte, utilize: 
```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```
**Opção dois:** análise avançada - analisar para fornecer detalhes adicionais com [taxonomia de categorias de 86](../Category-Taxonomy.md)

Para aplicativos em que pretende obter análises de imagem genérica além dos detalhes de um ou mais modelos de domínios específicos, podemos estender a API de v1 com o parâmetro de consulta de modelos.
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```
Quando esse método é invocado, vamos chamar o classificador de categoria 86 pela primeira vez. Se nenhuma das categorias fazer a correspondência de um modelo de correspondência/conhecido, ocorrerá uma segunda passagem de invocações de classificador. Por exemplo, se "Detalhes = all", ou "Detalhes" incluem 'celebridades', vamos chamar o modelo de celebridades depois do classificador de categoria 86 é chamado e o resultado inclui a pessoa de categoria. Isto irá aumentar a latência aos usuários interessados em celebridades, em comparação comparadas a opção de um.

Todos os parâmetros de consulta do v1 irão comportar-se o mesmo nesse caso.  Se visualFeatures = categorias não for especificada, ela será implicitamente ativada.

### <a name="Step3">Passo 3: Obter e compreender a saída JSON para analisar e visualFeatures = etiquetas, descrição</a>

Segue-se um exemplo:
```
  {
    “tags”: [
    {
    "name": "outdoor",
        "score": 0.976
    },
    {
    "name": "bird",
        "score": 0.95
    }
            ],
    “description”: 
    {
    "tags": [
    "outdoor",
    "bird"
            ],
    "captions": [
    {
    "text”: “partridge in a pear tree”,
            “confidence”: 0.96
    }
                ]
    }
  }
```
Campo   | Tipo  | Conteúdo
------|------|------|
Etiquetas    | objeto    | Objeto de nível superior para a matriz de etiquetas
as etiquetas []. Nome | cadeia    | Palavra-chave do classificador de etiquetas
as etiquetas []. Pontuação    | número    | Pontuação de confiança, entre 0 e 1.
descrição  | objeto   | Objeto de nível superior para obter uma descrição.
Description.tags [] |    cadeia  | Lista de etiquetas.  Se há confiança insuficiente na capacidade de produzir uma legenda, as etiquetas talvez as únicas informações disponíveis para o chamador.
Description.captions[].Text | cadeia    | Uma expressão que descreve a imagem.
Description.captions[].confidence   | número    | Confiança para a frase.

### <a name="Step4">Passo 4: Obter e compreender a saída JSON de modelos de domínios específicos</a>

**Opção um:** análise com âmbito - analisar apenas um modelo específico

O resultado será uma matriz de etiquetas, um exemplo será semelhante a este exemplo:
```
  { 
    "result": [ 
      { 
    "name": "golden retriever", 
    "score": 0.98
    },
    { 
    "name": "Labrador retriever", 
    "score": 0.78
    }
              ]
  }
```

**Opção dois:** análise avançada - analisar para fornecer detalhes adicionais com taxonomia de categorias de 86

Para modelos de domínios específicos com a opção dois (análise avançada), as categorias de devolvem o tipo é expandido. Segue um exemplo:
```
  {
    "requestId": "87e44580-925a-49c8-b661-d1c54d1b83b5",
    "metadata":     {
      "width": 640,
      "height": 430,
      "format": "Jpeg"
                    },
    "result": {
      "celebrities": 
      [
        {
        "name": "Richard Nixon",
        "faceRectangle": {
          "left": 107,
          "top": 98,
          "width": 165,
          "height": 165
                         },
        "confidence": 0.9999827
        }
      ]
  }
```

O campo de categorias é uma lista de um ou mais da [86 categorias](../Category-Taxonomy.md) na taxonomia original. Observe também que as categorias que termine num caráter de sublinhado corresponderá dessa categoria e seus filhos (por exemplo, people_, bem como people_group, para o modelo de celebridades).

Campo   | Tipo  | Conteúdo
------|------|------|
categorias | objeto | Objeto de nível superior
categorias [] Name    | cadeia   | Dê um nome de taxonomia da categoria 86
categorias [] .score  | número    | Pontuação de confiança, entre 0 e 1
categorias [] .detail  | objeto?      | Objeto de detalhe opcional

Tenha em atenção que se correspondem a várias categorias (por exemplo, o classificador de categoria 86 devolve uma pontuação para people_ e people_young quando modelos = celebridades), os detalhes estão ligados para a correspondência de nível mais geral (people_ esse exemplo.)

### <a name="Errors">Respostas de erros</a>
Estes são idênticos aos vision.analyze, com o erro adicional do erro de NotSupportedModel (HTTP 400), que pode ser devolvido em cenários de opção um e dois de opção. Opção dois (análise avançada), para se qualquer um dos modelos especificados nos detalhes não são reconhecidas, a API irá devolver um NotSupportedModel, mesmo se um ou mais deles são válidos.  Os usuários podem chamar listModels para descobrir quais modelos são suportados.

### <a name="Summary">Resumo</a>

Estas são as funcionalidades básicas da API de imagem digitalizada: como pode carregar imagens e obter os metadados importantes em troca.

Para utilizar a API REST, aceda a [referência de API de visão do computador](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
 
