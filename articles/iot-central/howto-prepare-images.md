---
title: Carregar imagens para a aplicação Azure IoT Central | Documentos da Microsoft
description: Como um construtor, saiba como preparar e carregar imagens para a aplicação Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 18c44a3d91a4964d054c8e142394da7d69772ed0
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960706"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Preparar e carregar imagens para a sua aplicação do Azure IoT Central

Este artigo descreve como, como um construtor, pode personalizar a aplicação do Microsoft Azure IoT Central através do carregamento de imagens personalizadas. Por exemplo, pode personalizar um dashboard do dispositivo com uma imagem do dispositivo.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

1. Uma aplicação do Azure IoT Central. Para obter mais informações, consulte a [criar um guia de introdução do aplicativo](quick-deploy-iot-central.md).
1. Uma ferramenta para dimensionamento e redimensionamento de ficheiros de imagens.

## <a name="choose-where-to-use-custom-images"></a>Escolha onde pretende utilizar imagens personalizadas

Pode adicionar imagens personalizadas para as seguintes localizações num aplicativo do Azure IoT Central:

* O **Gerenciador de aplicativos** página

    ![Imagem na página do Gestor de aplicações](media/howto-prepare-images/applicationmanager.png)

* A home page

    ![Imagem na home page](media/howto-prepare-images/homepage.png)

* Um modelo de dispositivo

    ![Imagem no modelo de dispositivo](media/howto-prepare-images/devicetemplate.png)

* Um mosaico num dashboard do dispositivo

    ![Imagem no mosaico do dispositivo](media/howto-prepare-images/devicetile.png)

* Um mosaico num dashboard dispositivos conjunto

    ![Imagem no mosaico do conjunto de dispositivo](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Preparar as imagens

Em todas as localizações de quatro, pode usar imagens PNG, GIF ou JPEG.

A tabela seguinte resume os tamanhos de imagem, que pode usar:

| Localização | Tamanhos |
| -------- | ------ |
| **Gestor de aplicações** | 268 x 160 px |
| Modelo de dispositivo | 64 x 64 px |
| Home page e mosaicos do dashboard | O mosaico de tamanho menor é 200 x 200 px, mosaicos maior podem ser o quadrado ou retangulares múltiplos de mosaicos pequenos. Por exemplo, 200 x 400 px, 400 x 200 px ou 400 x 400 px |

Para uma melhor visualização no aplicativo, deve criar imagens que correspondem às dimensões mostradas na tabela anterior.

## <a name="upload-the-images"></a>Carregar as imagens

As secções seguintes descrevem como carregar as imagens para utilizar em diferentes locais:

### <a name="application-manager"></a>Gestor de aplicações

Para carregar uma imagem a utilizar no **Gerenciador de aplicativos**, navegue para o **as definições da aplicação** página no **administração** secção. Tem de ser um administrador para concluir essa tarefa:

![Carregar a imagem de aplicação](media/howto-prepare-images/uploadapplicationmanager.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

### <a name="home-page"></a>Página de boas-vindas

Para carregar uma imagem a utilizar na home page, navegue para o **home page** do seu aplicativo e alternar o modo de design no. Tem de ser um construtor para concluir essa tarefa:

![Carregar a imagem da página inicial](media/howto-prepare-images/uploadhomepage.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

Depois de carrega a imagem, pode redimensioná-la enquanto o modo de design está ativado.

### <a name="device-template"></a>Modelo de dispositivo

Para carregar uma imagem a utilizar num modelo de dispositivo, navegue até **Device Explorer**, escolha o modelo de dispositivo e, em seguida, um dispositivo e alternar o modo de design. Tem de ser um construtor para concluir essa tarefa:

![Carregar a imagem de modelo do dispositivo](media/howto-prepare-images/uploaddevicetemplate.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

### <a name="device-dashboard"></a>Dashboard do dispositivo

Para carregar uma imagem a utilizar num dashboard do dispositivo, navegue até **Device Explorer**, escolha o modelo de dispositivo e, em seguida, um dispositivo. Em seguida, escolha o **Dashboard** modo de design de página e o comutador no. Tem de ser um construtor para concluir essa tarefa:

![Carregar a imagem do dashboard de dispositivo](media/howto-prepare-images/uploaddevicedashboard.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

Depois de carrega a imagem, pode redimensionar e reposicioná-lo ao **modo de Design** está ativado.

### <a name="device-set-dashboard"></a>Dashboard de conjunto de dispositivos

Para carregar uma imagem a utilizar num dashboard dispositivos conjunto, navegue até **conjuntos de dispositivo** e escolha o conjunto de dispositivo e, em seguida, um dispositivo. Em seguida, escolha o **Dashboard** página e o comutador **modo de Design** em:

![Carregar a imagem do dashboard de conjunto de dispositivo](media/howto-prepare-images/uploaddevicesetdashboard.png)

Clique na imagem de carregamento e, em seguida, escolha o ficheiro para carregar a partir do seu computador local.

Depois de carrega a imagem, pode redimensionar e reposicioná-lo enquanto o modo de design está ativado.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como preparar e carregar imagens para a aplicação Azure IoT Central, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Gerir dispositivos na sua aplicação do Azure IoT Central](howto-manage-devices.md)