---
title: Uma melhor exatidão dos modelos de visão & classificação do computador no Azure Machine Learning
description: Saiba como melhorar a precisão do seu computador visão imagem classificação, deteção de objeto e modelos de semelhança de imagem a utilizar o pacote do Azure Machine Learning para problemas de visão do computador.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: e134e1e544c51d6756d5021fef8c049fe7d8afb0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>Melhorar a precisão dos modelos de visão do computador

Com o **Azure Machine Learning pacote para computador visão**, pode criar e implementar a classificação de imagem, deteção de objeto e modelos de semelhança de imagem. Saiba mais sobre este pacote e como instalá-lo.

Neste artigo, irá aprender a otimizar estes modelos para aumentar os respetivos precisão. 

## <a name="accuracy-of-image-classification-models"></a>Precisão dos modelos de classificação de imagem

O pacote de imagem digitalizada do computador é apresentado para dar boas resultados para uma grande variedade de conjuntos de dados. No entanto, como é verdadeiro para a maioria das machine learning projetos, obter o melhor possíveis resultados para um novo conjunto de dados necessita de otimização parâmetro cuidado, bem como a avaliar as decisões de conceção diferentes. As secções seguintes fornecem orientações sobre como melhorar a precisão num determinado conjunto de dados, ou seja, são mais promising para otimizar em primeiro lugar, quais os valores para estes parâmetros um deve experimentar e que pitfalls para evitar que parâmetros.

Um modo geral, os modelos de Learning profunda de formação inclui um compromisso entre a hora de formação em comparação com a precisão do modelo. O pacote de imagem digitalizada do computador tem parâmetros de predefinição predefinidos (veja a primeira linha na tabela abaixo) que focar-se na velocidade de formação rápido ao produzir, normalmente, os modelos de precisão elevada. Este precisão, muitas vezes, pode ser melhorada, utilizando, por exemplo, superior resolução da imagem ou modelos de mais aprofundados, no entanto, mas utiliza aumentar o tempo de formação, um fator de 10x ou mais.

É recomendado que primeiro trabalhar com os parâmetros predefinidos, preparar um modelo, Inspecione os resultados, corrija zero truth anotações, conforme necessário e apenas, em seguida, tente parâmetros abrandar o tempo de preparação (consulte a tabela abaixo os valores de parâmetros sugerida). No entanto é recomendada uma compreensão destes parâmetros embora tecnicamente não seja necessário.


### <a name="best-practices-and-tips"></a>Melhores práticas e sugestões

* Qualidade de dados: os conjuntos de formação e teste devem ser de alta qualidade. Ou seja, as imagens foram anotadas corretamente, imagens ambíguas removido (por exemplo em que é claro para um olho humano se a imagem que mostra um ball tennis ou um lemon) e os atributos são mutuamente exclusivos (ou seja, cada imagem pertence a exatamente um atributo).

* Antes de limitar o DNN, deve ser preparado um classificador SVM utilizando um DNN fixo e previamente treinado como featurizer. Isto é suportado no pacote de imagem digitalizada do computador e não necessita de tempo para preparar uma vez que o DNN propriamente dito não está a ser modificada. Mesmo esta abordagem simple, muitas vezes, distribui accuracies bom e, por conseguinte, representa uma linha de base segura. O passo seguinte consiste em seguida otimizar o DNN que deve dar uma melhor exatidão.

* Se o objeto de interesse for pequeno na imagem, em seguida, abordagens de classificação de imagem se sabe não funciona. Nestes casos, considere a utilização de uma abordagem de deteção de objetos, tais como mais rapidamente do R-CNN do pacote de imagem digitalizada computador com base no Tensorflow.

* Os dados de formação mais melhor. Como uma regra de-de-geral, um deve ter, pelo menos, 100 exemplos para cada classe, ou seja, 100 imagens para "preguiçoso", 100 imagens para "cat", etc. Um modelo com menos de imagens de preparação, é possível mas poderia não produzir resultados bom.

* As imagens de preparação tem de residir localmente no computador com o GPU e ser numa unidade SSD (não um HDD). Caso contrário, a latência de leitura de imagem pode reduzir significativamente o velocidade de formação (ao mesmo um fator de 100 x).


### <a name="parameters-to-optimize"></a>Parâmetros para otimizar o

Localizar ideal valores para estes parâmetros é importante e, muitas vezes, pode melhorar significativamente a exatidão da:
* Taxa de aprendizagem (`lr_per_mb`): O parâmetro possivelmente mais importante para obter à direita. Se a precisão em de preparação definido depois DNN refinement é superior a % de ~ 5, em seguida, provavelmente o ritmo de aprendizagem é demasiado elevado, ou o número de epochs formação demasiado baixos. Especialmente com conjuntos de dados pequenos, o DNN tende a excessiva ajustará os dados de formação, no entanto na prática, que isto irá originar a modelos de boas de teste definida. Normalmente, utilizamos 15 epochs onde o ritmo de aprendizagem inicial é reduzido duas vezes; em alguns casos formação utilizando epochs mais pode melhorar o desempenho.

* Resolução de entrada (`image_dims`): A resolução da imagem predefinida é 224 x 224 pixéis. Utilizar superior resolução da imagem de, por exemplo, 500 x 500 pixels ou 1000 x 1000 pixels pode significativamente melhorar a precisão, mas atrasar DNN refinement. O pacote de imagem digitalizada do computador espera que a resolução de entrada para ser uma cadeia de identificação de (canais de cor de imagem largura, altura da imagem), por exemplo, (3 224, 224), onde o número de canais de cor tem de ser definido para 3 (bandas Red-verde-azul).

