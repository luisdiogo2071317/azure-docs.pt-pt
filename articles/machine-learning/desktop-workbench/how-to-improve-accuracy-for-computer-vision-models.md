---
title: Uma melhor precisão dos modelos de visão e a classificação de computador no Azure Machine Learning
description: Saiba como melhorar a precisão da classificação de imagens de visão do computador, deteção de objetos e modelos de semelhança de imagem com o pacote do Azure Machine Learning para imagem digitalizada.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: 6d6c540cd8711ae89784cdc797ca56d312522a83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989316"
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>Melhorar a precisão dos modelos de imagem digitalizada

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Com o **pacote do Azure Machine Learning para imagem digitalizada**, pode criar e implementar a classificação de imagens, deteção de objeto e modelos de semelhança de imagem. Saiba mais sobre este pacote e como instalá-lo.

Neste artigo, irá aprender a otimizar esses modelos para aumentar sua precisão. 

## <a name="accuracy-of-image-classification-models"></a>Precisão de modelos de classificação de imagens

O pacote de imagem digitalizada do computador é apresentado para dar bons resultados para uma grande variedade de conjuntos de dados. No entanto, como acontece com a máquina a maioria dos projetos de aprendizado, obter o melhor possível resultados para um novo conjunto de dados requer cuidado parâmetro otimização, bem como avaliar a decisões de conceção diferentes. As secções seguintes fornecem orientações sobre como melhorar a precisão num determinado conjunto de dados, ou seja, quais os parâmetros são mais promissoras para otimizar em primeiro lugar, quais os valores para estes parâmetros um deve tentar e quais armadilhas a serem evitadas.

Em termos gerais, modelos de aprendizagem aprofundada de treinamento é fornecido com uma compensação entre a hora de treinamento em comparação com a precisão do modelo. O pacote de imagem digitalizada do computador tem parâmetros de predefinição predefinidas (veja a primeira linha na tabela abaixo), quais se concentram na velocidade de treinamento rápido enquanto se produz, normalmente, os modelos de alta precisão. Essa precisão, muitas vezes, pode ser melhorada utilizando, por exemplo, maior resolução da imagem ou modelos mais profundos, no entanto ao custo aumenta o tempo de treinamento por um fator de 10x ou mais.

É recomendável que primeiro trabalhar com os parâmetros de padrão, preparar um modelo, inspecionar os resultados, corrija anotações de verdade de zero, conforme necessário e apenas em seguida, tente parâmetros diminuir o tempo de treinamento (consulte a tabela de valores de parâmetro sugeridos abaixo). Recomenda-se no entanto uma compreensão desses parâmetros, embora tecnicamente não seja necessário.


### <a name="best-practices-and-tips"></a>Práticas recomendadas e dicas

* Qualidade de dados: os conjuntos de formação e teste devem ser de alta qualidade. Ou seja, as imagens são anotadas corretamente, imagens ambíguas removido (por exemplo onde é não ficará claro para um olho humano se a imagem que mostra uma bola de tênis ou um lemon) e os atributos são mutuamente exclusivos (ou seja, cada imagem pertence a exatamente um atributo).

* Antes de refinar o DNN, um classificador SVM deve receber formação usando um DNN com formação prévia e fixo como featurizer. Isto é suportado no pacote de imagem digitalizada do computador e não requer muito para preparar uma vez que o DNN em si não está a ser modificado. Até mesmo essa abordagem simples, muitas vezes, distribui precisões bom e, por conseguinte, representa uma linha de base segura. A próxima etapa é, em seguida refinar o DNN que deve lhe dar uma melhor precisão.

* Se o objeto de interesse é pequeno na imagem, em seguida, abordagens de classificação de imagem são conhecidas por não funcionam bem. Nesses casos, considere a utilização de uma abordagem de deteção de objeto, como o mais rápido do R-CNN do pacote de imagem digitalizada computador com base em Tensorflow.

* Os dados de treinamento mais melhor. Como regra-de-geral, um deve ter, pelo menos, 100 exemplos para cada classe, ou seja, 100 imagens para "cachorro", 100 imagens para "gato", etc. Preparar um modelo com menos imagens é possível, mas poderia não produzir bons resultados.

* As imagens de formação precisam residir localmente na máquina com GPU e ser numa unidade SSD (não um HDD). Caso contrário, a latência de leitura de imagem pode reduzir drasticamente a velocidade de treinamento (por até mesmo um fator de 100 x).


### <a name="parameters-to-optimize"></a>Parâmetros para otimizar

Localizar valores ideal para estes parâmetros, é importante e, muitas vezes, pode melhorar significativamente precisão:
* Taxa de aprendizagem (`lr_per_mb`): O parâmetro comprovadamente mais importante de solucionar. Se a precisão no treinamento definida depois de Refinamento de DNN for superior a % de ~ 5, é muito provável que a taxa de aprendizagem é demasiado elevado, ou o número de epochs treinamento demasiado baixos. Especialmente com conjuntos de dados pequenos, o DNN tende a exceder o ajuste dos dados de treinamento, no entanto na prática, que isso levará a bons modelos de teste definido. Normalmente, usamos 15 epochs em que a taxa de aprendizagem inicial é reduzida para duas vezes; em alguns casos treinamento com mais epochs pode melhorar o desempenho.

* Resolução de entrada (`image_dims`): A resolução da imagem padrão é 224 x 224 pixels. Com resolução superior da imagem de, por exemplo, 500 x 500 pixels ou 1000 x 1000 pixels pode melhorar significativamente a precisão, mas pode atrasar o Refinamento de DNN. O pacote de imagem digitalizada do computador espera que a resolução de entrada para ser uma cadeia de identificação do (canais de cor, imagem largura, altura da imagem), por exemplo, (3, 224, 224), onde o número de canais de cor tem de estar definido como 3 (as faixas de vermelho-verde-azul).

