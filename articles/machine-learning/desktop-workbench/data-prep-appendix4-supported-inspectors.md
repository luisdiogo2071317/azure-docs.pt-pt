---
title: Suportado inspetores disponíveis com a preparação de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece uma lista completa dos inspetores disponíveis para a preparação de dados do Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: ef5f6f3dc7ae0c555b2afe000b54c443313800f1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649349"
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Suportado inspetores para a pré-visualização de preparação de dados do Azure Machine Learning
Este documento descreve o conjunto dos inspetores que estão disponíveis nesta pré-visualização.

## <a name="the-halo-effect"></a>O efeito de halo 
Alguns inspetores suportam o efeito de halo. Esse efeito usa duas cores diferentes para mostrar imediatamente a alteração visualmente a partir de uma transformação. O cinzento representa o valor antes da transformação mais recente e a azul mostra o valor atual. Esse efeito, pode ser ativado e desativado nas opções.

## <a name="graphical-filtering"></a>Filtragem de gráfico 
Alguns dos inspetores suportam a filtragem de dados ao utilizar o inspetor de como um editor. Utilizar o inspetor de como um editor envolve selecionar elementos gráficos e, em seguida, utilizar a barra de ferramentas na parte canto superior direito da janela de inspetor para filtrar e reduzir os valores selecionados. 

## <a name="column-statistics"></a>Estatísticas de coluna
Para colunas numéricas, esse inspetor fornece uma variedade de diferentes estatísticas sobre a coluna. As estatísticas incluem as seguintes medidas: 
- Mínimo
- Quartile inferior
- Mediana
- Quartile superior
- Máximo
- Média
- Desvio padrão


### <a name="options"></a>Opções 
- Nenhuma

## <a name="histogram"></a>Histograma 
Calcula e apresenta um histograma de uma única coluna numérica. O número predefinido de registos é calculado utilizando a regra de Scott. No entanto, a regra pode ser substituída através das opções disponíveis.

Esse Inspetor suporta o efeito de halo.


### <a name="options"></a>Opções
- Número mínimo de registos (aplica-se até mesmo quando padrão bucketing é verificada)
- Número predefinido de registos (regra de Scott) 
- Mostrar halo
- Sobreposição de plotagem de densidade de kernel (Gaussian kernel) 
- Utilizar escala logarítmica


### <a name="actions"></a>Ações
Esse inspetor oferece suporte a filtragem por meio de registos, que podem incluir registos de seleção único ou múltipla. Aplica filtros, conforme descritos anteriormente.

## <a name="value-counts"></a>Contagens de valores
Esse inspetor apresenta uma tabela de frequência de valores para a coluna atualmente selecionada. A exibição padrão é para os valores de seis principais. Pode alterar o limite para qualquer número de, no entanto. Também pode definir o ecrã para contar a partir da parte inferior, em vez de parte superior. Esse inspetor suporta o efeito de halo.

### <a name="options"></a>Opções 
- Número de valores mais altos
- Descendente
- Incluir valores nulos/erro
- Mostrar halo
- Utilizar escala logarítmica


### <a name="actions"></a>Ações 
Esse inspetor oferece suporte a filtragem por meio de barras, o que podem incluir barras de seleção única ou múltipla. Aplica filtros, conforme descritos anteriormente.

## <a name="box-plot"></a>Gráfico de caixa 
Um desenho de bigodes da caixa de uma coluna numérica.

### <a name="options"></a>Opções 
- Agrupar por coluna

## <a name="scatter-plot"></a>Dispersão
Um gráfico de dispersão para duas colunas numéricas. Os dados são objeto para baixo de amostragem por motivos de desempenho. O tamanho da amostra pode ser substituído nas opções.

### <a name="options"></a>Opções  
- Coluna do eixo x
- Coluna do eixo y
- Tamanho da amostra
- Agrupar por coluna


## <a name="time-series"></a>Série de tempo
Um gráfico de linha com o reconhecimento de tempo no eixo x.

### <a name="options"></a>Opções
- Coluna de data
- Coluna numérica
- Tamanho da amostra


### <a name="actions"></a>Ações
Esse inspetor oferece suporte a filtragem por meio de um método de select clicar e arrastar para selecionar um intervalo no gráfico. Depois de concluir a seleção, aplica filtros, conforme descritos anteriormente.


## <a name="map"></a>Mapa 
Um mapa com pontos que são desenhados, supondo que foram especificadas latitude e longitude. Em primeiro lugar, tem de ser selecionado latitude.

### <a name="options"></a>Opções
- Coluna de latitude
- Coluna de longitude
- Clustering em
- Agrupar por coluna


### <a name="actions"></a>Ações
Esse inspetor oferece suporte a filtragem por meio da seleção de ponto no mapa. Prima a **Ctrl** da chave e, em seguida, clique e arraste com o mouse para formar um quadrado usando os pontos. Em seguida, aplica filtros, conforme descritos anteriormente.

Dimensionar rapidamente o mapa para mostrar apenas os pontos de possíveis ao premir o **i** no lado esquerdo do mapa.


## <a name="pattern-frequency"></a>Frequência de padrão 

Este inspector mostra uma lista de padrões na coluna de cadeia de caracteres selecionada. Os padrões são representados por uma expressão regular, como a sintaxe. Pairar o rato sobre o padrão mostra exemplos de valores representados por esse padrão. Juntamente com os padrões, também é mostrado os coverages aproximadas em termos de percentagem.

![Imagem de inspetor de padrão](media/data-prep-appendix4-supported-inspectors/PatternInspectorProductNumber.png)

### <a name="options"></a>Opções
- Número de valores mais altos
- Descendente
- Mostrar halo

### <a name="actions"></a>Ações
Esse inspetor oferece suporte a filtragem com base nos padrões apresentados. Prima a **Ctrl** da chave e, em seguida, selecione as barras de manchas no inspector padrão. Em seguida, aplica filtros, conforme descritos anteriormente. Como resultado acion utilizador, é adicionado um passo de filtro avançado. Pode ver e modificar o código de Python gerado ao invocar a opção de editar o passo de filtro avançado.
