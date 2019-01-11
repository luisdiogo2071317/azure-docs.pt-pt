---
title: Configurar a home page da sua aplicação do Azure IoT Central | Documentos da Microsoft
description: Como um construtor, saiba como configurar a home page da sua aplicação do Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a03ac0ef66f4ffdce53d0bd2a35839bbe1615d0b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54199090"
---
# <a name="configuring-homepage"></a>Configurar a home page

A home page é a página que carrega quando os utilizadores que têm acesso à aplicação, navegue até ao URL da aplicação. Se tiver selecionado o "Exemplo de Contoso" ou "Exemplo Devkits" modelos de aplicativos durante a criação de seu aplicativo, seu aplicativo irá ter previamente definido Home Pages. Se por outro lado, tiver selecionado o modelo de aplicativo de "Aplicativo personalizado", sua home page estará em branco.

Por exemplo, segue-se a página inicial para aplicativos com base no modelo de "Exemplo Contoso". Para personalizar a home page para a sua aplicação, primeiro selecione **editar** no canto superior direito. 

![Home page para aplicações com base no modelo de "Exemplo Contoso"](media/howto-configure-homepage/image1.png)

Selecionando **editar**, abrirá a biblioteca do dashboard num painel à esquerda. Existem muitos tipos de mosaicos e primitivos de dashboard que podem ser adicionados para personalizar a sua home page.

![Biblioteca do dashboard](media/howto-configure-homepage/image2.png)

Por exemplo, pode adicionar um **definições e propriedades** mosaico para mostrar uma seleção dos valores atuais das definições e propriedades. Para tal, primeiro selecione um **modelo de dispositivo** , em seguida, selecione um **dispositivo instância**. Depois que fornecem o mosaico a um título e selecione um **definição** ou uma **propriedade** para apresentar. Neste caso, selecionámos **definir a temperatura**. Clicar **feito** fará com que este mosaico apareça na home page.

![Formulário de "Configurar os detalhes do dispositivo" com detalhes para as definições e propriedades](media/howto-configure-homepage/image3.png)

Agora quando um operador visualiza a home page, pode ver este mosaico que apresenta as propriedades ou as definições do dispositivo:

![Separador "Dashboard" com as definições apresentadas e propriedades para o mosaico](media/howto-configure-homepage/image4.png)

Experimente os vários outros tipos de blocos na biblioteca para descobrir como pode personalizar a home page do seu aplicativo ainda mais.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como configurar a sua home page do Azure IoT Central, pode:

> [!div class="nextstepaction"]
> [Saiba como preparar e carregar imagens](howto-prepare-images.md)