* Arquitetura de modelo (`base_model_name`): Experimente utilizar DNNs mais aprofundadas, tais como utilizar o ResNet-34 ou utilizar o ResNet-50, em vez do modelo de utilizar o ResNet-18 padrão. O modelo de utilizar o Resnet-50 não é apenas mais aprofundado, mas o resultado da camada a mais é do floats de tamanho de 2048 (vs. 512 floats dos modelos de utilizar o ResNet-18 e utilizar o ResNet-34). Este aumento dimensionalidade pode ser benéfica sobretudo quando manter DNN fixo e treinamento em vez disso, um classificador SVM.

* Tamanho de minibatch (`mb_size`): tamanhos de alta minibatch irão conduzir a menor tempo de treinamento, no entanto às custas de um maior consumo de memória DNN. Por conseguinte, quando selecionar modelos mais profundos (por exemplo, utilizar o ResNet-50 versus utilizar o ResNet-18) e/ou de resolução de imagem superior (500\*500 pixels versus 224\*224 pixels), normalmente, é necessário reduzir o tamanho de minibatch para evitar erros de insuficiência de memória. Ao alterar o tamanho de minibatch, muitas vezes, a taxa de aprendizagem tem também de ser ajustado conforme pode ser visto na tabela abaixo.
* Taxa de lista-out (`dropout_rate`) e L2 regularizer (`l2_reg_weight`): ajuste excessivo do DNN pode ser reduzido através da utilização de uma taxa de dropout de 0,5 (a predefinição é 0,5 no pacote de imagem digitalizada do computador) ou mais e aumentando o peso de regularizer (a predefinição é 0.0005 na visão do computador Pacote). No entanto, observe que especialmente com conjuntos de dados pequeno DNN ajuste excessivo é difícil e, muitas vezes, impossível evitar.


### <a name="parameter-definitions"></a>Definições de parâmetros

- **Taxa de aprendizagem**: passo tamanho utilizado durante a aprendizagem de gradiente descendente. Se definida muito baixa, em seguida, o modelo terá muitos epochs para preparar, se o conjunto demasiado elevado, em seguida, o modelo não irá convergir para uma boa solução. Normalmente, uma agenda é utilizada em que a taxa de aprendizagem é reduzida após um determinado número de epochs. Agenda de velocidade de exemplo de E.For o aprendizado `[0.05]*7 + [0.005]*7 + [0.0005]` corresponde a uma taxa de aprendizagem inicial de 0,05 a utilizar para os primeiro sete epochs, seguido de um 10 vezes o ritmo de aprendizagem reduzida de 0.005 epochs sete outro e, finalmente, ajuste o modelo para um único "Epoch" com um x 100 diminui a taxa de aprendizagem de 0.0005.

- **Tamanho de minibatch**: GPUs podem processar várias imagens em paralelo para acelerar a computação. Estas imagens processadas paralelas também são referidas como um minibatch. Quanto maior for o tamanho de minibatch o treinamento mais rápido será, no entanto às custas de um maior consumo de memória DNN.

### <a name="recommended-parameter-values"></a>Valores de parâmetros recomendados

A tabela abaixo fornece conjuntos de parâmetros diferentes que foram mostrados para produzir os modelos de alta precisão numa grande variedade de tarefas de classificação de imagens. Os parâmetros otimizados dependem do conjunto de dados específico e a GPU exata utilizado, por conseguinte, a tabela deve ser vista como uma diretriz apenas. Depois de tentar esses parâmetros, considere também as resoluções de imagem de mais de 500 x 500 pixels ou modelos mais aprofundados de como utilizar o Resnet-101 ou utilizar o Resnet-152.

A primeira linha na tabela corresponde aos parâmetros predefinidos que estão definidos no pacote de imagem digitalizada do computador. Todas as outras linhas demoram mais tempo a treinar (indicado na primeira coluna) no entanto, o benefício de uma maior precisão (consulte a segunda coluna da precisão média ao longo de três conjuntos de dados internos). Por exemplo, os parâmetros na última linha demorar 5 a 15 x mais tempo a dar formação e no entanto resultou numa maior precisão (média) em três conjuntos de teste interno de 82.6% 92.8%.

Modelos-se e resolução de entrada superior ocupam mais memória DNN e, por conseguinte, o tamanho de minibatch deve ser reduzida com complexidade maior de modelo para evitar erros de memória fora das. Como pode ser visto na tabela abaixo, é benéfico diminuir o ritmo de aprendizagem por um fator de dois, sempre que a diminuir o minibatch de tamanho pelo multiplicador mesmo. O tamanho de minibatch poderá ter de obter reduzida ainda mais em GPUs com mais pequenas quantidades de memória.

| Tempo de treinamento (estimativa) | Precisão de exemplo | Tamanho de minibatch (*mb_size*) | Taxa de aprendizagem (*lr_per_mb*) | Resolução de imagem (*image_dims*) | Arquitetura DNN (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1 x (referência) | % de 82.6 | 32 | [0,05] \*7 + [0.005]\*7 + [0.0005]  | (3, 224, 224) | ResNet18_ImageNet_CNTK |
| 2 a 5 x    | % de 90.2 | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2 a 5 x    | % de 87.5 | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 224, 224) | ResNet50_ImageNet_CNTK |
| 5-15 vezes        | % de 92.8 |  8 | [0,01] \*7 + [0,001]\*7 + [0,0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre o pacote do Azure Machine Learning para imagem digitalizada:
+ Consulte a documentação de referência

+ Saiba mais sobre [outros pacotes de Python para o Azure Machine Learning](reference-python-package-overview.md)