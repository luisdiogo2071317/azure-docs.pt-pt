---
title: Tutorial - Preparação distribuída com o Azure Batch AI e o Horovod | Microsoft Docs
description: Tutorial - Como preparar um modelo distribuído com o Horovod através do serviço Azure Batch AI e da CLI do Azure.
services: batch-ai
author: johnwu10
manager: jeconnoc
ms.service: batch-ai
ms.topic: tutorial
ms.date: 09/03/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 45255845d8645391ee33471830ac2ef27870a40d
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408634"
---
# <a name="tutorial-train-a-distributed-model-with-horovod"></a>Tutorial: Preparar um modelo distribuído com Horovod

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Neste tutorial, vai preparar um modelo de aprendizagem aprofundada distribuído ao executá-lo em paralelo em vários nós num cluster do Batch AI. O Batch AI é um serviço gerido para preparação de modelos de machine learning e IA em escala em clusters de GPUs do Azure. 

Este tutorial apresenta um fluxo de trabalho do Batch AI comum e descreve como interagir com os recursos do Batch AI através da CLI do Azure. Os tópicos abrangidos incluem:

> [!div class="checklist"]
> * Configurar uma área de trabalho, uma experimentação e um cluster do Batch AI
> * Configurar uma partilha de ficheiros do Azure para entrada e saída
> * Paralelizar um modelo de aprendizagem aprofundada com o Horovod
> * Submeter uma tarefa de preparação
> * Monitorizar a tarefa
> * Obter os resultados de preparação

