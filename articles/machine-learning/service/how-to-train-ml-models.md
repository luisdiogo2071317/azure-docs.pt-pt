---
title: Treine modelos de aprendizagem automática com o Azure Machine Learning
description: Saiba como realizar treinamento de nó único e distribuído de tradicional aprendizagem automática e modelos com o Azure Machine Learning services de aprendizagem profunda
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 50b808b5769f2160d765ecdb431d71856e651b33
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983587"
---
# <a name="how-to-train-models-with-azure-machine-learning"></a>Como criar modelos com o Azure Machine Learning

Treinamento modelos de machine learning, redes neurais profundas particularmente, muitas vezes, é uma tarefa de tempo e computação intensivas. Depois de terminar de escrever o script de treinamento e em execução num pequeno subconjunto de dados no seu computador local, provavelmente irá querer aumentar verticalmente a carga de trabalho.

Para facilitar a treinamento, o SDK de Python do Azure Machine Learning fornece uma abstração de alto nível, a classe de calculadora, que permite aos utilizadores a criar facilmente seus modelos no ecossistema do Azure. Pode criar e utilizar um objeto de Calculadora para submeter a qualquer código de treinamento que pretende executar na computação remota, se se trata de um treinamento de execução ou distribuído de nó único num cluster GPU. Para trabalhos PyTorch e TensorFlow, Azure Machine Learning também fornece respectivo PyTorch personalizado e TensorFlow avaliadores que facilitam a utilizar estas estruturas.

## <a name="train-with-an-estimator"></a>Preparar com um avaliador que esteja

Depois de criar a sua [área de trabalho](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace) e configure a sua [ambiente de desenvolvimento](how-to-configure-environment.md), preparar um modelo no Azure Machine Learning envolve os seguintes passos:  
1. [Criar um destino de computação remota](how-to-set-up-training-targets.md)
2. [Carregar os dados de treinamento](how-to-access-data.md) (opcional)
3. Criar o script de treinamento
4. Criar um objeto de Calculadora
5. Submeter a tarefa de treinamento

Este artigo se concentra nas etapas 4 e 5. Para obter passos 1 a 3, veja a isso [tutorial](tutorial-train-models-with-aml.md) para obter um exemplo.

### <a name="single-node-training"></a>Treinamento de nó único

O código a seguir explica um treinamento de nó único executar no cálculo remoto no Azure para um scikit-saiba modelo. Deverá ter já criado seu [destino de computação](how-to-set-up-training-targets.md#batch) objeto `compute_target` e a sua [arquivo de dados](how-to-access-data.md) objeto `ds`.

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

O trecho de código acima Especifica os parâmetros seguintes para o construtor de Calculadora:
* `source_directory`: O diretório de local que contém todos os seus códigos necessários para a tarefa de preparação. Esta pasta é copiada a partir do seu computador local para a computação remota 
* `script_params`: Um dicionário especificando os argumentos da linha de comandos para o script de treinamento `entry_script`, na forma de < argumento da linha de comandos, valor > pares
* `compute_target`: A computação remota que o script de treinamento serão executados nesse caso, um [Batch AI](how-to-set-up-training-targets.md#batch) cluster
* `entry_script`: O caminho do ficheiro (relativa a `source_directory`) do script de treinamento para ser executado na computação remota. Este ficheiro e todos os arquivos adicionais depende, devem ser localizados na pasta
* `conda_packages`: A lista de pacotes de Python a serem instalados por meio de conda necessário ao seu script de treinamento.  
O construtor tem outro parâmetro chamado `pip_packages` que pode utilizar quaisquer pacotes de pip necessário

Agora que criou seu objeto de calculadora, pode submeter a tarefa de preparação para ser executado na computação remota através de uma chamada para o `submit` funcionar no seu [experimentação](concept-azure-machine-learning-architecture.md#experiment) objeto `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Pastas especiais** duas pastas, *produz* e *registos*, receber tratamento especial do Azure Machine Learning. Durante o treinamento, se escrever ficheiros de pastas denominadas *produz* e *registos* que são relativos ao diretório raiz (`./outputs` e `./logs`, respectivamente), irão obter estes ficheiros carregados automaticamente para o seu histórico de execução para que terá acesso a eles vez concluída a execução. 
>
> Para acessar os artefactos criados durante o treinamento (por exemplo, ficheiros de modelo, os pontos de verificação, arquivos de dados ou imagens plotadas) escrever para o `./outputs` pasta.
>
> Da mesma forma, pode escrever quaisquer registos do seu treinamento executar para o `./logs` pasta. Para utilizar o Azure Machine Learning [TensorBoard integração](https://aka.ms/aml-notebook-tb) certificar-se de que escreve os registos de TensorBoard para esta pasta. Embora a execução está em curso, poderá iniciar TensorBoard e transmitir em fluxo estes registos.  Mais tarde, também poderá restaurar os registos a partir de qualquer uma das suas execuções anteriores.
>
> Por exemplo, para transferir um ficheiro escrito para o *produz* pasta no seu computador local após seu treinamento remoto, execute: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Formação distribuída e imagens personalizadas do Docker

Existem dois cenários de formação adicionais que podem ser executadas com o avaliador de:
1. Com uma imagem personalizada do Docker
2. Formação distribuída num cluster com vários nó

O código a seguir mostra como realizar treinamento distribuído para um modelo do CNTK. Além disso, em vez de usar as imagens do Azure Machine Learning padrão, ele pressupõe que tem sua própria imagem de docker personalizado que pretende utilizar para formação.

Deverá ter já criado seu [destino de computação](how-to-set-up-training-targets.md#batch) objeto `compute_target`. É possível criar o avaliador da seguinte forma:

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

O código acima expõe os seguintes novos parâmetros para o construtor de Calculadora:
* `custom_docker_base_image`: O nome da imagem que pretende utilizar. Só é possível fornecer imagens disponíveis nos repositórios de docker público (no caso este Docker Hub). Para utilizar uma imagem a partir de um repositório de docker privado, utilize o construtor `environment_definition` parâmetro em vez disso
* `node_count`: O número de nós a utilizar para a tarefa de preparação. Este argumento assume a predefinição `1`
* `process_count_per_node`: O número de processos (ou "funcionários") para executar em cada nó. Neste caso, utilize o `2` GPUs disponíveis em cada nó. Este argumento assume a predefinição `1`
* `distributed_backend`: O back-end para iniciar distribuído treinamento, o que o avaliador de oferece por meio de MPI. Este argumento é predefinido para `None`. Se deseja realizar treinamento paralelo ou distribuído (por exemplo, `node_count`> 1 ou `process_count_per_node`> 1 ou ambos), defina `distributed_backend='mpi'`. É a implementação de MPI usada pelo AML [MPI aberto](https://www.open-mpi.org/).

Por fim, submeta a tarefa de preparação:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Exemplos
Para um tutorial sobre como preparar um modelo de sklearn, consulte:
* `tutorials/01.train-models.ipynb`

Para um tutorial sobre CNTK distribuído com o docker personalizado, consulte:
* `training/06.distributed-cntk-with-custom-docker/06.distributed-cntk-with-custom-docker.ipynb`

Obtenha estes blocos de notas:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Executar a métrica durante o treinamento do Roteiro](how-to-track-experiments.md)
* [Utilizar modelos de PyTorch](how-to-train-pytorch.md)
* [Utilizar modelos do TensorFlow](how-to-train-tensorflow.md)
* [Otimizar hiperparâmetros](how-to-tune-hyperparameters.md)
* [Implementar um modelo preparado](how-to-deploy-and-where.md)
