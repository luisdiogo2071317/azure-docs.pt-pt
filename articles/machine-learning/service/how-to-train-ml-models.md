---
title: Formar modelos do ML com estimadores
titleSuffix: Azure Machine Learning service
description: Saiba como realizar treinamento de nó único e distribuído da máquina tradicional de aprendizagem e a aprendizagem profunda modelos com a classe de estimador de serviços do Azure Machine Learning
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 005854a51916d36bbad56f1296f17fa687020359
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251406"
---
# <a name="train-models-with-azure-machine-learning"></a>Utilizar modelos com o Azure Machine Learning

Treinamento modelos de machine learning, redes neurais profundas particularmente, muitas vezes, é uma tarefa de tempo e computação intensivas. Depois de terminar de escrever o script de treinamento e em execução num pequeno subconjunto de dados no seu computador local, provavelmente irá querer aumentar verticalmente a carga de trabalho.

Para facilitar a treinamento, o SDK de Python do Azure Machine Learning fornece uma abstração de alto nível, a classe de calculadora, que permite aos utilizadores a criar facilmente seus modelos no ecossistema do Azure. Pode criar e utilizar um [ `Estimator` objeto](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) para submeter a qualquer código de treinamento que pretende executar na computação remota, seja um treinamento de execução ou distribuído de nó único num cluster GPU. Para trabalhos PyTorch e TensorFlow, Azure Machine Learning também fornece personalizado respectivo `PyTorch` e `TensorFlow` estimadores para simplificar o uso dessas estruturas.

## <a name="train-with-an-estimator"></a>Preparar com um avaliador que esteja

