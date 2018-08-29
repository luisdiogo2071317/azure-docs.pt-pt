---
title: Com o CNTK dentro do Azure Machine Learning Workbench de classificação de imagens | Documentos da Microsoft
description: Treinar, avaliar e implementar um modelo de classificação de imagem personalizada com o Azure ML Workbench.
services: machine-learning
documentationcenter: ''
author: PatrickBue
ms.author: pabuehle
manager: mwinkle
ms.reviewer: marhamil, mldocs, garyericson, jasonwhowell
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 10/17/2017
ms.openlocfilehash: 48c21638fe5756e6527288ed0fdc73dd9e331afd
ms.sourcegitcommit: baed5a8884cb998138787a6ecfff46de07b8473d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "35622218"
---
# <a name="image-classification-using-azure-machine-learning-workbench"></a>Classificação de imagens com o Azure Machine Learning Workbench

Abordagens de classificação de imagem podem ser utilizadas para resolver um grande número de problemas de visão do computador.
Estes incluem a criação de modelos, que responder a perguntas como: *é um OBJETO presente na imagem?* onde o OBJETO por exemplo poderia ser *dog*, *carro*, ou  *Envie*. Ou perguntas mais complexas, como: *o que a classe de severidade de doenças de olho é evinced por verificação de retinal este paciente?*.

