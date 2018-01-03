---
title: "Suportado inspetores disponíveis com a preparação de dados do Azure Machine Learning | Microsoft Docs"
description: "Este documento fornece uma lista completa das inspetores disponíveis para a preparação de dados do Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: ff5fcbc6df8cb07e0b98b877f20d981d6bef5117
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Suportado inspetores para a pré-visualização de preparação de dados do Azure Machine Learning
Este documento descreve o conjunto de inspetores que estão disponíveis nesta pré-visualização.

## <a name="the-halo-effect"></a>O efeito de halo 
Alguns inspetores suportam o efeito de halo. Este efeito utiliza dois cores diferentes para mostrar imediatamente a alteração visualmente a partir de uma transformação. O cinzento representa o valor antes da transformação mais recente e a azul mostra o valor atual. Este efeito pode ser ativado e desativado nas opções.

## <a name="graphical-filtering"></a>Filtragem de gráfico 
Alguns dos inspetores suportam a filtragem de dados utilizando o inspector como um editor. Utilizar o inspector como um editor envolve a seleção de elementos gráficos e, em seguida, utilizar a barra de ferramentas na parte superior direito da janela inspector para filtrar a entrada ou saída valores selecionados. 

## <a name="column-statistics"></a>Estatísticas de coluna
Para colunas numéricas, este inspector proporciona uma variedade de diferentes estatísticas sobre a coluna. Estatísticas incluem os seguintes valores: 
- Mínimo
- Quartile inferior
- Mediana
- Quartile superior
- Máximo
- Média
- Desvio-padrão


### <a name="options"></a>Opções 
- Nenhum

## <a name="histogram"></a>Histograma 
Calcula e apresenta um histograma de uma coluna numérica. O número predefinido de registos é calculado utilizando a regra de blogue. No entanto, a regra pode ser substituída através de opções.

Este Inspector suporta o efeito de halo.


### <a name="options"></a>Opções
- Número mínimo de registos (aplica-se mesmo quando bucketing predefinido é verificado)
- Número predefinido de registos (regra de blogue) 
- Mostrar halo
- Sobreposição de desenho de densidade de kernel (Gaussian kernel) 


### <a name="actions"></a>Ações
Este inspector suporta filtragem através de registos, que podem incluir único ou selecionar vários registos. Aplica filtros conforme descritos anteriormente.

## <a name="value-counts"></a>Contagens de valor
Este inspector apresenta uma tabela de frequência de valores para a coluna atualmente selecionada. A apresentação predefinida é para os valores de seis superiores. Pode alterar o limite para quantas, no entanto. Também pode definir o ecrã para contagem na parte inferior, em vez de na parte superior. Este inspector suporta o efeito de halo.

### <a name="options"></a>Opções 
- Número de valores superiores
- Descendente
- Incluir valores de erros/nulo
- Mostrar halo


### <a name="actions"></a>Ações 
Este inspector suporta filtragem através de barras, que podem incluir as barras de valor única ou selecionar vários. Aplica filtros conforme descritos anteriormente.

## <a name="box-plot"></a>Desenho de caixa 
Gráfico caixa whisker de uma coluna numérica.

### <a name="options"></a>Opções 
- Agrupar por coluna

## <a name="scatter-plot"></a>Dispersão desenho
Um desenho de gráfico de dispersão para duas colunas numéricas. Os dados são amostragem baixo por motivos de desempenho. O tamanho da amostra pode ser substituído nas opções do.

### <a name="options"></a>Opções  
- Coluna de eixo x
- Coluna de eixo y
- Tamanho da amostra
- Agrupar por coluna


## <a name="time-series"></a>Série de tempo
Um gráfico de linha com deteção automática de tempo no eixo x.

### <a name="options"></a>Opções
- Coluna Data
- Coluna numérica
- Tamanho da amostra


### <a name="actions"></a>Ações
Este inspector suporta filtragem através de um método select de clique e arraste para selecionar um intervalo no gráfico. Depois de concluir a seleção, aplica filtros conforme descritos anteriormente.


## <a name="map"></a>Mapa 
Um mapa com pontos são desenhados, partindo do princípio que foram especificadas latitude e longitude. A latitude tem de selecionar primeiro.

### <a name="options"></a>Opções
- Coluna de latitude
- Coluna de longitude
- Clustering do
- Agrupar por coluna


### <a name="actions"></a>Ações
Este inspector suporta filtragem através de seleção de ponto no mapa. Prima a **Ctrl** chave e, em seguida, clique e arraste com o rato para formar um quadrado à volta de pontos. Em seguida, aplicar filtros conforme descritos anteriormente.

Rapidamente pode redimensionar o mapa para mostrar apenas os pontos de possíveis, premindo o **i** no lado esquerdo do mapa.


## <a name="pattern-frequency"></a>Frequência de padrão 

Este inspector mostra uma lista de padrões na coluna selecionada de cadeia. Os padrões são representados utilizando uma expressão regular como sintaxe. Posicionado o padrão mostra exemplos de valores representados por esse padrão. Juntamente com os padrões, também é apresentado os coverages aproximados em termos de percentagem.

![Imagem de inspector o padrão](media/data-prep-appendix4-supported-inspectors/PatternInspectorProductNumber.png)

### <a name="options"></a>Opções
- Número de valores superiores
- Descendente
- Mostrar halo

### <a name="actions"></a>Ações
Este inspector suporta filtragem com base em padrões apresentados. Prima a **Ctrl** da chave e, em seguida, selecione as barras de manchas na inspector padrão. Em seguida, aplicar filtros conforme descritos anteriormente. Como resultado acion utilizador, é adicionado um passo de filtragem avançadas. Pode ver e modificar o código de Python gerado ao invocar a opção de editar o passo de filtro avançadas.
