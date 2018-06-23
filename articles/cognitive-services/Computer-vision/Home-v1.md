---
title: Computador visão API cognitivos nos serviços da Microsoft | Microsoft Docs
description: Utilize algoritmos avançados a API de visão do computador para o ajudar a processar imagens e devolvem informações nos serviços cognitivos da Microsoft.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: 86e0441c600162e479c678d3cb1dbeaad423ddb5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354745"
---
# <a name="what-is-computer-vision-api-version-10"></a>O que é a versão 1.0 do computador visão API?

> [!IMPORTANT]
> Está agora disponível uma nova versão da API de visão do computador, consulte:
>- [Descrição geral](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Versão de API de visão do computador 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

A API de visão do computador baseado em nuvem fornece aos programadores com acesso ao algoritmos avançados para processamento de imagens e devolver informações. Carregamento de uma imagem ou especificar um URL de imagem, algoritmos de visão de computador do Microsoft podem analisar o conteúdo visual formas diferentes com base nas entradas e opções de utilizador. Com a API de visão do computador, os utilizadores podem analisar imagens para:
* [Imagens de etiqueta com base no conteúdo.](#Tagging)
* [Categorize imagens.](#Categorizing)
* [Identifique o tipo e a qualidade das imagens.](#Identifying)
* [Detetar faces humanos e as coordenadas de retorno. ](#Faces)
* [Reconhece o conteúdo de específicas do domínio.](#Domain-Specific)
* [Gere descrições do conteúdo.](#Descriptions)
* [Utilize optical caráter reconhecimento para identificar o texto impressos encontrado nas imagens.](#OCR)
* [Reconhece handwritten texto.](#RecognizeText)
* [Distingui esquemas de cores.](#Color)
* [Sinalizar conteúdo para adultos.](#Adult)
* [Fotografias cortar para ser utilizado como miniaturas.](#Thumbnails)

## <a name="requirements"></a>Requisitos
* Suportado métodos de introdução: binária na forma de um URL de application/octet-stream ou imagem de imagem não processada.
* Formatos de imagem suportados: JPEG, PNG, GIF, BMP.
* Tamanho do ficheiro de imagem: menos de 4 MB.
* Dimensão da imagem: maior do que 50 x 50 pixéis.

## <a name="tagging-images"></a>Marcação de imagens
API de visão do computador devolve etiquetas com base em mais de 2000 objetos reconhecível, beings de maior duração, scenery e ações. Quando as etiquetas são ambíguas ou dados de conhecimento não comum, a resposta da API fornece sugestões para esclarecer o significado da etiqueta no contexto de uma definição de conhecidos. As etiquetas não são organizadas como uma taxonomia e não existem hierarquias de herança existem. Uma coleção de etiquetas conteúdas constitui a base para uma imagem 'description', apresentado como idioma legível humano formatado no frases concluídas. Tenha em atenção que neste momento inglês é o único idioma suportado para descrição da imagem.

Depois de carregar uma imagem ou especificar um URL de imagem, algoritmos da API do computador visão saída etiquetas com base nos objetos, beings de maior duração e ações identificadas na imagem. Marcação não é limitada para o assunto principal, tal como uma pessoa em primeiro plano, mas também inclui furniture (indoor ou outdoor), de definição, as ferramentas, plants, animals, acessórios, miniaplicações etc.

### <a name="example"></a>Exemplo
![House_Yard](./Images/house_yard.jpg) '

```json
Returned Json
{
   'tags':[
      {
         "name":"grass",
         "confidence":0.999999761581421
      },
      {
         "name":"outdoor",
         "confidence":0.999970674514771
      },
      {
         "name":"sky",
         "confidence":999289751052856
      },
      {
         "name":"building",
         "confidence":0.996463239192963
      },
      {
         "name":"house",
         "confidence":0.992798030376434
      },
      {
         "name":"lawn",
         "confidence":0.822680294513702
      },
      {
         "name":"green",
         "confidence":0.641222536563873
      },
      {
         "name":"residential",
         "confidence":0.314032256603241
      },
   ],
}
```
## <a name="categorizing-images"></a>Categorizar imagens
Além de etiquetagem e as descrições, o computador visão API devolve as categorias com base em taxonomia definidas em versões anteriores. Estas categorias estão organizadas como uma taxonomia com hierarquias hereditary principal/subordinado. Todas as categorias estão em inglês. Podem ser utilizadas individualmente ou com os nossos novos modelos.

### <a name="the-86-category-concept"></a>O conceito de 86 categoria
Com base numa lista de 86 conceitos mostrado no diagrama seguinte, funcionalidades visual encontradas numa imagem podem ser classificadas entre abrangente e específico. Para Taxonomia completa no formato de texto, consulte [categoria taxonomia](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Analisar as categorias](./Images/analyze_categories.jpg)

Imagem                                                  | Resposta
------------------------------------------------------ | ----------------
![Woman Roof](./Images/woman_roof.jpg)                 | pessoas
![Fotografia família](./Images/family_photo.jpg)             | people_crowd
![Preguiçoso cute](./Images/cute_dog.jpg)                     | animal_dog
![Outdoor Mountain](./Images/mountain_vista.jpg)       | outdoor_mountain
![Visão analisar prato Bread](./Images/bread.jpg)       | food_bread

## <a name="identifying-image-types"></a>Identificar os tipos de imagem
Existem várias formas de categorizar imagens. API de visão do computador pode definir um sinalizador booleano para indicar se a uma imagem é negra e branco ou cor. -Também pode definir um sinalizador para indicar se uma imagem é um linha de desenho ou não. Também pode indicar se uma imagem é clip art ou não e indicar o respetiva qualidade como tal, numa escala de 0 a 3.

### <a name="clip-art-type"></a>Tipo de última geração clip
Deteta se uma imagem clip art ou não.  

Valor | Significado
----- | --------------
0     | Não-clip-art
1     | ambígua
2     | normal clip-última geração
3     | boa clip-última geração

Imagem|Resposta
----|----
![Visão analisar Cheese Clip Art](./Images/cheese_clipart.jpg)|3 boa clip-última geração
![Visão analisar próxima Yard](./Images/house_yard.jpg)|0 não clip-última geração

### <a name="line-drawing-type"></a>Tipo de desenho de linha
Deteta se uma imagem é um linha de desenho ou não.

Imagem|Resposta
----|----
![Visão analisar Lion desenho](./Images/lion_drawing.jpg)|Verdadeiro
![Visão analisar Flower](./Images/flower.jpg)|Falso

### <a name="faces"></a>Rostos
Deteta faces humanos dentro de uma imagem e gera as coordenadas de letra, o retângulo para a letra, sexo e idade. Estas funcionalidades visual são um subconjunto dos metadados gerados para enfrentam reside. Para mais extenso metadados gerados para faces (identificação facial, deteção apresentam e mais), utilize a API de rostos em.  

Imagem|Resposta
----|----
![Visão analisar Woman Roof enfrentam](./Images/woman_roof_face.png) | [{"idade": 23, "sexo": "Female", "faceRectangle": {"à esquerda": "superior", 1379: 320, "largura": "altura" 310,: 310}}]
![Visão analisar Mom Daughter enfrentam](./Images/mom_daughter_face.png) | [{"idade": 28, "sexo": "Female", "faceRectangle": {"à esquerda": 447, "superior": 195, "largura": "altura" 162,: 162}}, {"idade": 10, "sexo": "Male", "faceRectangle": {"à esquerda": 355, "superior": 87, "largura": "altura" 143,: 143}}]
![Visão analisar enfrentam Phot família](./Images/family_photo_face.png) | [{"idade": 11, "sexo": "Male", "faceRectangle": {"à esquerda": 113, "superior": 314, "largura": 222, "altura": 222}}, {"idade": 11, "sexo": "Female", "faceRectangle": {"à esquerda": "superior", 1200: 632, "largura": 215, "altura": 215}}, {"idade": 41, "sexo": "Male", " faceRectangle": {"à esquerda":"superior", 514: 223,"largura":"altura"205,: 205}}, {"idade": 37,"sexo":"Female","faceRectangle": {"à esquerda": 1008,"superior": 277,"largura": 201,"altura": 201}}]


## <a name="domain-specific-content"></a>Conteúdo de específicas do domínio

Além disso categorização a etiquetagem e de nível superior, API de visão do computador também suporta informações especializadas (ou específicas do domínio). Informações especializadas podem ser implementadas como método autónomo ou com a categorização de alto nível. Funciona como um meio para aperfeiçoar ainda mais a taxonomia de categoria 86 através da adição de modelos de específicas do domínio.

Atualmente, as únicas informações especializadas suportadas são reconhecimento celebrity e reconhecimento landmark. São específicas do domínio refinamentos para as pessoas e categorias de grupo de pessoas e landmarks em todo o mundo.

Existem duas opções para utilizar os modelos de específicas do domínio:

### <a name="option-one---scoped-analysis"></a>Análise de âmbito uma - opção
Analise apenas um modelo que escolheu invocando uma chamada de POST de HTTP. Para esta opção, se souber o modelo que pretende utilizar, especifique o nome do modelo e apenas receber informações relevantes para esse modelo. Por exemplo, pode utilizar esta opção para procurar apenas celebrity reconhecimento. A resposta contém uma lista de potenciais correspondente celebridades para, acompanhadas as pontuações de confiança.

### <a name="option-two---enhanced-analysis"></a>Opção dois - análise avançada
Analise fornecer detalhes adicionais relacionados com a categorias da taxonomia 86 categoria. Esta opção está disponível para utilização nas aplicações, onde os utilizadores querem aproveitar analysis imagem genérico além dos detalhes de um ou mais modelos de específicas do domínio. Quando este método é invocado, o classificador de categoria 86 taxonomia chama-se primeiro. Se qualquer uma das categorias de corresponder ao de modelos/correspondente ao conhecido, uma segunda passagem de invocações de classificador segue. Por exemplo, se ' Detalhes = all "ou"Detalhes"incluem 'celebridades para', o método chama o classificador de celebrity depois do classificador de categoria 86 é chamado. O resultado inclui etiquetas começados por 'people_'.

## <a name="generating-descriptions"></a>Gerar descrições 
Algoritmos da API do computador visão analisam o conteúdo de uma imagem. Esta análise compõe a base para um 'description' apresentado como idioma legível por humanos no frases concluídas. A descrição resume o que se encontra na imagem. Os algoritmos da API do computador visão geram vários descrições com base nos objetos identificados na imagem. As descrições são cada avaliada e gerou uma pontuação de confiança. Devolve uma lista, em seguida, ordenada de pontuação de confiança mais alta até ao mais baixo. É possível encontrar um exemplo de um bot que utiliza esta tecnologia para gerar legendas imagem [aqui](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Geração de descrição de exemplo
![Arra & TIR edifícios](./Images/bw_buildings.jpg) '
```json
 Returned Json

'description':{
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a large city',
         'confidence':0.607638706850331
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a photo of a large city',
         'confidence':0.577256764264197
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a city',
         'confidence':0.538493271791207
      }
   ]   
   'description':[
      "tags":{
         "outdoor",
         "city",
         "building",
         "photo",
         "large",
      }
   ]
}
```

## <a name="perceiving-color-schemes"></a>Perceiving esquemas de cores
O algoritmo de computador visão extrai cores de uma imagem. As cores são analisadas em três contextos diferentes: primeiro plano, segundo plano e tudo. Estes são agrupados em cores acentos dominante doze 12. As cores acentos são preta, azul, castanha, cinzento, verde, laranja, pink, roxa, vermelho, teal, em branco e amarelo. Consoante as cores numa imagem, podem ser devolvidas em branco e do negra simple ou cores acentos em códigos de cor hexadecimal.

Imagem                                                       | Em primeiro plano |Segundo plano| Cores
----------------------------------------------------------- | --------- | ------- | ------
![Outdoor Mountain](./Images/mountain_vista.jpg)            | Preto     | Preto   | Branco
![Visão analisar Flower](./Images/flower.jpg)               | Preto     | Branco   | Em branco, negra, verde
![Visão analisar formação estação](./Images/train_station.jpg) | Preto     | Preto   | Preto

### <a name="accent-color"></a>Cor a acentos
Cor extraído a partir de uma imagem concebida para representar a cor mais eye-popping aos utilizadores através de uma combinação de cores dominante e saturação.

Imagem                                                       | Resposta
----------------------------------------------------------- | ----
![Outdoor Mountain](./Images/mountain_vista.jpg)            | N. º BC6F0F
![Visão analisar Flower](./Images/flower.jpg)               | N. º CAA501
![Visão analisar formação estação](./Images/train_station.jpg) | N. º 484B83


### <a name="black--white"></a>Negra & em branco
Sinalizador booleano que indica se uma imagem é preta & branco ou não.

Imagem                                                      | Resposta
---------------------------------------------------------- | ----
![Visão analisar edifício](./Images/bw_buildings.jpg)      | Verdadeiro
![Visão analisar próxima Yard](./Images/house_yard.jpg)      | Falso

## <a name="flagging-adult-content"></a>Sinalizar conteúdo para adultos
Entre as várias categorias visual é o grupo para adultos e racy, que permite a deteção de materiais para adultos e restringe a visualização de imagens que contêm conteúdo sexual. O filtro para a deteção de conteúdos para adultos e racy pode ser definido numa escala móvel para acomodar preferência do utilizador.

## <a name="optical-character-recognition-ocr"></a>Reconhecimento de caráter Optical (OCR)
Tecnologia de OCR Deteta o conteúdo de texto numa imagem e extrai o texto identificado para um fluxo de caráter machine-readable. Pode utilizar o resultado para pesquisa e várias outras finalidades como registos médicas, segurança e a banca. Deteta automaticamente o idioma. OCR poupa tempo e fornece conveniência para os utilizadores, permitindo-lhes tirar fotografias de texto em vez de transcribing o texto.

OCR suporta 25 idiomas. Estes idiomas são: Árabe, chinês simplificado, chinês tradicional, checo, dinamarquês, neerlandês, inglês, finlandês, francês, alemão, grego, húngaro, italiano, japonês, coreano, norueguês, polaco, português, romeno, russo, Sérvio (carateres cirílicos e que sejam em Latim) Eslovaco, espanhol, sueco e turco.

Se for necessário, OCR corrige a rotação do texto reconhecido, em graus, à volta do eixo horizontal imagem. OCR fornece as coordenadas de moldura de cada palavra tal como ilustrado no abaixo ilustração.

![Descrição geral de OCR](./Images/vision-overview-ocr.png) requisitos para OCR:
- O tamanho da imagem de entrada tem de estar entre 40 x 40 e 3200 x 3200 pixéis.
- A imagem não pode ser superior a 10 megapixels.

Imagem de entrada pode rodar de qualquer múltiplo de 90 graus plus um ângulo pequeno de até ' 40 graus.

A precisão do reconhecimento de texto depende a qualidade da imagem. Uma leitura incorreta pode ser causada por seguintes situações:
- Blurry imagens.
- Texto handwritten ou cursive.
- Estilos de tipo de letra artístico.
- Tamanho do texto pequeno.
- Os fundos dos complexos, sombras ou glare através de texto ou perspetiva distortion.
- Grande dimensão ou em falta as letras em maiúsculas no beginnings das palavras
- Texto de índice, expoente ou strikethrough.

Limitações: No fotografias onde o texto é dominante, falsos positivos podem ter palavras parcialmente reconhecidas. Em alguns fotografias, especialmente fotografias sem qualquer texto muito precisão pode variar consoante o tipo de imagem.

## <a name="recognize-handwritten-text"></a>Reconhecer Handwritten texto
Esta tecnologia permite-lhe detetar e extrair texto handwritten de notas, letras, essays, whiteboards, formulários, etc. Funciona com diferentes superfícies e fundos, como documentos técnicos, notas de post-it e quadros.

O reconhecimento de texto manuscrito poupa tempo e esforço e pode torná-lo mais produtivo, ao permitir obter imagens de texto, em vez de ter de transcrever o texto. -Possibilita a digitize notas. Este digitization permite-lhe implementar pesquisa rápida e fácil. Também reduz a aglomeração de papel.

Requisitos de entrada:
- Formatos de imagem suportados: JPEG, PNG e BMP.
- Tamanho do ficheiro de imagem tem de ser inferior a 4 MB.
- Dimensões da imagem tem de ser, pelo menos, 40, 40, no máximo 3200 x 3200.

Nota: esta tecnologia está atualmente em pré-visualização e só está disponível para texto em inglês.

## <a name="generating-thumbnails"></a>Gerar miniaturas
Uma miniatura é uma representação pequena de uma imagem de tamanho completo. Variadas dispositivos, como telemóveis, tablets e PCs criar uma necessidade de esquemas de (UX) de experiência de utilizador diferente e tamanhos de miniaturas. Utilizar cropping inteligente, esta funcionalidade do computador visão API ajuda a resolver o problema.

Depois de carregar uma imagem, obtém gerou uma miniatura de alta qualidade e o algoritmo de API de visão do computador analisa os objetos dentro da imagem. -Crops, em seguida, a imagem para caber os requisitos da região de interesse (ROI). Obtém apresentado o resultado dentro de uma estrutura especial, como mostrado na abaixo ilustração. A miniatura gerada pode ser apresentada com um ration aspeto que é diferente da proporção da imagem original para acomodar as necessidades de um utilizador.

O algoritmo em miniatura funciona da seguinte forma:

1. Remove elementos distrair os leitores a imagem e reconhece o objeto principal, a região de interesse (ROI).
2. Crops a imagem com base na região identificada de interesse.
3. Altera a proporção para ajustar as dimensões em miniatura de destino.

![Miniaturas](./Images/thumbnail-demo.png)