Para este tutorial, um modelo de deteção de objetos é modificado para ser executado em paralelo com o [Horovod](https://github.com/uber/horovod). O modelo é preparado no [conjunto de dados CIFAR 10](https://www.cs.toronto.edu/~kriz/cifar.html) de imagens. A tarefa de preparação é executada num cluster que contém 24 vCPUs e 4 GPUs, e demora cerca de 60 minutos a concluir.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer que execute uma versão da CLI do Azure que seja a 2.0.38 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

## <a name="why-use-horovod"></a>Por quê utilizar o Horovod?

O Horovod é uma arquitetura de preparação distribuída para o Tensorflow, o Keras e o PyTorch, e é utilizado neste tutorial. Com o Horovod, pode converter um script de preparação criado para ser executado num único GPU para que seja executado com eficiência num sistema distribuído com apenas algumas linhas de código.

Além do Horovod, o Batch AI suporta preparação distribuída com várias outras arquiteturas de código aberto populares. Certifique-se de que revê os termos de licenciamento de qualquer arquitetura que utilizar para preparar modelos em produção.

## <a name="prepare-the-batch-ai-environment"></a>Preparar o ambiente do Batch AI

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Utilize o comando `az group create` para criar um grupo de recursos com o nome `batchai.horovod` na região `eastus`. Utilize o grupo de recursos para implementar recursos do Batch AI.

```azurecli-interactive
az group create --name batchai.horovod --location eastus
```

### <a name="create-a-workspace"></a>Criar uma área de trabalho

Crie uma área de trabalho do Batch AI com o comando `az batchai workspace create`. Uma área de trabalho é uma coleção de nível superior de outros recursos do Batch AI. O comando seguinte cria uma área de trabalho chamada `batchaidev` no grupo de recursos.

```azurecli-interactive
az batchai workspace create --resource-group batchai.horovod --workspace batchaidev 
```

### <a name="create-an-experiment"></a>Criar uma experimentação

Uma experimentação do Batch AI agrupa uma ou mais tarefas que consulta e gere em conjunto. O comando `az batchai experiment create` seguinte cria uma experimentação chamada `cifar` na área de trabalho e no grupo de recursos.

```azurecli-interactive
az batchai experiment create --resource-group batchai.horovod --workspace batchaidev --name cifar 
```

## <a name="set-up-a-gpu-cluster"></a>Configurar um cluster de GPU

Em seguida, vai configurar um cluster de GPU para executar a experimentação. O Batch AI fornece várias opções flexíveis para personalizar clusters para necessidades específicas.

O comando `az batchai cluster create` seguinte cria um cluster de quatro nós chamado `nc6cluster` na área de trabalho e no grupo de recursos. Por predefinição, as VMs no cluster executam uma imagem do Ubuntu Server concebida para alojar aplicações baseadas em contentores. Os nós de cluster neste exemplo utilizam o tamanho `Standard_NC6`, que contém um GPU NVIDIA Tesla K80.

```azurecli-interactive
az batchai cluster create --resource-group batchai.horovod --workspace batchaidev --name nc6cluster --vm-priority dedicated  --vm-size Standard_NC6 --target 4 --generate-ssh-keys
```

Execute o comando `az batchai cluster show` para ver o estado do cluster. Normalmente, demora alguns minutos a aprovisionar completamente o cluster.

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --output table
```

No início da criação do cluster, este está no estado `resizing`. Continue os passos seguintes enquanto o estado do cluster muda. O cluster está pronto para executar a tarefa de preparação quando o estado for `steady` e os nós estiverem `idle`. Por exemplo:

```
Name        Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
----------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
nc6cluster  batchai.horovod  batchaidev   STANDARD_NC6  steady        4          0            0          0           0
```

## <a name="set-up-storage"></a>Configurar o armazenamento

Utilize o comando `az storage account create` para criar uma conta de armazenamento para armazenar o script e a saída da preparação.

```azurecli-interactive
az storage account create --resource-group batchai.horovod --name mystorageaccount --location eastus --sku Standard_LRS
```

Crie uma partilha de ficheiros do Azure chamada `myshare` na conta com o comando `az storage share create`:

```azurecli-interactive
az storage share create --name myshare --account-name mystorageaccount
```

Na prática, este mesmo armazenamento pode ser utilizado em várias tarefas e experimentações. Para manter tudo organizado, crie um diretório na partilha de ficheiros para armazenar os ficheiros relacionados com esta experimentação específica. O comando `az storage directory create` seguinte cria um diretório chamado `cifar`.

```azurecli-interactive
az storage directory create --name cifar --share-name myshare --account-name mystorageaccount
```

O próximo passo é preparar o script de preparação real, que, em seguida, vai carregar para o diretório criado recentemente.

## <a name="create-the-training-script"></a>Criar o script de preparação

Nesta experimentação, vai executar um script Python atualizado com algumas alterações para executar um modelo de deteção de objetos em paralelo com o Horovod. O [modelo original](https://raw.githubusercontent.com/keras-team/keras/master/examples/cifar10_cnn.py) utiliza o Keras com um back-end do TensorFlow. 

Num diretório de trabalho na sua shell, utilize o seu editor de texto favorito para criar um ficheiro chamado `cifar_cnn_distributed.py` com o seguinte conteúdo. As alterações ao código fonte original são comentadas com um prefixo `HOROVOD`.

```python
from __future__ import print_function
import keras
from keras.datasets import cifar10
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation, Flatten
from keras.layers import Conv2D, MaxPooling2D
import tensorflow as tf
import horovod.keras as hvd
import os
from keras import backend as K
import math
import argparse 

# HOROVOD: initialize Horovod.
hvd.init()

# HOROVOD: pin GPU to be used to process local rank (one GPU per process)
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
config.gpu_options.visible_device_list = str(hvd.local_rank())
K.set_session(tf.Session(config=config))

batch_size = 32
num_classes = 10
# HOROVOD: adjust number of epochs based on number of GPUs.
epochs = int(math.ceil(100.0 / hvd.size()))

data_augmentation = True
num_predictions = 20
# BATCH AI: change save directory to mounted storage path
parser = argparse.ArgumentParser()
parser.add_argument("-d", "--dir", help="directory to save model to")
args = parser.parse_args()
save_dir = os.path.join(args.dir, 'saved_models')
model_name = 'keras_cifar10_trained_model.h5'

# The data, split between train and test sets:
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
print('x_train shape:', x_train.shape)
print(x_train.shape[0], 'train samples')
print(x_test.shape[0], 'test samples')

# Convert class vectors to binary class matrices.
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

model = Sequential()
model.add(Conv2D(32, (3, 3), padding='same',
                 input_shape=x_train.shape[1:]))
model.add(Activation('relu'))
model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(64, (3, 3), padding='same'))
model.add(Activation('relu'))
model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Flatten())
model.add(Dense(512))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(num_classes))
model.add(Activation('softmax'))

# HOROVOD: adjust learning rate based on number of GPUs.
opt = keras.optimizers.rmsprop(lr=0.0001 * hvd.size(), decay=1e-6)

# HOROVOD: add Horovod Distributed Optimizer.
opt = hvd.DistributedOptimizer(opt)

# Let's train the model using RMSprop
model.compile(loss='categorical_crossentropy',
              optimizer=opt,
              metrics=['accuracy'])

callbacks = [
    # HOROVOD: broadcast initial variable states from rank 0 to all other processes.
    # This is necessary to ensure consistent initialization of all workers when
    # training is started with random weights or restored from a checkpoint.
    hvd.callbacks.BroadcastGlobalVariablesCallback(0),
]

