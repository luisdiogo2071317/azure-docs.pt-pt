---
title: API de imagem digitalizada para serviços cognitivos da Microsoft | Documentos da Microsoft
description: Utilize algoritmos avançados a API de imagem digitalizada para ajudar a processar imagens e retornar informações sobre serviços cognitivos da Microsoft.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: 84d931ad79bf32b39a4d771f6afd1c9a05ad2395
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714824"
---
# <a name="what-is-computer-vision-api-version-10"></a>O que é a versão de API do computador visão 1.0?

> [!IMPORTANT]
> Está agora disponível uma nova versão da API de imagem digitalizada, consulte:
>- [Descrição geral](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Versão de API de visão do computador 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

A API de Imagem Digitalizada baseada na cloud fornece aos programadores acesso a algoritmos avançados para processamento de imagens e devolução de informações. Ao carregar uma imagem ou especificar um URL de imagem, os algoritmos da Imagem Digitalizada da Microsoft podem analisar o conteúdo visual de formas diferentes com base nas entradas e opções do utilizador. Com a API de imagem digitalizada, os usuários podem analisar imagens para:
* [Imagens de etiqueta com base no conteúdo.](#Tagging)
* [Categorize imagens.](#Categorizing)
* [Identifica o tipo e a qualidade das imagens.](#Identifying)
* [Detetar rostos humanos e retornar seus coordenadas. ](#Faces)
* [Reconhece conteúdo específico do domínio.](#Domain-Specific)
* [Gere descrições do conteúdo.](#Descriptions)
* [Utilize o reconhecimento ótico de carateres para identificar o texto impresso encontrado nas imagens.](#OCR)
* [Reconhece texto manuscrito.](#RecognizeText)
* [Distingui os esquemas de cores.](#Color)
* [Sinalizar conteúdo para adultos.](#Adult)
* [Fotografias de recorte para ser utilizado como miniaturas.](#Thumbnails)

## <a name="requirements"></a>Requisitos
* Métodos de entrada suportados: imagens Raw binárias na forma de um application/octet-stream ou URL da imagem.
* Formatos de imagem suportados: JPEG, PNG, GIF, BMP.
* Tamanho do ficheiro de imagem: menos de 4 MB.
* Dimensão da imagem: superior a 50 x 50 pixéis.

## <a name="tagging-images"></a>Identificação de imagens
API de imagem digitalizada devolve etiquetas com base em mais de 2000 objetos reconhecíveis, seres vivos, paisagens e ações. Quando as etiquetas são ambíguas ou dados de conhecimento não comuns, a resposta de API fornece "sugestões" para clarificar o significado da etiqueta no contexto de uma configuração conhecida. As etiquetas não são organizadas como uma taxonomia e não existem hierarquias de herança existem. Uma coleção de etiquetas de conteúdos serve de alicerce para uma imagem 'description', apresentado como linguagem legível humana formatada em sentenças completas. Tenha em atenção que neste momento inglês é o único idioma suportado para a descrição da imagem.

Depois de carregar uma imagem ou especificar um URL de imagem, algoritmos de API de imagem digitalizada saída etiquetas com base em objetos, seres vivos e ações identificadas na imagem. Marcação não se limita ao assunto principal, por exemplo, uma pessoa em primeiro plano, mas também inclui o mobiliário (fechado ou equipamentos esportivos), de definição, ferramentas, plantas, animais, acessórios, gadgets etc.

### <a name="example"></a>Exemplo
![House_Yard](./Images/house_yard.png) '

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
Para além das etiquetas e descrições, a API de imagem digitalizada devolve as categorias com base em taxonomia definidas nas versões anteriores. Estas categorias são organizadas como uma taxonomia com hierarquias principal/subordinado de hereditary. Todas as categorias estão em inglês. Eles podem ser usados isoladamente ou com nossos novos modelos.

### <a name="the-86-category-concept"></a>O conceito de categoria 86
Com base numa lista de 86 conceitos ilustrado no diagrama seguinte, recursos do visual encontrados numa imagem podem ser categorizados desde o amplo até específico. Para a taxonomia no formato de texto completa, consulte [taxonomia da categoria](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Analisar as categorias](./Images/analyze_categories.png)

Imagem                                                  | Resposta
------------------------------------------------------ | ----------------
![Teto de mulher](./Images/woman_roof.png)                 | pessoas
![Fotos de família](./Images/family_photo.png)             | people_crowd
![Dog fofinhos](./Images/cute_dog.png)                     | animal_dog
![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png)       | outdoor_mountain
![Visão analisar pão de comida](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Identificar os tipos de imagem
Existem várias formas para categorizar imagens. API de imagem digitalizada pode definir um sinalizador booliano para indicar se uma imagem é preto e branco ou de cor. Ele também pode definir um sinalizador para indicar se uma imagem é um desenho de linha ou não. Também pode indicar se uma imagem é clip-art ou não e indicar sua qualidade como tal, numa escala de 0 a 3.

### <a name="clip-art-type"></a>Tipo de clip-arts
Deteta se uma imagem é clip-art ou não.  

Valor | Significado
----- | --------------
0     | Não-clip-arts
1     | Ambígua
2     | Normal – clip-arts
3     | Bom-clip-arts

Imagem|Resposta
----|----
![Visão analisar queijo Clip-Art](./Images/cheese_clipart.png)|3 vale-clip-arts
![Visão analisar Yard de casa](./Images/house_yard.png)|0 não-clip-arts

### <a name="line-drawing-type"></a>Tipo de desenho de linha
Deteta se uma imagem é um desenho de linha ou não.

Imagem|Resposta
----|----
![Visão analisar Lion desenho](./Images/lion_drawing.png)|Verdadeiro
![Visão analisar flor](./Images/flower.png)|Falso

### <a name="faces"></a>Rostos
Deteta rostos humanos em imagens e gera as coordenadas da face, o retângulo para o rosto, sexo e idade. Esses recursos visuais são um subconjunto de metadados gerados para rosto. Para mais extensivo metadados gerados para rostos (identificação facial, deteção de posição e muito mais), utilize a API Face.  

Imagem|Resposta
----|----
![Visão analisar mulher teto Face](./Images/woman_roof_face.png) | [{"idade": 23, "sexo": "Feminino", "faceRectangle": {"left": "superior", 1379: 320, "largura": "altura" 310,: 310}}]
![Visão analisar Mom filha Face](./Images/mom_daughter_face.png) | [{"idade": 28, "sexo": "Feminino", "faceRectangle": {"left": 447, "superior": "largura" 195,: 162, "altura": 162}}, {"idade": 10, "sexo": "Masculino", "faceRectangle": {"left": 355, "superior": 87, "largura": "altura" 143,: 143}}]
![Visão analisar Phot família Face](./Images/family_photo_face.png) | [{"idade": 11, "sexo": "Masculino", "faceRectangle": {"left": 113, "superior": "largura" 314,: 222, "altura": 222}}, {"idade": 11, "sexo": "Feminino", "faceRectangle": {"left": 1200, "superior": 632, "largura": 215, "altura": 215}}, {"idade": 41, "sexo": "Masculino", " faceRectangle": {"left":"superior", 514: 223,"largura":"altura"205,: 205}}, {"idade": 37,"sexo":"Feminino","faceRectangle": {"left": 1008,"superior": 277,"largura": 201,"altura": 201}}]


## <a name="domain-specific-content"></a>Conteúdo de domínio específico

Além disso categorização a etiquetagem e de nível superior, a API de imagem digitalizada também suporta informações especializadas (ou específicas de domínio). Informações especializadas podem ser implementadas como método autónomo ou com a categorização de alto nível. Ele funciona como um meio para refinar ainda mais a taxonomia da categoria de 86 por meio da adição de modelos de domínios específicos.

Atualmente, as únicas informações especializadas suportadas são de reconhecimento de celebridade e reconhecimento de marcos. Eles são específicas do domínio refinamentos para as pessoas e categorias de grupo de pessoas e os pontos de referência em todo o mundo.

Existem duas opções para utilizar os modelos de domínios específicos:

### <a name="option-one---scoped-analysis"></a>Opção One - análise de âmbito
Analise apenas um modelo escolhido, invocando uma chamada de POST de HTTP. Para esta opção, se souber qual modelo de que pretende utilizar, especifique o nome do modelo e apenas obterá informações relevantes para esse modelo. Por exemplo, pode utilizar esta opção apenas serve para reconhecimento de celebridade. A resposta contém uma lista de potencial de correspondência de celebridades, acompanhadas por suas pontuações de confiança.

### <a name="option-two---enhanced-analysis"></a>Opção dois - recursos de análise
Analise para fornecer detalhes adicionais relacionados com categorias de taxonomia 86 categoria. Esta opção está disponível para uso em aplicativos em que os usuários querem obter análises de imagem genérica além dos detalhes de um ou mais modelos de domínios específicos. Quando esse método é invocado, o classificador de taxonomia da categoria 86 é chamado pela primeira vez. Se nenhuma das categorias fazer a correspondência dos modelos de correspondência/conhecido, uma segunda passagem de invocações de classificador segue. Por exemplo, se "Detalhes = all" ou "Detalhes" incluem 'celebridades', o método chama o classificador de celebridade depois que o classificador de categoria 86 é chamado. O resultado inclui etiquetas que começam com 'people_'.

## <a name="generating-descriptions"></a>Geração de descrições 
Algoritmos de API de imagem digitalizada analisam o conteúdo numa imagem. Esta análise serve de alicerce para uma 'description' apresentada como uma linguagem legível em sentenças completas. A descrição resume o que for encontrado na imagem. Algoritmos de API de imagem digitalizada geram vários descrições com base nos objetos identificados na imagem. Cada uma das descrições é avaliada e é gerada uma pontuação de confiança. Em seguida, é devolvida uma lista ordenada da pontuação de confiança mais alta para a mais baixa. Pode encontrar um exemplo de um bot que usa essa tecnologia para gerar legendas de imagem [aqui](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Geração de descrição de exemplo
![B & edifícios W](./Images/bw_buildings.png) '
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

## <a name="perceiving-color-schemes"></a>Perceber esquemas de cores
O algoritmo de imagem digitalizada extrai as cores de uma imagem. As cores são analisadas em três contextos diferentes: primeiro plano, segundo plano e tudo. Estes são agrupados em cores de destaque dominantes de 12. Essas cores de destaque são pretas, azul, brown, cinzento, verde, laranja, rosa, Roxo, vermelho, verde-cinza, branco e amarelo. Consoante as cores numa imagem, podem ser devolvidas simple preto e branco ou cores de destaque em códigos de cor hexadecimal.

Imagem                                                       | em primeiro plano |Segundo plano| Cores
----------------------------------------------------------- | --------- | ------- | ------
![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png)            | Preto     | Preto   | Branco
![Visão analisar flor](./Images/flower.png)               | Preto     | Branco   | White, preto, verde
![Visão analisar Train estação](./Images/train_station.png) | Preto     | Preto   | Preto

### <a name="accent-color"></a>Cor de destaque
Extraídos a partir de uma imagem projetada para representar a cor mais atrativo para os usuários por meio de uma combinação de cores dominantes e saturação de cor.

Imagem                                                       | Resposta
----------------------------------------------------------- | ----
![Hora das regiões equipamentos esportivos](./Images/mountain_vista.png)            | N. º BC6F0F
![Visão analisar flor](./Images/flower.png)               | N. º CAA501
![Visão analisar Train estação](./Images/train_station.png) | N. º 484B83


### <a name="black--white"></a>Preto e branco
Sinalizador booleano que indica se uma imagem é preta e branco ou não.

Imagem                                                      | Resposta
---------------------------------------------------------- | ----
![Construção de analisar de imagem digitalizada](./Images/bw_buildings.png)      | Verdadeiro
![Visão analisar Yard de casa](./Images/house_yard.png)      | Falso

## <a name="flagging-adult-content"></a>Sinalizar conteúdo para adultos
Entre as várias categorias de visual é o grupo para adultos, que permite a deteção de materiais para adultos e restringe a exibição de imagens que contêm conteúdo sexual. O filtro para a deteção de conteúdos para adultos pode ser definido numa escala móvel para acomodar a preferência do usuário.

## <a name="optical-character-recognition-ocr"></a>Reconhecimento Ótico de carateres (OCR)
Tecnologia de OCR Deteta o conteúdo de texto numa imagem e extrai o texto identificado para um fluxo de carateres legível por máquina. Pode usar o resultado de pesquisa e inúmeras outras finalidades, como registros médicos, segurança e banca. Deteta automaticamente o idioma. O OCR poupa tempo e cómodo para os utilizadores ao permitir tirar fotografias do texto em vez de transcrever o texto.

OCR suporta 25 idiomas. Essas linguagens são: Árabe, chinês simplificado, chinês tradicional, checo, dinamarquês, Holandês, inglês, finlandês, francês, alemão, grego, húngaro, italiano, japonês, coreano, norueguês, polaco, português, romeno, russo, Sérvio (Cirílico e Latim) Eslovaco, espanhol, sueco e turco.

Se for necessário, OCR corrige a rotação do texto reconhecido, em graus, à volta do eixo horizontal de imagem. OCR fornece as coordenadas de quadro de cada palavra, como visto na abaixo ilustração.

![Descrição geral de OCR](./Images/vision-overview-ocr.png) requisitos para OCR:
- O tamanho da imagem de entrada tem de ser entre 40 x 40 e 3200 x 3200 pixels.
- A imagem não pode ser maior do que 10 megapixels.

Imagem de entrada pode ser rodada qualquer múltiplo de 90 graus além de um ângulo pequeno de até ' 40 graus.

A precisão de reconhecimento de texto depende a qualidade da imagem. Uma leitura incorreta pode ser causada por seguintes situações:
- Imagens indistinta.
- Texto manuscrito ou cursivo.
- Estilos de tipo de letra artístico.
- Tamanho do texto pequeno.
- Planos de fundo complexos, sombras ou glare pela distorção de texto ou uma perspetiva.
- Demasiado grandes ou em falta letras maiúsculas nos primórdios de palavras
- Dolní Index Je, superior ou rasurado texto.

Limitações: Fotografias em que o texto é dominante, falsos positivos podem ser provenientes palavras parcialmente reconhecidas. Em algumas fotos, especialmente as fotos sem qualquer texto, muita precisão pode variar consoante o tipo de imagem.

## <a name="recognize-handwritten-text"></a>Reconhecer texto manuscrito
Esta tecnologia permite-lhe detetar e extrair texto manuscrito de notas, cartas, ensaios, quadros, formulários, etc. Funciona com diferentes superfícies e fundos, como documentos técnicos, notas de post-it e quadros.

O reconhecimento de texto manuscrito poupa tempo e esforço e pode torná-lo mais produtivo, ao permitir obter imagens de texto, em vez de ter de transcrever o texto. Ele possibilita a digitalização de notas. Este digitalização permite que implemente a pesquisa rápida e fácil. Também reduz a aglomeração de papel.

Requisitos de entrada:
- Formatos de imagem suportados: JPEG, PNG e BMP.
- Tamanho do ficheiro de imagem tem de ser inferior a 4 MB.
- Dimensões da imagem tem de ser, pelo menos, 40 x 40, no máximo de 3200 x 3200.

Nota: esta tecnologia está atualmente em pré-visualização e só está disponível para texto em inglês.

## <a name="generating-thumbnails"></a>Geração de miniaturas
Uma miniatura é uma representação pequenas de uma imagem em tamanho normal. Variados dispositivos como telemóveis, tablets e PCs criam a necessidade de layouts de experiência (UX) de utilizador diferente e tamanhos de miniatura. Utilizar o corte inteligente, esse recurso de API de imagem digitalizada ajuda a resolver o problema.

Depois de carregar uma imagem, é gerada uma miniatura de alta qualidade e o algoritmo de API de imagem digitalizada analisa os objetos dentro da imagem. Ele, em seguida, recorta a imagem de acordo com os requisitos da região de interesse (ROI). É apresentado o resultado numa estrutura especial como visto na abaixo ilustração. A miniatura gerada pode ser apresentada com ration um aspecto diferente da proporção de aspecto da imagem original para acomodar as necessidades de um utilizador.

O algoritmo de miniatura funciona da seguinte forma:

1. Remove a distração de elementos da imagem e reconhece o objeto principal, a região de interesse (ROI).
2. Recorta a imagem com base na região de interesse identificado.
3. Altera a taxa de proporção de acordo com as dimensões de miniatura de destino.

![Miniaturas](./Images/thumbnail-demo.png)
