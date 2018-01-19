---
title: "Como utilizar o Azure CLI 2.0 e a extensão de IoT para gerir os serviços de aprovisionamento de dispositivos | Microsoft Docs"
description: "Saiba como utilizar o Azure CLI 2.0 e a extensão de IoT para gerir os serviços de aprovisionamento de dispositivos"
services: iot-dps
keywords: 
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 674245f1e284e7308474fed0f6c53b350ec1c819
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Como utilizar o Azure CLI 2.0 e a extensão de IoT para gerir os serviços de aprovisionamento de dispositivos

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) é um open source entre a ferramenta de linha de comandos de plataforma para gerir recursos do Azure, tais como o limite de IoT. Azure CLI 2.0 está disponível no Windows, Linux e MacOS. 2.0 do CLI do Azure permite-lhe gerir recursos, instâncias de serviço aprovisionamento de dispositivos e ligado hubs a Box do IoT Hub do Azure.

A extensão de IoT otimiza Azure CLI 2.0 com funcionalidades como a gestão de dispositivos e a capacidade total do limite de IoT.

Neste tutorial, primeiro conclua os passos para configurar o Azure CLI 2.0 e a extensão de IoT. Em seguida, irá aprender a executar os comandos da CLI para efetuar operações de serviço aprovisionamento básica do dispositivo. 

## <a name="installation"></a>Instalação 

### <a name="step-1---install-python"></a>Passo 1 – instalar Python

[Python 2.7 x ou Python 3](https://www.python.org/downloads/) é necessária.

### <a name="step-2---install-azure-cli-20"></a>Passo 2 - instalar a CLI do Azure 2.0

Siga o [instruções de instalação](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) ao configurar o Azure CLI 2.0 no seu ambiente. No mínimo, a versão 2.0 do Azure CLI tem de ser 2.0.24 ou superior. Utilize `az –version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comando Knack. Uma forma simples de instalar o Windows é para transferir e instalar o [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Passo 3 - extensão de instalação IoT

[A extensão de IoT ficheiro Leia-me](https://github.com/Azure/azure-iot-cli-extension) descreve várias formas de instalar a extensão. A forma mais simples consiste em executar `az extension add --name azure-cli-iot-ext`. Após a instalação, pode utilizar `az extension list` para validar as extensões atualmente instaladas ou `az extension show --name azure-cli-iot-ext` para ver detalhes sobre a extensão de IoT. Para remover a extensão, pode utilizar `az extension remove --name azure-cli-iot-ext`.


## <a name="basic-device-provisioning-service-operations"></a>Básicas de operações do serviço de aprovisionamento de dispositivos
O exemplo mostra como iniciar sessão na sua conta do Azure, crie um grupo de recursos do Azure (um contentor que retém recursos relacionados para uma solução do Azure), criar um IoT Hub, criar um dispositivo de aprovisionamento servie, listar os serviços de aprovisionamento de dispositivos existente e Crie um IoT hub ligado com comandos da CLI. 

Conclua os passos de instalação descritos anteriormente antes de começar. Se não tiver uma conta do Azure ainda, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?v=17.39a) hoje. 


### <a name="1-log-in-to-the-azure-account"></a>1. Inicie sessão para a conta do Azure
  
    az login

![início de sessão][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Criar um grupo de recursos IoTHubBlogDemo no eastus

    az group create -l eastus -n IoTHubBlogDemo

![Criar grupo de recursos][2]


### <a name="3-create-two-device-provisioning-services"></a>3. Criar dois serviços de aprovisionamento de dispositivos

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Criar DPS][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Listar todos os dispositivos de existente serviços sob este grupo de recursos de aprovisionamento

    az iot dps list --resource-group IoTHubBlogDemo

![Lista DPS][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Criar um blogDemoHub do IoT Hub sob o grupo de recursos criado de novo

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Criar o IoT Hub][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Associar um IoT Hub existente para um dispositivo de serviço de fornecimento

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Hub de ligação][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Inscrever o dispositivo
> * Iniciar o dispositivo
> * Certifique-se de que o dispositivo está registado

Avançar para o próximo tutorial para saber como aprovisionar vários dispositivos em hubs com balanceamento de carga. 

> [!div class="nextstepaction"]
> [Aprovisionar dispositivos entre os hubs IoT do balanceamento de carga](./tutorial-provision-multiple-hubs.md)