# HOROVOD: save checkpoints only on worker 0 to prevent other workers from corrupting them.
if hvd.rank() == 0:
    callbacks.append(keras.callbacks.ModelCheckpoint('./checkpoint-{epoch}.h5'))

x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
x_train /= 255
x_test /= 255

if not data_augmentation:
    print('Not using data augmentation.')
    model.fit(x_train, y_train,
              batch_size=batch_size,
              epochs=epochs,
              validation_data=(x_test, y_test),
              shuffle=True)
else:
    print('Using real-time data augmentation.')
    # This will do preprocessing and realtime data augmentation:
    datagen = ImageDataGenerator(
        featurewise_center=False,  # set input mean to 0 over the dataset
        samplewise_center=False,  # set each sample mean to 0
        featurewise_std_normalization=False,  # divide inputs by std of the dataset
        samplewise_std_normalization=False,  # divide each input by its std
        zca_whitening=False,  # apply ZCA whitening
        zca_epsilon=1e-06,  # epsilon for ZCA whitening
        rotation_range=0,  # randomly rotate images in the range (degrees, 0 to 180)
        width_shift_range=0.1,  # randomly shift images horizontally (fraction of total width)
        height_shift_range=0.1,  # randomly shift images vertically (fraction of total height)
        shear_range=0.,  # set range for random shear
        zoom_range=0.,  # set range for random zoom
        channel_shift_range=0.,  # set range for random channel shifts
        fill_mode='nearest',  # set mode for filling points outside the input boundaries
        cval=0.,  # value used for fill_mode = "constant"
        horizontal_flip=True,  # randomly flip images
        vertical_flip=False,  # randomly flip images
        rescale=None,  # set rescaling factor (applied before any other transformation)
        preprocessing_function=None,  # set function that will be applied on each input
        data_format=None,  # image data format, either "channels_first" or "channels_last"
        validation_split=0.0)  # fraction of images reserved for validation (strictly between 0 and 1)

    # Compute quantities required for feature-wise normalization
    # (std, mean, and principal components if ZCA whitening is applied).
    datagen.fit(x_train)

    # Fit the model on the batches generated by datagen.flow().
    model.fit_generator(datagen.flow(x_train, y_train,
                                     batch_size=batch_size),
                        epochs=epochs,
                        validation_data=(x_test, y_test),
                        workers=4)

# Save model and weights
if not os.path.isdir(save_dir):
    os.makedirs(save_dir)
model_path = os.path.join(save_dir, model_name)
model.save(model_path)
print('Saved trained model at %s ' % model_path)

# Score trained model.
scores = model.evaluate(x_test, y_test, verbose=1)
print('Test loss:', scores[0])
print('Test accuracy:', scores[1])
```

Conforme mostrado neste exemplo, apenas são necessárias algumas atualizações ao modelo para permitir a preparação distribuída com a arquitetura Horovod. 

Não se esqueça que este script utiliza um modelo e um conjunto de dados relativamente pequenos para fins de demonstração, pelo que este modelo distribuído não mostrará necessariamente uma melhoria significativa de desempenho. Para ver verdadeiramente o poder da preparação distribuída, utilize um modelo e um conjunto de dados muito maiores.

## <a name="upload-the-training-script"></a>Carregar o script de preparação

Assim que o script estiver pronto, o passo seguinte é carregá-lo para o diretório da partilha de ficheiros que criou anteriormente. O comando `az storage file upload` seguinte carrega-o do diretório de trabalho local para a localização correta.

```azurecli-interactive
az storage file upload --path cifar --share-name myshare --source cifar_cnn_distributed.py --account-name mystorageaccount
```

## <a name="submit-the-training-job"></a>Submeter a tarefa de preparação

Depois de concluir os passos anteriores, crie uma tarefa de preparação. No Batch AI, vai utilizar um ficheiro `job.json` para definir os parâmetros para saber como executar uma tarefa. Com o seu editor de texto favorito, crie um ficheiro de configuração de tarefa chamado `job.json` com o seguinte conteúdo.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 4,
        "horovodSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar/cifar_cnn_distributed.py",
            "commandLineArgs": "--dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "jobPreparation": {
            "commandLine": "apt update; apt install mpi-default-dev mpi-default-bin -y; pip install keras horovod"
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Explicação das propriedades:

| Propriedade | Descrição |
| --------- | --------- |
| `nodeCount` | O número de nós a dedicar à tarefa. Aqui, a tarefa será executada em paralelo em `4` nós. |
| `horovodSettings` | O campo `pythonScriptFilePath` define o caminho para o script do Horovod, localizado no diretório `cifar` criado anteriormente. O campo `commandLineArgs` inclui os argumentos de linha de comandos para executar o script. Para esta experimentação, o diretório a partir do qual será guardado o modelo é o único argumento necessário. `$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare` é o caminho onde a partilha de ficheiros foi montada. | 
| `stdOutErrPathPrefix` | O caminho para armazenar as saídas e os registos da tarefa que, neste exemplo, é o mesmo diretório `cifar`. |
| `jobPreparation` | Quaisquer instruções especiais para preparar o ambiente para execução da tarefa. Este script requer a instalação dos pacotes MPI e Horovod indicados. |
| `containerSettings` | As definições do contentor em que a tarefa é executada. Esta tarefa utiliza um contentor do Docker criado com `tensorflow`.

Com a configuração, crie a tarefa com o comando `az batchai job create`. O comando seguinte coloca uma nova tarefa chamada `cifar_distributed` a utilizar todos os recursos configurados até este ponto. 

```azurecli-interactive
az batchai job create --cluster nc6cluster --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --experiment cifar --config-file job.json --storage-account-name mystorageaccount
```

Se os nós estiverem atualmente ocupados, a tarefa pode demorar algum tempo até começar a ser executada. Utilize o comando `az batchai job show` para ver o estado de execução da tarefa.

```azurecli-interactive
az batchai job show --experiment cifar --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --query "executionState"
```

### <a name="visualize-the-distributed-training"></a>Visualizar a preparação distribuída

Assim que a tarefa começar a ser executada, utilize novamente o comando `az batchai cluster show` para consultar o estado dos nós de cluster. 

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --query "nodeStateCounts"
```

