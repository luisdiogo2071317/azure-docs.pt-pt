---
title: Implementar modelos para dispositivos do IoT Edge do serviço Azure Machine Learning | Documentos da Microsoft
description: Saiba como implementar um modelo de serviço do Azure Machine Learning para dispositivos do Azure IoT Edge. Implementar um modelo para um dispositivo do edge permite-lhe classificar os dados no dispositivo em vez de enviá-la para a cloud e aguardando uma resposta.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shipatel
author: shivanipatel
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: dc6119bdca850a71064795a80f3087c15189a2e5
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710006"
---
# <a name="prepare-to-deploy-models-on-iot-edge"></a>Preparar a implementação de modelos do IoT Edge

Neste documento, saiba como utilizar o serviço Azure Machine Learning para preparar um modelo preparado para a implementação para um dispositivo Azure IoT Edge.

Um dispositivo Azure IoT Edge é um Linux ou um dispositivo baseado em Windows que executa o tempo de execução do Azure IoT Edge. Modelos de aprendizagem automática podem ser implementados para estes dispositivos, como módulos do IoT Edge. Implementar um modelo para um dispositivo IoT Edge permite ao dispositivo utilizar o modelo diretamente, em vez de precisar enviar dados para a cloud para processamento. Obtém os tempos de resposta mais rápidos e menos transferência de dados.

Antes de implementar um modelo para um dispositivo do edge, utilize os passos neste documento para preparar o modelo e o dispositivo.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

* Uma área de trabalho de serviço do Azure Machine Learning. Para criar um, utilize os passos no [introdução ao serviço Azure Machine Learning](quickstart-get-started.md) documento.

* Um ambiente de desenvolvimento. Para obter mais informações, consulte a [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md) documento.

* Uma [IoT Hub do Azure](../../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure. 

* Um modelo preparado. Para obter um exemplo de como preparar um modelo, consulte a [preparar um modelo de classificação de imagem com o Azure Machine Learning](tutorial-train-models-with-aml.md) documento. Um modelo com formação prévia está disponível na [ferramentas de ia para o repositório do GitHub do Azure IoT Edge](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

## <a name="prepare-the-iot-device"></a>Preparar o dispositivo de IoT

Para saber como instalar o runtime do IoT e registar o seu dispositivo, siga os passos a [início rápido: implementar o seu primeiro módulo do IoT Edge num dispositivo de Linux x64](../../iot-edge/quickstart-linux.md) documento.

## <a name="register-the-model"></a>Registe o modelo

Módulos do IoT Edge do Azure são baseados em imagens de contentor. Para implementar o seu modelo para um dispositivo IoT Edge, utilize os seguintes passos para registar o seu modelo numa área de trabalho do serviço do Azure Machine Learning e criar uma imagem do Docker. 

1. Inicializar a área de trabalho e carregar o ficheiro Config JSON:

    ```python
    from azureml.core  import Workspace

    #Load existing workspace from the config file info.
    ws  = Workspace.from_config()
    ```    

1. Registe o modelo na área de trabalho. Substitua o texto predefinido com seu caminho de modelo, o nome, a etiquetas e a descrição:

    > [!IMPORTANT]
    > Se utilizou o Azure Machine Learning para preparar o modelo, já pode ser registado na área de trabalho. Se assim for, ignore este passo. Para ver uma lista de modelos registado com esta área de trabalho, utilize `Model.list(ws)`.

    ```python
    from azureml.core.model import Model
    model = Model.register(model_path = "model.pkl", # this path points to the local file
                        model_name = "best_model", # the model gets registered as this name
                        tags = {'attribute': "myattribute", 'classification': "myclassification"},
                        description = "My awesome model",
                        workspace = ws)
    ```    

1. Obter o modelo registado: 

    ```python
    from azureml.core.model import Model

    model_name = "best_model"
    model = Model(ws, model_name)                     
    ```    

## <a name="create-a-docker-image"></a>Crie uma imagem do Docker.

1. Criar uma **script de classificação** com o nome `score.py`. Este ficheiro é utilizado para executar o modelo dentro da imagem. Tem de incluir as seguintes funções:

    * A função `init()`, que, geralmente, carrega o modelo para um objeto global. Esta função só é executada uma vez, quando o contentor do Docker é iniciado. 

    * A função `run(input_data)` utiliza o modelo para prever um valor com base nos dados de entrada. Regra geral, as entradas e as saídas da execução utilizam JSON para a serialização e a desserialização, mas são suportados outros formatos.

    Por exemplo, veja a [tutorial de classificação de imagem](tutorial-deploy-models-with-aml.md#make-script).

1. Criar uma **ficheiro de ambiente** com o nome `myenv.yml`. Este ficheiro é uma especificação de ambiente de Conda e apresenta uma lista de todas as dependências necessárias para o script e o modelo. Por exemplo, veja a [tutorial de classificação de imagem](tutorial-deploy-models-with-aml.md#make-myenv).

1. Configurar a imagem do Docker com o `score.py` e `myenv.yml` ficheiros:
    
    ```python
    from azureml.core.image import Image, ContainerImage
    
    #Image configuration
    image_config = ContainerImage.image_configuration( runtime = "python", 
                           execution_script = "score.py",
                           conda_file = "myenv.yml", 
                           tags = {"attributes", "calssification"},
                           description = "Image that contains my model",
                           
                        )
    ```    

1. Crie a imagem a utilizar a configuração de modelo e de imagem:

    ```python
    image = ContainerImage.create (name = "myimage", 
                           models = [model], #this is the model object
                           image_config = image_config,
                           workspace = ws
                        )
    ```     

    A criação de imagem demora cerca de 5 minutos.

## <a name="get-the-container-registry-credentials"></a>Obter as credenciais do registo de contentor

IoT do Azure tem as credenciais para que o serviço Azure Machine Learning armazena imagens do docker no registo de contentor. Utilize os seguintes passos para obter as credenciais:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/signin/index).

1. Aceda à sua área de trabalho do serviço do Azure Machine Learning e selecione __descrição geral__. Para ir para as definições de registo de contentor, selecione o __Registro__ ligação.

    ![Uma imagem da entrada de registo de contentor](./media/how-to-deploy-to-iot/findregisteredcontainer.png)

1. Uma vez no registo de contentor, selecione **chaves de acesso** e, em seguida, ative o utilizador de administrador.

    ![Uma imagem do ecrã de chaves de acesso](./media/how-to-deploy-to-iot/findaccesskey.png)

1. Guarde os valores para o servidor de início de sessão, o nome de utilizador e palavra-passe. 

   Estas credenciais são necessárias para fornecer o acesso de dispositivo do IoT edge para imagens no seu registo de contentor privado.

## <a name="next-steps"></a>Passos Seguintes

Concluiu preparativos para a implementação. Agora, utilize os passos no [módulos de implementar o Azure IoT Edge do portal do Azure](../../iot-edge/how-to-deploy-modules-portal.md) documento para implementar no seu dispositivo do edge. Quando configurar o __definições de registo__ para o dispositivo, utilize as credenciais que configurou anteriormente.
