---
title: Implementar um modelo do Azure Machine Learning para um dispositivo Azure IoT Edge | Documentos da Microsoft
description: Este documento descreve como os modelos do Azure Machine Learning podem ser implementados para dispositivos do Azure IoT Edge.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 7322b07740d5dec85b6217e122fb262647527c96
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258413"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Implementar um modelo do Azure Machine Learning para um dispositivo Azure IoT Edge

Modelos de Machine Learning do Azure podem ser em contentores como serviços da web baseadas no Docker. O Azure IoT Edge permite-lhe implementar contentores remotamente nos dispositivos. Utilize estes serviços em conjunto para executar seus modelos na periferia para tempos de resposta mais rápidos e menos transferência de dados. 

Scripts adicionais e as instruções podem ser encontradas no [ferramentas de IA do Azure IoT Edge](https://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Operacionalizar o modelo

Módulos do IoT Edge do Azure são baseados em imagens de contentor. Para implementar o seu modelo de Machine Learning para um dispositivo IoT Edge, terá de criar uma imagem do Docker.

Operacionalizar o seu modelo ao seguir as instruções em [do Azure Machine Learning modelo de gestão de implementação do serviço Web](model-management-service-deploy.md) para criar uma imagem do Docker com o seu modelo.

## <a name="deploy-to-azure-iot-edge"></a>Implementar o Azure IoT Edge

Assim que tiver a imagem do seu modelo, pode implementá-la para qualquer dispositivo Azure IoT Edge. Todos os modelos de Machine Learning podem executar em dispositivos IoT Edge. 

### <a name="set-up-an-iot-edge-device"></a>Configurar um dispositivo IoT Edge

Utilize a documentação do Azure IoT Edge para preparar um dispositivo. 

1. [Registar um dispositivo hub IoT do Azure](../../iot-edge/how-to-register-device-portal.md). A saída dos processos desta é uma cadeia de ligação que pode utilizar para configurar o dispositivo físico. 
2. Instalar o runtime do IoT Edge no dispositivo físico e configurá-lo com uma cadeia de ligação. Pode instalar o tempo de execução num [Windows](../../iot-edge/how-to-install-iot-edge-windows-with-windows.md) ou [Linux](../../iot-edge/how-to-install-iot-edge-linux.md) dispositivos.  


### <a name="find-the-machine-learning-container-image-location"></a>Encontrar a localização de imagem de contentor do Machine Learning
Tem a localização da sua imagem de contentor do Machine Learning. Para localizar a localização de imagem do contentor:

1. Inicie sessão no [portal do Azure](http://portal.azure.com/).
2. Na **Azure Container Registry**, selecione o registo que quer inspecionar.
3. No Registro, clique em **repositórios** para ver uma lista de todos os repositórios e suas imagens.

Enquanto estiver olhando para o registo de contentor no portal do Azure, obter as credenciais de registo de contentor. Estas credenciais têm de ser dado ao dispositivo IoT Edge, para que ele pode extrair a imagem do seu registo privado. 

1. No registo de contentores, clique em **chaves de acesso**. 
2. **Ativar** o utilizador de administrador, se ainda não estiver. 
3. Guarde os valores para **servidor de início de sessão**, **nome de utilizador**, e **palavra-passe**. 

### <a name="deploy-the-container-image-to-your-device"></a>Implementar a imagem de contentor no seu dispositivo

Com a imagem de contentor e as credenciais de registo de contentor, está pronto para implementar o modelo de machine learning para o seu dispositivo IoT Edge. 

Siga as instruções em [módulos de implementar o IoT Edge do portal do Azure](../../iot-edge/how-to-deploy-modules-portal.md) para iniciar o seu modelo no seu dispositivo IoT Edge. 