A saída deve ser semelhante à seguinte, que mostra os quatro num estado de execução. Este resultado mostra que os quatro nós estão a ser utilizados atualmente na preparação distribuída.

```
{
  "idleNodeCount": 0,
  "leavingNodeCount": 0,
  "preparingNodeCount": 0,
  "runningNodeCount": 4,
  "unusableNodeCount": 0
}
```

## <a name="monitor-the-job"></a>Monitorizar a tarefa

### <a name="list-output-files"></a>Listar os ficheiros de saída

Durante a execução da tarefa, utilize o comando `az batchai job file list` para listar os ficheiros de saída gerados pela tarefa.

```azurecli-interactive
az batchai job file list --experiment cifar --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev --output table
```

Para esta experimentação específica, a saída deve ser semelhante à seguinte. A saída geral da tarefa está registada em `stdout.txt` enquanto que `stderr.txt` apresenta quaisquer erros ocorridos na execução principal. Os outros ficheiros são registos de saída, erros e preparação da tarefa correspondentes a cada nó individual.

```
Name                                                    Type       Size  Modified
------------------------------------------------------  ------  -------  -------------------------
execution-tvm-676767296_1-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_2-20180718t174802z-p.log        file      15094  2018-07-18T22:41:55+00:00
execution-tvm-676767296_3-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_4-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
stderr-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr.txt                                              file       7653  2018-07-18T22:46:32+00:00
stdout-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout.txt                                              file    2316480  2018-07-18T22:46:32+00:00
```

### <a name="stream-an-output-file"></a>Transmitir um ficheiro de saída

Utilize o comando `az batchai job file stream` para transmitir o conteúdo de um ficheiro. O exemplo seguinte transmite o registo de saída principal.

```azurecli-interactive
az batchai job file stream --experiment cifar --file-name stdout.txt --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev
```

Durante a execução da tarefa, o comando transmite a saída padrão da tarefa de preparação, mostrando uma saída semelhante à seguinte.