Depois de criar a sua [área de trabalho](concept-azure-machine-learning-architecture.md#workspace) e configure a sua [ambiente de desenvolvimento](how-to-configure-environment.md), preparar um modelo no Azure Machine Learning envolve os seguintes passos:  
1. Criar um [destino de computação remota](how-to-set-up-training-targets.md)
2. Carregar seu [dados de treinamento](how-to-access-data.md) (opcional)
3. Criar sua [script de treinamento](tutorial-train-models-with-aml.md#create-a-training-script)
4. Criar um `Estimator` objeto
5. Submeter a tarefa de treinamento

Este artigo se concentra nas etapas 4 e 5. Para obter passos 1 a 3, veja a [treinar um tutorial de modelo](tutorial-train-models-with-aml.md) para obter um exemplo.

### <a name="single-node-training"></a>Treinamento de nó único

Utilize um `Estimator` para um treinamento de nó único executar no cálculo remoto no Azure para um scikit-saiba modelo. Deverá ter já criado seu [destino de computação](how-to-set-up-training-targets.md#amlcompute) objeto `compute_target` e a sua [arquivo de dados](how-to-access-data.md) objeto `ds`.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Este fragmento de código especifica os parâmetros seguintes para o `Estimator` construtor.

Parâmetro | Descrição
--|--
`source_directory`| Diretório de local que contém todos os seus códigos necessários para a tarefa de preparação. Esta pasta é copiada a partir do seu computador local para a computação remota 
`script_params`| Especificar os argumentos da linha de comandos para o script de treinamento de dicionário `entry_script`, na forma de < argumento da linha de comandos, valor > pares
`compute_target`| Destino de computação remota que o script de treinamento serão executados no, neste caso uma computação do Azure Machine Learning ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) cluster
`entry_script`| Caminho do ficheiro (relativa a `source_directory`) do script de treinamento para ser executado na computação remota. Este ficheiro e todos os arquivos adicionais depende, devem ser localizados na pasta
`conda_packages`| Lista de pacotes de Python a serem instalados por meio de conda necessário ao seu script de treinamento.  
O construtor tem outro parâmetro chamado `pip_packages` que utilizar quaisquer pacotes de pip necessário

Agora que criou sua `Estimator` objeto, submeter a tarefa de preparação para ser executado na computação remota com uma chamada para o `submit` funcionar no seu [experimentação](concept-azure-machine-learning-architecture.md#experiment) objeto `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Pastas especiais** duas pastas, *produz* e *registos*, receber tratamento especial do Azure Machine Learning. Durante o treinamento, quando gravar arquivos em pastas denominadas *produz* e *registos* que são relativos ao diretório raiz (`./outputs` e `./logs`, respectivamente), os ficheiros serão automaticamente carregar para o seu histórico de execução para que tenha acesso aos mesmos, uma vez concluída a execução.
>
> Para criar artefactos durante o treinamento (por exemplo, ficheiros de modelo, os pontos de verificação, arquivos de dados ou imagens plotadas) para escrever o `./outputs` pasta.
>
> Da mesma forma, escreve quaisquer registos do seu treinamento executar para o `./logs` pasta. Para utilizar o Azure Machine Learning [TensorBoard integração](https://aka.ms/aml-notebook-tb) certificar-se de que escreve os registos de TensorBoard para esta pasta. Embora a execução está em curso, poderá iniciar TensorBoard e transmitir em fluxo estes registos.  Mais tarde, também poderá restaurar os registos a partir de qualquer uma das suas execuções anteriores.
>
> Por exemplo, para transferir um ficheiro escrito para o *produz* pasta no seu computador local após seu treinamento remoto, execute: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Formação distribuída e imagens personalizadas do Docker

Existem dois cenários de formação adicionais que podem ser executadas com o `Estimator`:
* Com uma imagem personalizada do Docker
* Formação distribuída num cluster com vários nó

O código a seguir mostra como realizar treinamento distribuído para um modelo do CNTK. Além disso, em vez de usar as imagens do Azure Machine Learning predefinidas, presume que está a utilizar sua própria imagem de docker personalizado para treinamento.

Deverá ter já criado seu [destino de computação](how-to-set-up-training-targets.md#amlcompute) objeto `compute_target`. Criar o avaliador da seguinte forma:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-cntk-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      pip_packages=['cntk==2.5.1'],
                      custom_docker_base_image='microsoft/mmlspark:0.12')
```

O código acima expõe os seguintes novos parâmetros para o `Estimator` construtor:

Parâmetro | Descrição | Predefinição
--|--|--
`custom_docker_base_image`| Nome da imagem que pretende utilizar. Forneça apenas as imagens disponíveis nos repositórios de docker público (no caso este Docker Hub). Para utilizar uma imagem a partir de um repositório de docker privado, utilize o construtor `environment_definition` parâmetro em vez disso | `None`
`node_count`| Número de nós a utilizar para a tarefa de preparação. | `1`
`process_count_per_node`| Número de processos (ou "funcionários") para executar em cada nó. Neste caso, utilize o `2` GPUs disponíveis em cada nó.| `1`
`distributed_backend`| Back-end para iniciar distribuído treinamento, o que o avaliador de oferece por meio de MPI.  Para a realização do treinamento paralelo ou distribuído (por exemplo, `node_count`> 1 ou `process_count_per_node`> 1 ou ambos), defina `distributed_backend='mpi'`. É a implementação de MPI usada pelo AML [MPI aberto](https://www.open-mpi.org/).| `None`

Por fim, submeta a tarefa de preparação:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Exemplos
Para um bloco de notas que prepara um modelo de sklearn, consulte:
* [tutoriais/img-classificação-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Para blocos de notas na aprendizagem profunda distribuída, consulte:
* [How-to-use-azureml/Training-with-Deep-Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Executar a métrica durante o treinamento do Roteiro](how-to-track-experiments.md)
* [Utilizar modelos de PyTorch](how-to-train-pytorch.md)
* [Utilizar modelos do TensorFlow](how-to-train-tensorflow.md)
* [Otimizar hiperparâmetros](how-to-tune-hyperparameters.md)
* [Implementar um modelo preparado](how-to-deploy-and-where.md)
