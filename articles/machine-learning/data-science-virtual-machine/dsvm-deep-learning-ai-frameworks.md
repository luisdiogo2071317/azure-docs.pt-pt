---
title: Learning profunda e AI estruturas - Azure | Microsoft Docs
description: Arquiteturas de Aprendizagem Profunda e AI
keywords: ferramentas de ciência de dados, máquina de virtual de ciência de dados, ferramentas para ciência de dados, ciência de dados do linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: cd8579fe29282f3875ecfddf67a34444e3de9ef5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="deep-learning-and-ai-frameworks"></a>Arquiteturas de Aprendizagem Profunda e AI
O [máquinas de virtuais de ciência de dados](http://aka.ms/dsvm) (DSVM) e o [profunda VM Learning](http://aka.ms/dsvm/deeplearning) suporta um número de estruturas de aprendizagem avançada para o ajudar a criar aplicações Artificial Intelligence (AI) com a Análise Preditiva e capacidades cognitivos como imagem e a compreensão de idiomas. 

Seguem-se os detalhes em todos os avançada disponíveis no DSVM estruturas de aprendizagem.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Windows, Linux     |
| Como é mesmo configurado / instalado o DSVM?  | Microsoft cognitivos Toolkit (CNTK) está instalado no Python 3.5 [Linux e o Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e Python 3.6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).   |
| Ligações para amostras      | Notas do Jupyter de exemplo são incluídas.     |
| Ferramentas relacionadas no DSVM      | Keras      |
| Como utilizar / executá-lo?    | * Num terminal: Ativar o ambiente correto, em seguida, execute o Python. <br/>
 * No Jupyter: Ligar ao [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório CNTK para exemplos. |

## <a name="tensorflow"></a>TensorFlow

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Windows, Linux     |
| Como é mesmo configurado / instalado o DSVM?  | TensorFlow está instalado no Python 3.5 [Linux e o Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e Python 3.6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition).  |
| Ligações para amostras      | Notas do Jupyter de exemplo são incluídas.     |
| Ferramentas relacionadas no DSVM      | Keras      |
| Como utilizar / executá-lo?    | * Num terminal: Ativar o ambiente correto, em seguida, execute o Python. <br/>
 * No Jupyter: Ligar ao [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório de TensorFlow para exemplos.  |

## <a name="horovod"></a>Horovod

|    |           |
| ------------- | ------------- |
| O que é?   | Estrutura de aprendizagem profunda Distribued para TensorFlow      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | Horovod está instalado no Python 3.5 [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Ligações para amostras      | [https://github.com/uber/horovod/tree/master/examples](https://github.com/uber/horovod/tree/master/examples)     |
| Ferramentas relacionadas no DSVM      | TensorFlow      |
| Como utilizar / executá-lo?    | Cada um terminal: Ativar o ambiente correto, em seguida, execute o Python. |

## <a name="keras"></a>Keras

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Windows, Linux     |
| Como é mesmo configurado / instalado o DSVM?  | TensorFlow está instalado no Python 3.5 [Linux e o Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e Python 3.6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). |
| Ligações para amostras      | https://github.com/fchollet/keras/tree/master/examples      |
| Ferramentas relacionadas no DSVM      | Microsoft cognitivos Toolkit, TensorFlow, Theano      |
| Como utilizar / executá-lo?    | * Num terminal: Ativar o ambiente correto, em seguida, execute o Python. <br/>
 * No Jupyter: Transfira os exemplos da localização do Github, ligue ao [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório de exemplo. |

## <a name="caffe"></a>Caffe

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | Caffe está instalado no `/opt/caffe`.    |
| Como mudar para o Python 2.7 | Execute `source activate root` |
| Ligações para amostras      | Amostras incluídas no `/opt/caffe/examples`.      |
| Ferramentas relacionadas no DSVM      | Caffe2      |
### <a name="how-to-use--run-it"></a>Como utilizar / executá-lo?  

Utilize X2Go para iniciar sessão na sua VM, em seguida, inicie um novo terminal e introduza

```
cd /opt/caffe/examples
source activate root
jupyter notebook
```

Abre uma nova janela do browser com blocos de notas do exemplo.

Binários são instalados na /opt/caffe/build/install/bin.

Versão instalada do Caffe requer o Python 2.7 e não funcionará com o Python 3.5 ativado por predefinição. Executar `source activate root` mudar ambiente Anaconda. 

## <a name="caffe2"></a>Caffe2

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | Caffe2 está instalado o [Python 2.7 (root) conda ambiente](dsvm-languages.md#python-linux-and-windows-server-2012-edition). A origem está a ser `/opt/caffe2`. |
| Ligações para amostras      | Notas de exemplo são incluídas no JupyterHub. |
| Ferramentas relacionadas no DSVM      | Caffe      |
| Como utilizar / executá-lo?    | * No terminal: Ativar o [ambiente do Python raiz](dsvm-languages.md#python-linux-and-windows-server-2012-edition), iniciar Python e importar caffe2. <br/> * Em JupyterHub: [ligar ao JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, navegue para o diretório de Caffe2 para localizar os blocos de notas do exemplo. Algumas notas requerem raiz Caffe2 ser definida no código Python; Introduza /opt/caffe2. |
| Criar notas | Caffe2 é criada a partir da origem no Linux e inclui CUDA, cuDNN e Intel MKL. A consolidação atual é 0d9c0d48c6f20143d6404b99cc568efd29d5a4be, que foi escolhido para estabilidade em todos os GPUs e amostras testadas. |

## <a name="chainer"></a>Chainer

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Windows, Linux     |
| Como é mesmo configurado / instalado o DSVM?  | Chainer está instalado no [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL e ChainerCV também são instalados.   |
| Ligações para amostras      | Notas de exemplo são incluídas no JupyterHub. |
| Ferramentas relacionadas no DSVM      | Caffe      |
| Como utilizar / executá-lo?  | * No terminal: Ativar o [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ambiente, executar _python_, em seguida, importar Chainer. <br/>
* Em JupyterHub: [ligar ao JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, navegue para o diretório de Chainer para localizar os blocos de notas do exemplo.


## <a name="deep-water"></a>Máximo de profundo

|    |           |
| ------------- | ------------- |
| O que é?   | Arquitetura de aprendizagem profunda de H2O      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | Máximo de profundo está instalado no [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e também está disponível no `/dsvm/tools/deep_water`.   |
| Ligações para amostras      | Notas de exemplo são incluídas no JupyterHub.      |
| Ferramentas relacionadas no DSVM      | H2O, Sparkling máximo      |

### <a name="how-to-use--run-it"></a>Como utilizar / executá-lo?  

Máximo de profundo requer CUDA 8 com cuDNN 5.1. Este não está no caminho de biblioteca por predefinição, como outras estruturas de aprendizagem profunda utilizam CUDA 9 e cuDNN 7. Para utilizar CUDA 8 + cuDNN 5.1 profunda máximo:

```
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
export CUDA_ROOT=/usr/local/cuda-8.0
```

Para utilizar profunda máximo:
* No terminal: Ativar o [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) ambiente, em seguida, execute _python_. <br/>
* Em JupyterHub: [ligar ao JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, navegue para o diretório de deep_water para localizar os blocos de notas do exemplo.

## <a name="mxnet"></a>MXNet

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Windows, Linux     |
| Como é mesmo configurado / instalado o DSVM?  | MXNet está instalado no `C:\dsvm\tools\mxnet` no Windows e `/dsvm/tools/mxnet` no Linux. Enlaces de Python estão instalados no Python 3.5 no [Linux e o Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e Python 3.6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). Enlaces de R também estão instaladas no Ubuntu.   |
| Ligações para amostras      | Notas do Jupyter de exemplo são incluídas.    |
| Ferramentas relacionadas no DSVM      | Keras      |
| Como utilizar / executá-lo?    | * Num terminal: Ativar o ambiente correto, em seguida, execute o Python. <br/>
 * No Jupyter: Ligar ao [Jupyter](provision-vm.md#tools-installed-on-the-microsoft-data-science-virtual-machine) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório de mxnet para exemplos.  |
 | Criar notas | MXNet é criada a partir da origem no Linux. Este compilação inclui CUDA, cuDNN, NCCL e MKL. |

## <a name="nvidia-digits"></a>NVIDIA DÍGITOS

|    |           |
| ------------- | ------------- |
| O que é?   | Ligação avançada de formação rapidamente modelos learning profunda aprendizagem sistema a partir de NVIDIA      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | DÍGITOS está instalado no `/dsvm/tools/DIGITS` e está disponível um serviço chamado _dígitos_.   |
### <a name="how-to-use--run-it"></a>Como utilizar / executá-lo?  

Inicie sessão na VM com o X2Go. Cada um terminal, inicie o serviço:

    sudo systemctl start digits

O serviço demora cerca de um minuto para iniciar. Iniciar um browser e navegue para `http://localhost:5000`.



## <a name="nvdia-smi"></a>nvdia-smi

|    |           |
| ------------- | ------------- |
| O que é?   | Ferramenta NVIDIA para consultar a atividade GPU      |
| Edições de DSVM suportados      | Windows, Linux     |
| Como é mesmo configurado / instalado o DSVM?  | _NVIDIA smi_ está disponível no caminho do sistema.   |
| Como utilizar / executá-lo? | Inicie uma linha de comandos (no Windows) ou um terminal (no Linux), em seguida, execute _nvidia smi_.



## <a name="theano"></a>Theano

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | Theano está instalado no Python 2.7 (_raiz_), bem como o Python 3.5 (_py35_) ambiente.   |
| Ferramentas relacionadas no DSVM      | Keras      |
| Como utilizar / executá-lo?    | * Num terminal, ative a versão do Python que pretende (raiz ou py35), execute o python e importar theano. <br/> 
* No Jupyter, selecione o Python 2.7 ou 3.5 kernel, em seguida, importar theano.  
<br/>
Para contornar um erro MKL recente, tem de definir primeiro MKL threading layer:<br/><br/>
_Exportar MKL_THREADING_LAYER = GNU_
|



## <a name="torch"></a>Torch

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | Torch está instalado no `/dsvm/tools/torch`. PyTorch está instalado no Python 2.7 (_raiz_), bem como o Python 3.5 (_py35_) ambiente.   |
| Ligações para amostras      | Exemplos de torch estão localizados em `/dsvm/samples/torch`. Exemplos de PyTorch estão localizados em `/dsvm/samples/pytorch`.      |


## <a name="pytorch"></a>PyTorch

|    |           |
| ------------- | ------------- |
| O que é?   | Learning profunda framework      |
| Edições de DSVM suportados      | Linux     |
| Como é mesmo configurado / instalado o DSVM?  | PyTorch está instalado no [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  |
| Ligações para amostras      | Blocos de notas do Jupyter exemplo estão incluídos e amostras também podem ser encontradas na /dsvm/samples/pytorch.      |
| Ferramentas relacionadas no DSVM      | Torch      |
| Como utilizar / executá-la | 
* Cada um terminal: Ativar o ambiente correto, em seguida, execute o Python. <br/>
 * No Jupyter: Ligar ao [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório de PyTorch para exemplos.  |

## <a name="mxnet-model-server"></a>Servidor MXNet modelo

|    |           |
| ------------- | ------------- |
| O que é?   | Um servidor para criar pontos finais HTTP para modelos MXNet e ONNX      |
| Edições de DSVM suportados      | Linux     |
| Como é mesmo configurado / instalado o DSVM?  | _servidor de modelo mxnet_ está disponível no terminal.   |
| Ligações para amostras      | Procure exemplos atualizados [página MXNet modelo servidor](https://github.com/awslabs/mxnet-model-server).    |
| Ferramentas relacionadas no DSVM      | MXNet      |

## <a name="tensorflow-serving"></a>Servir TensorFlow

|    |           |
| ------------- | ------------- |
| O que é?   | Um servidor para executar inferencing um modelo de TensorFlow      |
| Edições de DSVM suportados      | Linux     |
| Como é mesmo configurado / instalado o DSVM?  | _tensorflow_model_server_ está disponível no terminal.   |
| Ligações para amostras      | Amostras estão disponíveis [online](https://www.tensorflow.org/serving/).      |
| Ferramentas relacionadas no DSVM      | TensorFlow      |

## <a name="tensorrt"></a>TensorRT

|    |           |
| ------------- | ------------- |
| O que é?   | Uma profunda servidor inferência NVIDIA de aprendizagem. |
| Edições de DSVM suportados      | Ubuntu     |
| Como é mesmo configurado / instalado o DSVM?  | TensorRT é instalado como um _apt_ pacote.   |
| Ligações para amostras      | Amostras estão disponíveis [online](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).      |
| Ferramentas relacionadas no DSVM      | TensorFlow servir, servidor MXNet modelo  |



