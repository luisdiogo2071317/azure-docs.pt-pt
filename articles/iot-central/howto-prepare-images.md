---
title: Carregar imagens para a aplicação Azure IoT Central | Documentos da Microsoft
description: Como um construtor, saiba como preparar e carregar imagens para a aplicação Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 10/31/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 9d4de2a8be13718f7989f064b76b586407ca3a48
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198614"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Preparar e carregar imagens para a sua aplicação do Azure IoT Central

Este artigo descreve como, como um construtor, pode personalizar a aplicação Azure IoT Central através do carregamento de imagens personalizadas. Por exemplo, pode personalizar um dashboard do dispositivo com uma imagem do dispositivo.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

1. Uma aplicação do Azure IoT Central. Para obter mais informações, veja [criar um início rápido da aplicação](quick-deploy-iot-central.md).
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
| Gestor de Aplicações | 268 x 160 px |
| Modelo de dispositivos | 64 x 64 px |
| Home page e mosaicos do dashboard | O mosaico de tamanho menor é 200 x 200 px, mosaicos maior podem ser o quadrado ou retangulares múltiplos de mosaicos pequenos. Por exemplo, 200 x 400 px, 400 x 200 px ou 400 x 400 px |

Para uma melhor visualização no aplicativo, deve criar imagens que correspondem às dimensões mostradas na tabela anterior.

## <a name="upload-the-images"></a>Carregar as imagens

As secções seguintes descrevem como carregar as imagens em diferentes locais:

### <a name="application-manager"></a>Gestor de aplicações

Para carregar uma imagem no **Gerenciador de aplicativos**, navegue para o **as definições da aplicação** página no **administração** secção. Tem de ser um administrador para concluir essa tarefa:

![Carregar a imagem de aplicação](media/howto-prepare-images/uploadapplicationmanager.png)

Clique no mosaico de imagem de aplicação para carregar a sua imagem preparada (268 x 160 px) do seu computador local.

### <a name="home-page"></a>Página de boas-vindas

Para carregar uma imagem na home page, navegue para o **home page** da sua aplicação e clique em **editar**. Tem de ser um construtor para concluir essa tarefa:

![Carregar a imagem da página inicial](media/howto-prepare-images/uploadhomepage.png)

Na imagem de configurar, clique no mosaico de imagem para carregar a sua imagem preparada do seu computador local. O mosaico de tamanho menor é 200 x 200 px, mosaicos maior podem ser o quadrado ou retangulares múltiplos de mosaicos pequenos. Por exemplo, 200 x 400 px, 400 x 200 px ou 400 x 400 px.

**Guardar** a imagem carregada. Pode redimensioná-la no modo de edição. Clique em **feito** quando terminar. 

### <a name="device-template"></a>Modelo de dispositivos

Para carregar uma imagem num modelo de dispositivo, navegue até **Device Explorer**, escolha o modelo de dispositivo e, em seguida, um dispositivo. Tem de ser um construtor para concluir essa tarefa:

![Carregar a imagem de modelo do dispositivo](media/howto-prepare-images/uploaddevicetemplate.png)

Clique no mosaico de imagem para carregar a sua imagem preparada (64 x 64 px) do seu computador local. 

### <a name="device-dashboard"></a>Dashboard do dispositivo

Para carregar uma imagem num dashboard do dispositivo, navegue até **Device Explorer**, escolha o modelo de dispositivo e, em seguida, um dispositivo. Em seguida, escolha o **Dashboard** página e clique em **Editar modelo**. Tem de ser um construtor para concluir essa tarefa:

![Carregar a imagem do dashboard de dispositivo](media/howto-prepare-images/uploaddevicedashboard.png)

Em configurar a imagem, clique no mosaico de imagem e, em seguida, escolha o ficheiro para carregar a partir do seu computador local. O mosaico de tamanho menor é 200 x 200 px, mosaicos maior podem ser o quadrado ou retangulares múltiplos de mosaicos pequenos. Por exemplo, 200 x 400 px, 400 x 200 px ou 400 x 400 px.

**Guardar** a imagem carregada. Pode redimensionar e reposicionar ele enquanto estiver no modo de edição. Clique em **feito** quando terminar.

### <a name="device-set-dashboard"></a>Dashboard de conjunto de dispositivos

Para carregar uma imagem num dashboard dispositivos conjunto, navegue até **conjuntos de dispositivo** e escolha o conjunto de dispositivo e, em seguida, um dispositivo. Em seguida, escolha o **Dashboard** página e clique em **editar**:

![Carregar a imagem do dashboard de conjunto de dispositivo](media/howto-prepare-images/uploaddevicesetdashboard.png)

Na imagem de configurar, clique no mosaico de imagem para carregar a sua imagem preparada do seu computador local. O mosaico de tamanho menor é 200 x 200 px, mosaicos maior podem ser o quadrado ou retangulares múltiplos de mosaicos pequenos. Por exemplo, 200 x 400 px, 400 x 200 px ou 400 x 400 px.

**Guardar** a imagem carregada. Pode redimensionar e reposicionar ele enquanto estiver no modo de edição. Clique em **feito** quando terminar.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como preparar e carregar imagens para a aplicação Azure IoT Central, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Gerir dispositivos na sua aplicação do Azure IoT Central](howto-manage-devices.md)