* Arquitetura de modelo (`base_model_name`): Experimente utilizar DNNs mais aprofundadas, tais como ResNet 34 ou ResNet 50 em vez do modelo de ResNet-18 predefinido. O modelo de Resnet 50 não é só mais aprofundado, mas o resultado da camada penultimate de floats tamanho 2048 (vs. 512 floats dos modelos ResNet-18 e ResNet 34). Este aumento dimensionalidade pode ser vantajoso especialmente quando manter DNN fixo e em vez disso, um classificador SVM de preparação.

* Tamanho de minibatch (`mb_size`): tamanhos minibatch elevada resultará mais rápido do tempo de formação contudo em detrimento de um maior consumo de memória DNN. Por conseguinte, quando selecionar modelos mais profundo (por exemplo, ResNet a 50 versus ResNet-18) e/ou a resolução da imagem superior (500\*500 pixels versus 224\*224 pixels), um normalmente tem de reduzir o tamanho de minibatch para evitar erros de memória esgotada. Quando alterar o tamanho de minibatch, muitas vezes, também o ritmo de aprendizagem tem de ser ajustado como podem ser vistos na tabela abaixo.
* Taxa de ignorados fora (`dropout_rate`) e de secretária L2 regularizer (`l2_reg_weight`): ajuste superior DNN pode ser reduzido através da utilização de uma taxa de dropout de 0,5 (a predefinição é 0,5 no pacote de imagem digitalizada do computador) ou mais e do aumento a ponderação regularizer (a predefinição é 0.0005 no visão do computador Pacote). Tenha em atenção que que especialmente com conjuntos de dados pequenos DNN ajuste superior é difícil e, muitas vezes, é impossível evitar.


### <a name="parameter-definitions"></a>Definições de parâmetros

- **Taxa de aprendizagem**: passo tamanho utilizado durante a aprendizagem gradação descendente. Se definido demasiado baixo, em seguida, o modelo terá muitos epochs para preparar, se o conjunto demasiado elevado, em seguida, o modelo não irá convergir uma boa solução. Normalmente, uma agenda é utilizada em que o ritmo de aprendizagem é reduzido após um determinado número de epochs. Agenda de velocidade de exemplo E.For a aprendizagem `[0.05]*7 + [0.005]*7 + [0.0005]` corresponde a um ritmo de aprendizagem inicial de 0,05 a utilizar para os primeiro sete epochs, seguido de um 10x ritmo de aprendizagem reduzida de 0.005 para outro epochs sete e, finalmente, fine-tuning o modelo para uma único época com um x 100 reduzido ritmo de aprendizagem do 0.0005.

- **Tamanho de minibatch**: GPUs podem processar várias imagens em paralelo para acelerar a computação. Estas imagens de processamento paralelas são também conhecidas como um minibatch. Quanto maior for o tamanho de minibatch a formação mais rápida irá ser, no entanto em detrimento de um maior consumo de memória DNN.

### <a name="recommended-parameter-values"></a>Valores de parâmetros recomendada

A tabela abaixo fornece os conjuntos de parâmetros diferentes que foram apresentados para produzir os modelos de precisão elevada numa ampla variedade de tarefas de classificação de imagem. Os parâmetros ideal dependem o conjunto de dados específico e a GPU exato utilizado, por conseguinte, a tabela deve ser vista como orientação apenas. Depois de a tentar estes parâmetros, considere também as resoluções de imagem de mais do que 500 x 500 pixels ou modelos mais aprofundados, tais como Resnet 101 ou Resnet 152.

A primeira linha na tabela corresponde a parâmetros predefinidos que estão definidos no interior do pacote de imagem digitalizada do computador. Todas as outras linhas demorar mais tempo a formação (indicados na primeira coluna) no entanto, a vantagem de maior precisão (consulte a segunda coluna para a precisão média ao longo de três conjuntos de dados internos). Por exemplo, os parâmetros na última linha demorar 5-15 x mais para preparar, no entanto resultou numa maior precisão (média) em três conjuntos de teste interno de 82.6% 92.8%.

Modelos mais aprofundados e resolução de entrada superior demorar mais memória DNN e, por conseguinte, o tamanho de minibatch tem de ser reduzida, com a complexidade de aumento de modelo para evitar o limite de memória erros. Como podem ser vistos na tabela abaixo, é vantajoso diminuir o ritmo de aprendizagem por um fator de dois sempre que o minibatch a diminuir o tamanho o multiplicador do mesmo. O tamanho de minibatch poderá ter de obter reduzido adicional no GPUs com mais pequenas quantidades de memória.

| Tempo de formação (estimativa aproximada) | Precisão de exemplo | Tamanho de minibatch (*mb_size*) | Taxa de aprendizagem (*lr_per_mb*) | Resolução de imagem (*image_dims*) | Arquitetura DNN (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1 x (referência) | % de 82.6 | 32 | [0,05] \*7 + [0.005]\*7 + [0.0005]  | (3 224, 224) | ResNet18_ImageNet_CNTK |
| 2-5x    | % de 90.2 | 16 | [0.025] \*7 + [0.0025]\*7 + [0.00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2-5x    | % de 87.5 | 16 | [0.025] \*7 + [0.0025]\*7 + [0.00025] | (3 224, 224) | ResNet50_ImageNet_CNTK |
| 5-15 x        | % de 92.8 |  8 | [0.01] \*7 + [0,001]\*7 + [0,0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre o pacote do Azure Machine Learning para problemas de visão do computador:
+ Consulte a documentação de referência

+ Saiba mais sobre [outros pacotes do Python para o Azure Machine Learning](reference-python-package-overview.md)