```
...
50000 train samples
10000 test samples
Using real-time data augmentation.
Epoch 1/25


   1/1563 [..............................] - ETA: 2:42:25 - loss: 2.3334 - acc: 0.0312   1/1563 [..............................] - ETA: 2:30:42 - loss: 2.2973 - acc: 0.0938
   1/1563 [..............................] - ETA: 30:36 - loss: 2.3175 - acc: 0.1250
   1/1563 [..............................] - ETA: 2:32:58 - loss: 2.3489 - acc: 0.0625
   2/1563 [..............................] - ETA: 1:21:59 - loss: 2.3230 - acc: 0.0625

   2/1563 [..............................]   2/1563 [..............................] - ETA: 1:16:09 - loss: 2.2913 - acc: 0.0938 - ETA: 1:17:15 - loss: 2.3147 - acc: 0.0781
   2/1563 [..............................] - ETA: 16:07 - loss: 2.3678 - acc: 0.0938
   3/1563 [..............................] - ETA: 55:05 - loss: 2.3232 - acc: 0.0938  
   3/1563 [..............................] - ETA: 51:57 - loss: 2.3185 - acc: 0.1146  
   3/1563 [..............................] - ETA: 51:12 - loss: 2.3179 - acc: 0.1042  
   3/1563 [..............................] - ETA: 11:13 - loss: 2.3504 - acc: 0.0833
   4/1563 [..............................] - ETA: 39:43 - loss: 2.3224 - acc: 0.1094
   4/1563 [..............................] - ETA: 42:09 - loss: 2.3049 - acc: 0.1250
   4/1563 [..............................] - ETA: 39:15 - loss: 2.3089 - acc: 0.1094
   4/1563 [..............................] - ETA: 9:16 - loss: 2.3316 - acc: 0.1016 
   5/1563 [..............................] - ETA: 39:51 - loss: 2.3153 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:58 - loss: 2.3197 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:35 - loss: 2.3148 - acc: 0.1062
   5/1563 [..............................] - ETA: 13:38 - loss: 2.3263 - acc: 0.1062
   6/1563 [..............................] - ETA: 35:48 - loss: 2.3168 - acc: 0.1198

   6/1563 [..............................]   6/1563 [..............................] - ETA: 34:13 - loss: 2.3142 - acc: 0.1198 - ETA: 33:51 - loss: 2.3162 - acc: 0.1042
   6/1563 [..............................] - ETA: 13:54 - loss: 2.3225 - acc: 0.1094
   7/1563 [..............................] - ETA: 30:53 - loss: 2.3181 - acc: 0.1071

   7/1563 [..............................]   7/1563 [..............................] - ETA: 29:32 - loss: 2.3149 - acc: 0.1161 - ETA: 29:13 - loss: 2.3140 - acc: 0.0938
   7/1563 [..............................] - ETA: 12:09 - loss: 2.3174 - acc: 0.1205
   8/1563 [..............................] - ETA: 26:04 - loss: 2.3113 - acc: 0.1133
   8/1563 [..............................] - ETA: 27:15 - loss: 2.3169 - acc: 0.1133
   8/1563 [..............................] - ETA: 10:51 - loss: 2.3152 - acc: 0.1172
...
```

O script prepara mais de 25 épocas ou transmite o conjunto de dados de preparação. Este processo demora cerca de 60 minutos. 

## <a name="retrieve-the-results"></a>Obter os resultados

Quando o script estiver concluído, se tudo tiver corrido bem, a precisão da validação deve ser cerca de 70-75% e o modelo preparado é guardado na partilha de ficheiros em `cifar/saved_models/keras_cifar10_trained_model.h5`. 

A preparação de modelos é normalmente uma parte de um fluxo de trabalho maior. Por exemplo, pode expor o modelo preparado noutra aplicação. Para transferir o modelo preparado localmente, utilize o comando `az storage file download`. 

```azurecli-interactive
az storage file download --path cifar/saved_models/keras_cifar10_trained_model.h5 --share-name myshare --account-name mystorageaccount
```
## <a name="clean-up-resources"></a>Limpar recursos

Após a conclusão da execução das tarefas, uma prática recomendada para poupar custos de computação é reduzir verticalmente todos os clusters para `0 nodes` para que o tempo de inatividade não seja cobrado. Utilize o comando `az batchai cluster resize` seguinte. 

```azurecli-interactive
az batchai cluster resize --name nc6cluster --resource-group batchai.horovod --target 0 --workspace batchaidev
```

Mais tarde, redimensione-o para 1 ou mais nós para executar as suas tarefas. 

Se não planeia utilizar a área de trabalho e a conta de armazenamento no futuro, elimine o grupo de recursos com o comando `az group delete`. A eliminação de um grupo de recursos elimina todos os recursos que fazem parte desse grupo.

```azurecli-interactive
az group delete --name batchai.horovod
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar uma área de trabalho, uma experimentação e um cluster do Batch AI
> * Configurar uma partilha de ficheiros do Azure para entrada e saída
> * Paralelizar um modelo com o Horovod
> * Submeter uma tarefa de preparação
> * Monitorizar a tarefa
> * Obter os resultados de preparação

Para obter exemplos de utilização do Batch AI com diferentes arquiteturas, veja as receitas no GitHub.

> [!div class="nextstepaction"]
> [Receitas do Batch AI](https://github.com/Azure/BatchAI/tree/master/recipes)
