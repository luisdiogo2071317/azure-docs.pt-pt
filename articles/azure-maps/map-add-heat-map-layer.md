---
title: Adicionar uma camada de mapa térmico para o Azure Maps | Documentos da Microsoft
description: Como adicionar uma camada de mapa térmico para o mapa de Javascript
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ec1343f85216171adac22f873f9be2e72bb4c282
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892466"
---
# <a name="add-a-heat-map-layer"></a>Adicionar uma camada de mapa térmico

Mapas de calor, também conhecido como ponto de mapas de densidade, são um tipo de visualização de dados utilizada para representar a densidade dos dados através de um intervalo de cores. Eles costumam ser usados para mostrar os dados "pontos de acesso" num mapa e são uma ótima maneira de processar grandes conjuntos de dados do ponto.  Por exemplo, processamento dezenas de milhares de pontos de dentro da vista do mapa, como símbolos cobrem a maior da área do mapa e resultaria em muitos símbolos que está a ser abrangidos por outras pessoas, tornando difícil obter mais informações sobre os dados. No entanto, visualizar esse mesmo conjunto de dados como um mapa térmico torna mais fácil ver onde os dados de ponto são o densest e a densidade relativa a outras áreas. Existem muitos cenários em que térmico mapas, são usados. Aqui estão alguns exemplos;

* Dados de temperatura normalmente são processados como mapa térmico, pois fornece aproximações para que a temperatura entre dois pontos de dados.
* Compor os dados de sensores de ruído como um mapa térmico não só mostra insanity de ruído em que é o sensor, mas também pode fornecer informações sobre a dissipação através de uma distância. O nível de ruído em qualquer um site pode não ser alto, no entanto, se a área de cobertura de ruído de vários sensores sobrepõe-se, é possível que esta área de sobreposição poderá experienciar níveis mais altos de ruído e, portanto, seria visível no mapa térmico.
* Visualizar um GPS o rastreamento, que inclui a velocidade como um mapa de altura de peso onde a intensidade de cada ponto de dados baseia-se na velocidade de é uma ótima maneira de ver rapidamente onde foi acelerando o veículo.

> [!TIP]
> Camadas de bolhas, por predefinição processará as coordenadas de todas as geometrias numa origem de dados. Para limitar a camada de forma que ele só processa geometria de ponto de conjunto de funcionalidades a `filter` propriedade da camada para `['==', '$type', 'Point']`

## <a name="add-a-heat-map-layer"></a>Adicionar uma camada de mapa térmico

Para processar dados de um origem dos pontos como um mapa térmico simple passar a sua origem de dados para uma instância da classe HeatMapLayer e adicioná-lo ao mapa como mostrado aqui.

<br/>

<iframe height='500' scrolling='no' title='Camada do mapa térmico simples' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>camada do mapa térmico simples</a> através do Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

Neste exemplo, cada ponto de calor tem um raio de 10 pixels em todos os níveis de zoom. Ao adicionar a camada do mapa térmico ao mapa, este exemplo insere-lo abaixo da camada de etiqueta. Esta ação cria uma melhor experiência de utilizador, como as etiquetas são claramente visíveis acima do mapa térmico. Os dados neste exemplo são obtidos a partir da [USGS Sismo perigos programa](https://earthquake.usgs.gov/) e consiste em pontos que representa sismos significativos que ocorreram nos últimos 30 dias.

## <a name="customizing-the-heat-map-layer"></a>Personalizando a camada do mapa térmico

O exemplo anterior personalizada mapa térmico definindo as opções de radius e a opacidade. A camada do mapa térmico fornece várias opções de personalização;

* `radius`: Define um raio de pixel no qual processar cada ponto de dados. Raio pode ser definido como um número fixo ou como uma expressão. Definir o raio como uma expressão que define o radius com base no nível de zoom pode resultar em mapas gráficos que parecem ter um raio que representa uma área geográfica consistente no mapa.
* `color`: Especifica a forma como o mapa gráfico é colorido. Uma paleta de cores de gradiente, muitas vezes, é utilizada para mapas gráficos, mas gradual também são úteis se quiser tornar o mapa de calor aspeto mais dados contour paletas de cores. Estes paletas de cores, definir as cores do mínimo ao valor máximo de densidade. Valores de cor para mapas gráficos são especificadas como uma expressão no `heatmap-density` valor. A cor no índice 0 numa expressão de gradação ou a cor predefinida de uma cor de passo define a cor da área em que não existe nenhum dado, ou a cor de fundo. Muitos preferem definir este valor como transparente ou um preto semitransparente. Seguem-se exemplos de expressões de cor;

| Expressão de cor de gradação | Expressão da cor gradual | 
|---------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolar',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\["densidade de mapa térmico"\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, "transparente",<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, 'roxa',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;"passo",<br/>&nbsp;&nbsp;&nbsp;&nbsp;\["densidade de mapa térmico"\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;"transparente",<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,25, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, "verde",<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,75, 'amarelo',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1,00, "vermelho"<br/>\] |   

* `opacity`: Especifica como opaco ou transparente do calor é da camada do mapa.
* `intensity`: Aplica-se um multiplicador para a importância de cada ponto de dados para aumentar a intensidade geral do mapa térmico. Isso ajuda a tornar as pequenas diferenças no peso de pontos de dados se tornam mais fácil de visualizar.
* `weight`: Por predefinição, todos os pontos de dados tem uma ponderação de 1, portanto, todos os pontos de dados são ponderados igualmente. A opção de peso age como um multiplicador e pode ser definida como um número ou uma expressão. Se um número é definido como número, digamos que a 2, seria o equivalente de colocar cada ponto de dados no mapa duas vezes, duplicação, portanto, a densidade. Definir a opção de peso para um número renderiza o mapa térmico de forma semelhante para utilizar a opção de intensidade. No entanto, se for utilizada uma expressão, o peso de cada ponto de dados pode ser baseado em diferentes e com base em algumas métrica nas propriedades do ponto. Dados de sismo tome como exemplo, cada ponto de dados representam um sismo e uma métrica importante que cada sismo tem é a magnitude. Sismos ocorrem o tempo todo, mas a maioria tem uma magnitude baixa e até mesmo não é sentidas. Utilizar o valor de magnitude numa expressão para atribuir a opção de peso permitirá que o sismos ser mais significativos aumento melhor ser representados no mapa térmico.
* Além das opções de camada de base; Mín/Máx de zoom, visíveis e filtrar, há também uma `source` opção se pretender atualizar a origem de dados e `source-layer` opção se a sua origem de dados é uma origem de mosaico do vetor.

Aqui está uma ferramenta para testar as opções de camada de mapa térmico diferentes.

<br/>

<iframe height='700' scrolling='no' title='Opções de camada do mapa térmico' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>opções de camada de mapa de calor</a> através do Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Ao ativar o clustering na origem de dados, os pontos que estão próximos uns dos outros são agrupados em conjunto como um ponto em cluster. A contagem de ponto de cada cluster pode ser utilizado como a expressão de peso para o mapa gráfico e reduzir significativamente o número de pontos que têm de ser de composição. A contagem de ponto de um cluster é armazenada numa propriedade de point_count da funcionalidade de ponto, conforme mostrado abaixo. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Se o raio de clustering é apenas alguns pixels lá irá ser visual pouca diferença o processamento. Um raio maior serão agrupar mais pontos em cada cluster e melhorar o desempenho do mapa térmico, mas ter o mais notável serão as diferenças.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as classes e métodos usados neste artigo:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes:

> [!div class="nextstepaction"]
> [Adicionar uma camada de símbolo](./map-add-pin.md)