Este tutorial endereços resolver esses problemas. Vamos mostrar como treinar, avaliar e implementar a sua própria classificação de imagem modelo com o [Microsoft Cognitive Toolkit (CNTK) ](https://docs.microsoft.com/cognitive-toolkit/) para aprendizagem profunda.
Imagens de exemplo são fornecidas, mas o leitor pode também traga seu próprio conjunto de dados e preparar os seus próprios modelos personalizados.

Imagem digitalizada soluções tradicionalmente necessário conhecimento especializado para identificar e implementar chamado manualmente *funcionalidades*, que realçar as informações desejadas em imagens.
Essa abordagem manual alteradas no 2012 com o famoso [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) [1] aprendizagem paper e neste momento, as redes Neurais profundas (DNN) são utilizadas para localizar automaticamente esses recursos.
DNNs levados a uma grande melhoria no campo não apenas para classificação de imagens, mas também para outros problemas de visão do computador, como deteção de objetos e semelhança de imagem.


## <a name="link-to-the-gallery-github-repository"></a>Ligar para o repositório do GitHub de galeria
[https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK](https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK)

## <a name="overview"></a>Descrição geral

Este tutorial é dividido em três partes:

- Parte 1 mostra como treinar, avaliar e implementar um sistema de classificação de imagem usando um DNN com formação prévia como featurizer e formação um SVM em sua saída.
- Em seguida, parte 2 mostra como melhorar a precisão ao, por exemplo, refinar o DNN, em vez de usá-lo como um featurizer fixo.
- Parte 3 aborda como utilizar o seu próprio conjunto de dados em vez das imagens de exemplo fornecido e, se necessário, como produzir seu próprio conjunto de dados pela captura de imagens a partir da net.

Embora não seja necessária experiência anterior com machine learning e CNTK, é útil para entender os princípios subjacentes. Números de precisão, hora, etc. comunicado no tutorial de preparação são apenas para referência, e os valores de reais ao executar o código quase certamente diferentes.


## <a name="prerequisites"></a>Pré-requisitos

As pré-requisitos para executar este exemplo são os seguintes:

1. Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
2. O [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) seguintes a [guia de início rápido de instalação](../service/quickstart-installation.md) para instalar o programa e criar uma área de trabalho.  
3. Uma máquina Windows. SO do Windows é necessário uma vez que a Bancada de trabalho suporta apenas Windows e MacOS, ao conjunto de ferramentas cognitivas da Microsoft (que usamos como biblioteca de aprendizagem profunda) só suporta o Windows e Linux.
4. Uma GPU dedicada não é necessária para executar o treinamento SVM na parte 1, no entanto, é necessário para refinar de DNN descrito na parte 2. Se não têm uma GPU forte, pretende dar formação em vários GPUs ou não tem uma máquina Windows, considere utilizar Deep Learning Virtual Machine do Azure com o sistema de operativo do Windows. Ver [aqui](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning) para obter um guia de implementação de clique de 1. Depois de implementada, ligar à VM através de uma ligação de ambiente de trabalho remota, instale a Bancada de trabalho de existir e executar o código localmente a partir da VM.
5. Várias bibliotecas de Python como OpenCV tem de ser instalado. Clique em *linha de comandos aberta* partir do *ficheiro* menu na bancada de trabalho e execute os seguintes comandos para instalar estas dependências:  
    - `pip install https://cntk.ai/PythonWheel/GPU/cntk-2.2-cp35-cp35m-win_amd64.whl`  
    - `pip install opencv_python-3.3.1-cp35-cp35m-win_amd64.whl` Depois de baixar o OpenCV roda de http://www.lfd.uci.edu/~gohlke/pythonlibs/ (o mesmo nome de ficheiro e versão podem alterar)
    - `conda install pillow`
    - `pip install -U numpy`
    - `pip install bqplot`
    - `jupyter nbextension enable --py --sys-prefix bqplot`
    - `jupyter nbextension enable --py widgetsnbextension`

### <a name="troubleshooting--known-bugs"></a>Bugs de resolução de problemas / conhecidos
- Uma GPU é necessária para a parte 2 e, caso contrário, o erro "Batch treinamento de normalização em CPU ainda não está implementado" é acionada uma ao tentar refinar o DNN.
- Erros de insuficiência de memória durante o treinamento de DNN podem ser evitados ao reduzir o tamanho de minibatch (variável `cntk_mb_size` em `PARAMETERS.py`).
- O código foi testado utilizam CNTK 2.2, e deve também executadas nos mais antiga (até v2.0) e mais recentes versões sem qualquer ou apenas com pequenas alterações.
- No momento da escrita, o Azure Machine Learning Workbench tiveram problemas que mostra os blocos de notas maior do que 5 \ MBytes disponíveis. Blocos de notas deste tamanho grande podem acontecer se o bloco de notas é guardado com todas as células saída apresentada. Se encontra este erro, em seguida, abra a linha de comandos no menu Arquivo dentro do Workbench, executar `jupyter notebook`, abra o bloco de notas, desmarque todos os resultados e guardar o bloco de notas. Depois de efetuar estes passos, o bloco de notas será aberto corretamente dentro do Azure Machine Learning Workbench novamente.
- Todos os scripts fornecidos neste exemplo tem de ser executado localmente e não por exemplo, um ambiente remoto do docker. Todos os blocos de notas tem de ser executado com o kernel definido como o kernel do projeto local com o nome "PROJECTNAME local" (por exemplo, "myImgClassUsingCNTK local").

    
## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do workbench

Para criar um novo projeto com este exemplo como um modelo:
1.  Abra o Azure Machine Learning Workbench.
2.  Sobre o **projetos** página, clique no **+** inicie sessão e selecione **novo projeto**.
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto.
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Classificação de imagens" e selecione o modelo.
5.  Clique em **Criar**.

Executar os passos abaixo cria a estrutura do projeto mostrada abaixo. O diretório de projeto é restrito a ser menos de 25 Mbytes, uma vez que o Azure Machine Learning Workbench cria uma cópia desta pasta após cada execução (para habilitar o histórico de execuções). Portanto, todas as imagens e arquivos temporários são guardados de e para o diretório *~/Desktop/imgClassificationUsingCntk_data* (denominados *DATA_DIR* neste documento).

  Pasta| Descrição
  ---|---
  aml_config/|                           Diretório que contém os ficheiros de configuração do Azure Machine Learning Workbench
  bibliotecas /|                              Diretório que contém todas as funções de programa auxiliar de Python e o Jupyter
  blocos de notas /|                              Diretório que contém todos os blocos de notas
  recursos /|                              Diretório que contém todos os recursos (para o url do exemplo de imagens de moda)
  scripts /|                              Diretório que contém todos os scripts
  PARAMETERS.py|                       Especificar todos os parâmetros de script de Python
  readme.md|                           Este documento Leia-me


## <a name="data-description"></a>Descrição de dados

Este tutorial utiliza como executar um conjunto de dados para os textura de vestuário corpo superior, que se consistindo em até 428 imagens de exemplo. Cada imagem está anotada como um dos três texturas diferentes (leopard pontilhada, repartido,). Mantivemos o número de imagens pequenas, para que este tutorial pode ser executado rapidamente. No entanto, o código é bem testado e funciona com dezenas de milhares de imagens ou mais. Todas as imagens foram obtida com a pesquisa de imagens do Bing e mão-anotados conforme é explicado [parte 3](#using-a-custom-dataset). A imagem de URLs com seus respectivos atributos estão listados na */resources/fashionTextureUrls.tsv* ficheiro.

O script `0_downloadData.py` downloads todas as imagens para o *DATA_DIR/imagens/fashionTexture/* diretório. Alguns dos URLs de 428 são provavelmente quebradas. Não é um problema e apenas significa que temos um pouco menos imagens para formação e testar. Todos os scripts fornecidos neste exemplo tem de ser executado localmente e não por exemplo, um ambiente remoto do docker.

A figura seguinte mostra exemplos para os atributos pontuados (esquerda), repartido (meio) e leopard (direita). Anotações tivesse terminadas de acordo com o item de vestuário corpo superior.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/examples_all.jpg"  alt="alt text" width="700">
</p>


## <a name="part-1---model-training-and-evaluation"></a>Parte 1 - de modelo de formação e avaliação

Na primeira parte deste tutorial, iremos são treinar um sistema que utiliza, mas não modificar, uma rede neural profunda com formação prévia. Este DNN com formação prévia é utilizado como um featurizer, e um SVM linear é preparado para prever o atributo (com pontos, repartidos, ou leopard) de uma determinada imagem.

Agora que descrevemos essa abordagem em detalhes, passo a passo e mostram quais scripts precisam ser executadas. Recomendamos que após cada passo para inspecionar os ficheiros que são escritos e em que são escritos.

Todos os parâmetros importantes são especificados, e uma breve explicação fornecido, num único local: o `PARAMETERS.py` ficheiro.




### <a name="step-1-data-preparation"></a>Passo 1: Preparação de dados
`Script: 1_prepareData.py. Notebook: showImages.ipynb`

O bloco de notas `showImages.ipynb` podem ser utilizados para visualizar as imagens e para corrigir seu anotação conforme necessário. Para executar o bloco de notas, abra-o no Azure Machine Learning Workbench, clique em "Iniciar bloco de notas do servidor" se esta opção é apresentada, altere para o kernel do projeto local com o nome "PROJECTNAME local" (por exemplo, "myImgClassUsingCNTK local"), e, em seguida, executar todas as células a Bloco de notas. Consulte a secção de resolução de problemas neste documento se obtiver um erro reclamando que o bloco de notas é demasiado grande para ser apresentado.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showImages.jpg" alt="alt text" width="700"/>
</p>

Agora a executar o script com o nome `1_prepareData.py`, que atribui todas as imagens para a formação de qualquer conjunto ou definir o teste. Esta atribuição é mutuamente exclusiva - não existe nenhuma imagem de treinamento também é utilizada para fins de teste ou vice versa. Por predefinição, um aleatório 75% das imagens de cada classe de atributo são atribuídos ao treinamento e, a 25% restantes são atribuídas ao teste. Todos os dados gerados pelo script são guardados no *DATA_DIR/proc/fashionTexture/* pasta.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_1_white.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-2-refining-the-deep-neural-network"></a>Passo 2: Refinar a rede Neural profunda
`Script: 2_refineDNN.py`

Como Explicamos na parte 1 deste tutorial, o DNN com formação prévia permanece fixo (ou seja, não é refinada). No entanto, o script com o nome `2_refineDNN.py` ainda é executada na parte 1, como ele carrega uma com formação prévia [utilizar o ResNet](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) modelo [2] e modifica-lo, por exemplo, para permitir a resolução de entrada de imagem superior. Este passo é rápida (segundos) e não requer uma GPU.

Na parte 2 do tutorial, as causas de ficheiros de uma modificação a PARAMETERS.py o `2_refineDNN.py` script para refinar também o DNN com formação prévia. Por predefinição, executamos 45 epochs de formação durante o refinamento.

Em ambos os casos, o modelo final, em seguida, é escrito no ficheiro *DATA_DIR/proc/fashionTexture/cntk_fixed.model*.

### <a name="step-3-evaluate-dnn-for-all-images"></a>Passo 3: Avaliar DNN para todas as imagens
`Script: 3_runDNN.py`

Agora podemos usar o DNN (possivelmente refinado) do último passo para caracterização as nossas imagens. Devido uma imagem como entrada para o DNN, a saída é o vetor de 512 floats da camada a mais do modelo. Esse vetor é muito menor dimensional do que a imagem propriamente dita. No entanto, ele deve conter (e até mesmo realçam) todas as informações a imagem relevante para reconhecer o atributo da imagem, que é, se o item de vestuário tem pontilhada, distribuído ou textura leopard.

Todas as representações de imagem DNN são guardadas no ficheiro *DATA_DIR/proc/fashionTexture/cntkFiles/features.pickle*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_4_white.jpg" alt="alt text" width="700"/>
</p>


### <a name="step-4-support-vector-machine-training"></a>Passo 4: Treinamento de máquina de Vetor com suporte
`Script: 4_trainSVM.py`

512-floats representações calculadas no último passo agora são usadas para treinar um classificador SVM: devido uma imagem como entrada, o SVM produz uma classificação para cada atributo estar presente. No nosso conjunto de dados de exemplo, isso significa que uma classificação para "repartidos", para "pontilhada" e para "leopard".

Script `4_trainSVM.py` carrega as imagens de formação, prepara um SVM para diferentes valores do parâmetro regularização (slack) C e mantém o SVM com precisão mais elevada. A precisão da classificação é impresso no console e desenhada na bancada de trabalho. Para os dados de textura fornecido estes valores devem ser aproximadamente 100% e 88%, respetivamente. Por fim, o SVM preparado é gravada no arquivo *DATA_DIR/proc/fashionTexture/cntkFiles/svm.np*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/vienna_svm_log_zoom.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-5-evaluation-and-visualization"></a>Passo 5: Avaliação e visualização
`Script: 5_evaluate.py. Notebook: showResults.ipynb`

A precisão do classificador de preparação de imagem pode ser medida usando o script `5_evaluate.py`. As pontuações de script todas as imagens de teste utilizando o classificador SVM preparado, atribui cada imagem o atributo com a pontuação mais alta e compara os atributos previstas com as anotações de verdade de zero.

A saída do script `5_evaluate.py` é mostrado abaixo. A precisão da classificação de cada classe individual é calculada, bem como a precisão para o conjunto de teste completo ('precisão geral") e a média com as precisões individuais ('média de classe precisão geral"). 100% corresponde à maior precisão possível e 0% para o pior. A adivinhação aleatórios em média a produzir uma precisão de média de classe 1 sobre o número de atributos: no nosso caso, essa precisão seria 33.33%. Esses resultados melhoram significativamente quando utilizar uma resolução de entrada superior como `rf_inputResoluton = 1000`no entanto, ao custo de computação de DNN períodos.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_6_white.jpg" alt="alt text" width="700"/>
</p>

Além de precisão, a curva cor MULTICLASSE é desenhada com a respectiva área-em-curva (esquerda); e a matriz de confusão é mostrado (direita):

<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat.jpg" alt="alt text" width="700"/>
</p>

Por fim, o bloco de notas `showResults.py` é fornecido para percorrer as imagens de teste e visualizar suas pontuações da respetiva classificação. Tal como explicado no passo 1, cada bloco de notas neste exemplo tem de utilizar o kernel do projeto local com o nome "PROJECTNAME local":
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showResults.jpg" alt="alt text" width="700"/>
</p>





### <a name="step-6-deployment"></a>Passo 6: implementação
`Scripts: 6_callWebservice.py, deploymain.py. Notebook: deploy.ipynb`

O sistema preparado agora pode ser publicado como uma API REST. Implementação é explicada no bloco de notas `deploy.ipynb`e com base na funcionalidade dentro do Azure Machine Learning Workbench (Lembre-se definir como o kernel do kernel do projeto local com o nome "PROJECTNAME local"). Consulte também a seção de implantação excelente a [IRIS tutorial](tutorial-classifying-iris-part-3.md) para a implementação de obter mais informações relacionadas.

Uma vez implantado, o serviço web pode ser chamado utilizando o script `6_callWebservice.py`. Tenha em atenção que o endereço IP (local ou na cloud) do serviço web tem de ser definido pela primeira vez no script. O bloco de notas `deploy.ipynb` explica como encontrar este endereço IP.








## <a name="part-2---accuracy-improvements"></a>Parte 2 - melhorias de precisão

Na parte 1, mostramos como classificar uma imagem por uma máquina de Vetor com suporte linear na saída 512 floats de uma rede Neural profunda de treinamento. Este DNN foi com formação prévia em milhões de imagens e a camada a mais devolvido como vetor de funcionalidade. Essa abordagem é rápida, uma vez que o DNN é utilizado como-é, mas, no entanto, muitas vezes, fornece bons resultados.

Agora, apresentamos o várias formas de melhorar a precisão do modelo a partir da parte 1. Mais notavelmente podemos refinar o DNN, em vez de mantê-lo a corrigir.

### <a name="dnn-refinement"></a>Refinamento de DNN

Em vez de um SVM, um pode fazer a classificação diretamente na rede neural. Isto é conseguido ao adicionar uma nova camada de último a DNN com formação prévia, o que leva as 512-floats da camada a mais como entrada. A vantagem de fazer a classificação no DNN é que agora a toda a rede pode reestruturar usando backpropagation. Essa abordagem é muitas vezes conduzem à muito melhor precisões de classificação em comparação ao uso do DNN com formação prévia como-é, no entanto às custas de tempo de treinamento muito maior (mesmo com GPU).

Treinando a rede Neural em vez de um SVM é feito alterando a variável `classifier` no `PARAMETERS.py` partir `svm` para `dnn`. Em seguida, conforme descrito na parte 1, todos os scripts, exceto a preparação de dados (etapa 1) e o treinamento de SVM (etapa 4) tem de ser executados novamente. Refinamento de DNN requer uma GPU. Se não foi encontrado nenhum GPU ou se a GPU é bloqueada (por exemplo, uma execução anterior do CNTK), em seguida, o script `2_refineDNN.py` emite um erro. Treinamento de DNN pode lançar o erro de fora da memória em algumas GPUs, o que podem ser evitadas ao reduzir o tamanho de minibatch (variável `cntk_mb_size` em `PARAMETERS.py`).

Após a conclusão do treinamento, o modelo refinado está guardado *DATA_DIR/proc/fashionTexture/cntk_refined.model*, e um desenho desenhada que mostra como os erros de classificação de preparação e teste alteram durante o treinamento. Tenha em atenção em que desenho que o erro no conjunto de treinamento é muito menor do que no conjunto de teste. Esse comportamento de excesso fitting chamado pode ser reduzido, por exemplo, utilizando um valor mais alto para a taxa de dropout `rf_dropoutRate`.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_3_plot.png" alt="alt text" height="300"/>
</p>

Como pode ser visto no desenho abaixo, a precisão com o Refinamento de DNN no conjunto de dados fornecido é de % de 92.35 versus a % de 88.92 antes (parte 1). Em particular, as imagens de 'pontilhadas' melhoram significativamente, com uma curva cor MULTICLASSE área-em-de 0.98 com o refinamento vs. 0.94 antes. Estamos a utilizar um pequeno conjunto de dados e, por conseguinte, os reais precisões de execução do código são diferentes. Essa discrepância é devido a stochastic gradient efeitos como a divisão aleatório das imagens em preparação e teste conjuntos.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat_dnn.jpg" alt="alt text" width="700"/>
</p>

### <a name="run-history-tracking"></a>Executar o controlo de histórico

Os arquivos do Azure Machine Learning Workbench o histórico de cada execução no Azure para permitir a comparação de duas ou mais execuções, que são até mesmo semanas diferença. Isso é explicado detalhadamente no [Iris tutorial](tutorial-classifying-iris-part-2.md). Também é mostrado nas capturas de ecrã seguintes, onde podemos comparar duas execuções do script `5_evaluate.py`, utilizando qualquer um dos Refinamento de DNN ou seja, `classifier = "dnn"`(número de execuções 148) ou SVM treinamento ou seja, `classifier = "svm"` (número de execuções 150).

A primeira captura de ecrã, o Refinamento de DNN resulta numa melhor precisões de treinamento de SVM para todas as classes. A segunda captura de ecrã mostra todas as métricas que estão sendo controladas, incluindo qual era o classificador. Esse controle é feito no script `5_evaluate.py` chamando o agente de log do Azure Machine Learning Workbench. Além disso, o script também salva a matriz de curva e a confusão de ROC para o *produz* pasta. Isso *produz* pasta é especial em que seu conteúdo também é controlado pela funcionalidade de histórico de Bancada de trabalho e, por conseguinte, os ficheiros de saída podem ser acedidos em qualquer altura, independentemente se foram substituídas cópias locais.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison1.jpg" alt="alt text" width="700"/>
</p>

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison2b.jpg" alt="alt text" width="700"/>
</p>


### <a name="parameter-tuning"></a>Otimização de parâmetro

Como é verdadeiro para a máquina a maioria dos projetos de aprendizado, obter bons resultados para um novo conjunto de dados requer que o parâmetro cuidadoso de otimização, bem como para avaliar as decisões de conceção diferentes. Para ajudar com estas tarefas, todos os parâmetros importantes são especificados e uma breve explicação fornecido, num único local: o `PARAMETERS.py` ficheiro.

Alguns dos meios mais promissoras para melhorias são:

- Qualidade de dados: Certifique-se de que os conjuntos de formação e teste têm alta qualidade. Ou seja, as imagens são corretamente anotadas, ambíguas imagens removidas (por exemplo itens de vestuário com reparte e pontos), e os atributos são mutuamente exclusivos (ou seja, escolhido, de modo a que cada imagem pertence a exatamente um atributo).

- Se o objeto de interesse é pequeno na imagem, em seguida, abordagens de classificação de imagem se sabe não funcionam bem. Nesses casos, considere utilizar uma abordagem de deteção de objeto, conforme descrito neste [tutorial](https://github.com/Azure/ObjectDetectionUsingCntk).
- Refinamento de DNN: O parâmetro comprovadamente mais importante de solucionar é a taxa de aprendizagem `rf_lrPerMb`. Se a precisão em de preparação definido (figura primeiro na parte 2) não está próximo de 0 a 5%, muito provavelmente é devido a um problema a taxa de aprendizagem. Os outros parâmetros a partir do `rf_` são menos importantes. Normalmente, o erro de treinamento deve exponencialmente diminuir e ser próximas de 0% depois de treinamento.

- Resolução de entrada: A resolução da imagem padrão é 224 x 224 pixels. Utilizar a resolução de imagem superior (parâmetro: `rf_inputResoluton`), por exemplo, 448 x 448 ou 896 x 896 pixels significativas, muitas vezes, melhora a precisão, mas pode atrasar o Refinamento de DNN. **Utilizar a resolução de imagem superior é praticamente de bônus e quase sempre aumenta a precisão**.

- Ajuste excessivo do DNN: evitar uma grande diferença entre a formação e a precisão de teste durante o Refinamento de DNN (primeiro figura na parte 2). Essa lacuna pode ser reduzida com base em tarifas dropout `rf_dropoutRate` de 0,5, ou mais e aumentando o peso de regularizer `rf_l2RegWeight`. Usar uma taxa elevada de dropout pode ser especialmente útil se a resolução de entrada de imagem DNN é elevada.

- Tente utilizar DNNs mais aprofundadas, alterando `rf_pretrainedModelFilename` partir `ResNet_18.model` a qualquer uma `ResNet_34.model` ou `ResNet_50.model`. O modelo de utilizar o Resnet-50 não é apenas mais aprofundado, mas o resultado da camada a mais é do floats de tamanho de 2048 (vs. 512 floats dos modelos de utilizar o ResNet-18 e utilizar o ResNet-34). Esta dimensão uma maior pode ser especialmente útil ao treinar um classificador SVM.

## <a name="part-3---custom-dataset"></a>Parte 3 - conjunto de dados personalizada

Na parte 1 e 2, treinados e avaliado um modelo de classificação de imagem usando as imagens de texturas de vestuário corpo superior. Agora, vamos mostrar como utilizar um conjunto de dados personalizado de fornecidos pelo usuário em vez disso. Ou, se não está disponível, como gerar e anotar como um conjunto de dados através do Bing imagens de pesquisa.

### <a name="using-a-custom-dataset"></a>Usando um conjunto de dados personalizado

Em primeiro lugar, vamos dar uma olhada na estrutura de pastas para os dados de textura de vestuário. Tenha em atenção como todas as imagens para os atributos diferentes são nas respetivas subpastas *pontilhada*, * leopard, e *repartidos* na *DATA_DIR/imagens/fashionTexture/*. Observe também como o nome da pasta de imagem também ocorre no `PARAMETERS.py` ficheiro:
```python
datasetName = "fashionTexture"
```

Usar um conjunto de dados personalizado é tão simples como reproduzir essa estrutura de pasta em que todas as imagens são em subpastas, de acordo com seus atributos e para copiar estas subpastas para um novo diretório especificado pelo utilizador *DATA_DIR/imagens/newDataSetName/*. A única alteração de código necessária é definir os `datasetName` variável à *newDataSetName*. Scripts de 1 a 5, em seguida, podem ser executados por ordem, e todos os arquivos intermediários são escritos *DATA_DIR/proc/newDataSetName/*. Sem alterações de código são necessárias.

É importante que cada imagem pode ser atribuída a exatamente um atributo. Por exemplo, seria errado precisar atributos para "animal" e "leopard", uma vez que uma imagem de "leopard" também teria de pertencer ao "animal". Além disso, é melhor remover as imagens que são ambíguas e, por conseguinte, difícil de anotar.



### <a name="image-scraping-and-annotation"></a>Captura de imagem e a anotação

Recolher um número grande o suficiente de anotado imagens de formação e teste pode ser difícil. Uma forma de solucionar esse problema é extrair imagens a partir da Internet. Por exemplo, veja abaixo os resultados de pesquisa de imagens Bing, da consulta *camiseta repartidos*. Conforme o esperado, a maioria das imagens, de fato, estão repartidas camisetas. Algumas imagens incorretas ou é ambíguas (por exemplo, coluna 1, linha 1; ou coluna 3, linha 2) podem ser identificadas e removidas facilmente:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/bing_search_striped.jpg" alt="alt text" width="600"/>
</p>

Para gerar um conjunto de dados grandes e diverso, devem ser usadas várias consultas. Por exemplo, 7\*3 = 21 consultas podem ser sintetizadas automaticamente com todas as combinações de itens de vestuário {blouse, hoodie, pullover, sweater, camisola, camiseta, vest} e {leopard repartido, pontilhada,} de atributos. Baixar as imagens de 50 principais por consulta, em seguida, poderia levar a um máximo de 50 * 21 = 1050 imagens.

Em vez de transferir manualmente imagens a partir de pesquisa de imagens Bing, é muito mais fácil usar em vez disso, o [cognitivos API de pesquisa de imagem do serviços Bing](https://www.microsoft.com/cognitive-services/bing-image-search-api) que retorna um conjunto de URLs de imagens, dada uma cadeia de consulta.

Alguns das imagens baixadas são exatos ou próximo duplicados (por exemplo, diferem apenas por artefactos de resolução ou jpg de imagem). Estes duplicados devem ser removidos para que a divisão de preparação e teste não contêm as imagens do mesmo. Remover duplicadas imagens pode ser obtido usando uma abordagem baseada em hash, que funciona em dois passos: (i) em primeiro lugar, a cadeia de hash é calculada para todas as imagens; (i) numa segunda passagem sobre as imagens, apenas as imagens são mantidas com uma cadeia de hash que ainda não foi encontrada. Todas as outras imagens são eliminadas. Foi encontrado o `dhash` abordagem na biblioteca de Python `imagehash` e descrito neste [blog](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html) para executar bem, com o parâmetro `hash_size` definido como 16. Há problema incorretamente remover algumas imagens não duplicado, desde que a maioria dos duplicados real são removidas.





## <a name="conclusion"></a>Conclusão

Alguns destaques principais deste exemplo são:
- Código para preparar, avaliar e implementar modelos de classificação de imagens.
- Imagens de demonstração fornecidas, mas facilmente adaptáveis (mudança de uma linha) para utilizar a imagem próprio conjunto de dados.
- Recursos de especialistas topo de gama implementados a criar modelos de alta precisão, com base na aprendizagem de transferência.
- Desenvolvimento de modelo interativa com o Azure Machine Learning Workbench e o bloco de notas do Jupyter.


## <a name="references"></a>Referências

[1] Alex Krizhevsky e Geoffrey E. Hinton, Ilya Sutskever [ _ImageNet classificação com redes Neurais Convolucionais aprofundada_](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf). NIPS 2012.  
[2] Kaiming He, Xiangyu Zhang, Shaoqing Ren e Jian Sun, [ _aprofundada residuais de aprendizagem do reconhecimento de imagens_](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf). CVPR 2016.
