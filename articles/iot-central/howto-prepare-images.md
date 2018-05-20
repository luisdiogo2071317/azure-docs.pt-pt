---
title: Carregar imagens para a aplicação do Azure IoT Central | Microsoft Docs
description: Como um construtor, saiba como preparar e carregar imagens para a aplicação do Azure IoT Central.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: a43f2dd780604235ada1d8e3ae8a20563042fbaa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Preparar e carregar imagens para a aplicação do Azure IoT Central

Este artigo descreve como, como um construtor, pode personalizar a aplicação do Microsoft Azure IoT Central através do carregamento de imagens personalizadas. Por exemplo, pode personalizar um dashboard do dispositivo com uma imagem do dispositivo.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

1. Uma aplicação do Azure IoT Central. Para obter mais informações, consulte [criar a sua aplicação Central do Azure IoT](howto-create-application.md).
1. Uma ferramenta de dimensionamento e o redimensionamento de ficheiros de imagens.

## <a name="choose-where-to-use-custom-images"></a>Selecione onde utilizar imagens personalizadas

Pode adicionar imagens personalizadas para as seguintes localizações por uma aplicação do Azure IoT Central:

* O **Gestor de aplicação** página

    ![Imagem na página do Gestor de aplicações](media/howto-prepare-images/applicationmanager.png)

* A home page

    ![Imagem na home page](media/howto-prepare-images/homepage.png)

* Um modelo de dispositivo

    ![Imagem de modelo de dispositivo](media/howto-prepare-images/devicetemplate.png)

* Um mosaico no dashboard de um dispositivo

    ![Imagem no mosaico do dispositivo](media/howto-prepare-images/devicetile.png)

* Um mosaico no dashboard de conjunto de um dispositivo

    ![Imagem no mosaico do conjunto de dispositivos](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Preparar as imagens

Em todos os quatro localizações, pode utilizar imagens PNG, GIF ou JPEG.

A tabela seguinte resume os tamanhos de imagem, que pode utilizar:

| Localização | Tamanhos |
| -------- | ------ |
| **Gestor de aplicações** | 268 x 160 px |
| Modelo de dispositivo | 64 x 64 px |
| Home page e mosaicos do dashboard | O mosaico tamanho mais pequeno é 200 x 200 px, maior mosaicos podem ser quadrados ou retangular múltiplos de mosaicos pequenos. Por exemplo, 200, 400 px, 400 x 200 px ou 400 x 400 px |

Para a melhor visualização na aplicação, deve criar imagens que correspondem às dimensões mostradas na tabela anterior.

## <a name="upload-the-images"></a>Carregar as imagens

As secções seguintes descrevem como carregar as imagens para utilizar em diferentes localizações:

### <a name="application-manager"></a>Gestor de aplicações

Para carregar uma imagem a utilizar o **Gestor de aplicações**, navegue para o **definições da aplicação** página no **administração** secção. Tem de ser um administrador para concluir esta tarefa:

![Carregar imagem de aplicação](media/howto-prepare-images/uploadapplicationmanager.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

### <a name="home-page"></a>Home page

Para carregar uma imagem a utilizar na home page, navegue para o **home page** da sua aplicação e o modo de estrutura de comutador no. Tem de ser um construtor para concluir esta tarefa:

![Carregar imagem da página inicial](media/howto-prepare-images/uploadhomepage.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

Depois da imagem carrega, pode redimensionar enquanto mudado modo de estrutura.

### <a name="device-template"></a>Modelo de dispositivo

Para carregar uma imagem a utilizar num modelo de dispositivo, navegue até à **Explorador de dispositivo**, escolha o modelo de dispositivo e, em seguida, um dispositivo e mudar o modo de estrutura. Tem de ser um construtor para concluir esta tarefa:

![Carregar imagem de modelo do dispositivo](media/howto-prepare-images/uploaddevicetemplate.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

### <a name="device-dashboard"></a>Dashboard de dispositivo

Para carregar uma imagem a utilizar um dashboard de dispositivo, navegue até à **Explorador de dispositivo**, escolha o modelo de dispositivo e, em seguida, um dispositivo. Em seguida, escolha o **Dashboard** modo de estrutura de página e o comutador no. Tem de ser um construtor para concluir esta tarefa:

![Carregar imagem de dashboard do dispositivo](media/howto-prepare-images/uploaddevicedashboard.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

Depois da imagem carrega, pode redimensionar e reposicioná-lo ao **modo de estrutura** está ativado.

### <a name="device-set-dashboard"></a>Dashboard de conjunto de dispositivos

Para carregar uma imagem a utilizar um dashboard de conjunto de dispositivos, navegue até à **conjuntos de dispositivo** e escolha o conjunto de dispositivos e, em seguida, um dispositivo. Em seguida, escolha o **Dashboard** página e o comutador **modo de estrutura** em:

![Carregar imagem de dashboard do conjunto de dispositivos](media/howto-prepare-images/uploaddevicesetdashboard.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

Depois da imagem carrega, pode redimensionar e reposicionar-enquanto mudado modo de estrutura.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como preparar e carregar imagens para a aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Gerir dispositivos na sua aplicação do Azure IoT Central](howto-manage-devices.md)